# Spark 元数据:结构化流中的提交清理问题

> 原文：<https://blog.devgenius.io/spark-metadata-commit-cleanup-issue-in-structured-streaming-c6f8d69baf38?source=collection_archive---------6----------------------->

**简介:**

这个故事是为那些仍在使用 Spark 2.3 并在生产中运行结构化流作业的开发人员而写的。伙计们，小心点。你可能已经做了各种各样的优化和调整，但是仍然发现工作有问题。如果您正在处理大量数据(每小时几 TB 到几百 TB ),并且看到间歇性延迟，那么您的工作很可能面临元数据压缩问题。这里我来解释一下问题。

**问题详情:**

[https://issues.apache.org/jira/browse/SPARK-24295](https://issues.apache.org/jira/browse/SPARK-24295)

检查点是在 Spark 中实现*语义的一种方式。流式作业将输入和输出批处理信息保存在检查点中，并按批处理提交文件。流式作业全天候运行，可能会运行数年，导致处理大量数据，文件大小会随着时间的推移而增长，最高可达数十 GB。**压缩文件存储流式作业从开始日期开始处理和生成的所有文件的详细信息。**对，你没看错，*从申请开始*。*

*提交文件压缩(_Spark_Metadata)在驱动程序上每 10 批执行一次(从 0 开始)。这是一个整体过程，根据压缩文件的大小，最多需要 10-15 分钟。它大大降低了结构化流作业的性能，因为当驱动程序压缩文件时，所有工作节点都不做任何事情，并且作业完全闲置，浪费了资源。有一个控制压缩间隔的 spark config:***spark . SQL . streaming . filesink . log . compact interval****

*默认情况下，此配置的值为 10，即每 10 个微批次后，提交文件压缩就会发生。它将最后 9 个批次的增量添加到前面的 ***中。压缩*** 文件并新建一个 ***。压缩*文件和**文件。你可以根据你的需要改变它的频率。*

***解决方案:***

*您唯一的解决方案是编写一个代码来清理 _Spark_Metadata 目录中的元数据。压缩文件是 JSON 格式的，并且还跟踪文件的时间戳和其他信息。*

*可以采取以下方法:*

*   *启动 spark 作业，读取 _Spark_Metadata 的最新`.comapct` JSON 文件*
*   *根据保留期过滤掉旧文件的所有记录。*
*   *保存旧的压缩文件以备备份。*
*   *将新的粉碎压缩文件移动到 *_Spark_Metadata* 目录。保持相同的文件结构和文件名。*
*   *在我们更新元数据压缩文件时，请确保 spark 流作业没有运行。*

*下面火花码接最新**。compact**file from _ spark _ metadata dir，根据配置驱动的参数 retentionPeriod 清除元数据，并重写丢弃的。压缩同一路径下的文件。这已经在 ORC 文件和 HDFS 接收器的生产中进行了测试。如果您的触发时间较短(即 60 秒或更长)，您可以每月运行一次此应用程序作为辅助维护应用程序。或者，如果您的 triggertime 少于 60 秒，并且您的应用程序处理大量数据，那么最好每周进行一次清理。您可以将此代码添加到您的流媒体应用程序的开头，以便在重新启动时，首先进行清理。*

```
*import org.apache.hadoop.fs._
import org.apache.spark.sql._
import org.apache.spark.sql.execution.streaming._
import org.json4s._
import org.json4s.jackson.JsonMethods._

object MetadataRemover extends App {

  val *spark* = SparkSession
    .*builder* .master(ApplicationConfig.*sparkMaster*)
    .appName(ApplicationConfig.*sparkAppName*)
    .getOrCreate()
  *spark*.sparkContext.setLogLevel("ERROR")

  val *sc* = *spark*.sparkContext
  */**
   * regex to find last compact file
   */* val *file_pattern* = """(hdfs://.*/_spark_metadata/\d+\.compact)""".r.unanchored
  val *fs* = FileSystem.*get*(*sc*.hadoopConfiguration)

  */**
   * implicit hadoop RemoteIterator convertor
   */* implicit def convertToScalaIterator[T](underlying: RemoteIterator[T]): Iterator[T] = {
    case class wrapper(underlying: RemoteIterator[T]) extends Iterator[T] {
      override def hasNext: Boolean = underlying.hasNext

      override def next: T = underlying.next
    }
    wrapper(underlying)
  }

  */**
   * delete file or folder recursively
   */* def removePath(dstPath: String, fs: FileSystem): Unit = {
    val path = new Path(dstPath)
    if (fs.exists(path)) {
      *println*(s"deleting **$**{dstPath}...")
      fs.delete(path, true)
    }
  }

  */**
   * remove json entries older than* `*days*` *from compact file
   * preserve* `*v1*` *at the head of the file
   * re write the small file back to the original destination
   */* def compact(file: Path, days: Int) = {
    val ttl = new java.util.Date().getTime - java.util.concurrent.TimeUnit.*DAYS*.toMillis(days)
    val compacted_file = s"/tmp/**$**{file.getName.toString}"
    // If this path already exists : then it will be removed
    *removePath*(compacted_file, *fs*)
    val lines = *sc*.textFile(file.toString)
    val reduced_lines = lines.mapPartitions({
      p =>
        implicit val formats = DefaultFormats
        p.collect({
          case "v1" => "v1"
          case x if {
            parse(x).extract[SinkFileStatus].modificationTime > ttl
          } => x
        })
    }).coalesce(1)
    *println*(s"removing **$**{lines.count - reduced_lines.count} lines from **$**{file.toString}...")
    reduced_lines.saveAsTextFile(compacted_file)
    FileUtil.*copy*(*fs*, new Path(compacted_file + "/part-00000"), *fs*, file, false, *sc*.hadoopConfiguration)
    *removePath*(compacted_file, *fs*)
  }

  */**
   * get last compacted files if exists:
   */* def getLastCompactFile(path: Path) = {
    *fs*.listFiles(path, true).toList.sortBy(_.getModificationTime).reverse.collectFirst({
      case x if (*file_pattern*.findFirstMatchIn(x.getPath.toString).isDefined) =>
        x.getPath
    })
  }

  val *landingDir* = ApplicationConfig.*landingPath* val *retentionPeriod* = ApplicationConfig.*retentionPeriod* val *metadataDir* = new Path(s"**$***landingDir*/_spark_metadata")
  *getLastCompactFile*(*metadataDir*).map(x => *compact*(x, *retentionPeriod*))

}*
```