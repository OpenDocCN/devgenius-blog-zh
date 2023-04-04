# 科特林的样本反应堆 Kafka 生产者和消费者

> 原文：<https://blog.devgenius.io/sample-reactor-kafka-producer-and-consumer-with-spring-boot-in-kotlin-145b5f46e59e?source=collection_archive---------2----------------------->

创建了[sample-reactor-kafka-springboot](https://github.com/seetharamani/sample-reactor-kafka-springboot)存储库来展示如何使用 project-reactor 和 spring boot 启动 Kafka 消费者和生产者

![](img/26dc039df058830d1773dfd755b0fcca.png)

@nasawebb

# 发布者配置

这里需要注意的几件事是

*   我已经用`String`数据类型的键和`ByteArray`数据类型的值配置了发布者，从而配置了`SenderOptions`中的`StringSerializer`和`ByteArraySerializer`
*   这些配置只是为了举例。根据您的使用情况和负载相应地调整它们
*   只有在构造 ProducerRecord 时，才会构造 Publisher 主题。`KafkaSender`中未配置主题

```
@Component
@ConfigurationProperties("kafka")
class PublisherConfiguration {
    lateinit var kafkaBootstrapServers: String
    lateinit var clientId: String
    lateinit var publisherTopic: String
    lateinit var acks: String

    var maxRequestSize: Int = 0
    var retries: Int = 0
    var lingerMs: Int = 0
    var reconnectBackoffMs: Int = 0
    var retryBackoffMs: Int = 0
    var batchSize: Int = 16384

    // this is default, in general should be much less, 
    // but > replica.lag.time.max.ms
    var requestTimeoutMs: Int = 30 * 1000

    private fun getSenderOptions(): 
SenderOptions<String, ByteArray> {
        val properties = Properties()
        properties[ProducerConfig.BOOTSTRAP_SERVERS_CONFIG] = kafkaBootstrapServers
        properties[ProducerConfig.CLIENT_ID_CONFIG] = clientId
        properties[ProducerConfig.ACKS_CONFIG] = acks
        properties[ProducerConfig.RETRIES_CONFIG] = retries
        properties[ProducerConfig.LINGER_MS_CONFIG] = lingerMs
        properties[ProducerConfig.MAX_REQUEST_SIZE_CONFIG] = maxRequestSize
        properties[ProducerConfig.RECONNECT_BACKOFF_MS_CONFIG] = reconnectBackoffMs
        properties[ProducerConfig.RETRY_BACKOFF_MS_CONFIG] = retryBackoffMs
        properties[ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG] = StringSerializer::class.java
        properties[ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG] = ByteArraySerializer::class.java
        properties[ProducerConfig.BATCH_SIZE_CONFIG] = batchSize
        properties[ProducerConfig.REQUEST_TIMEOUT_MS_CONFIG] = requestTimeoutMs

        log.info("Kafka publisher properties configured : {}", properties)

        return SenderOptions.create(properties)
    }

    @Bean
    fun kafkaSender(
           publisherConfiguration: PublisherConfiguration
        ): KafkaSender<String, ByteArray> { return KafkaSender.create(
             publisherConfiguration.getSenderOptions()
             )
    }

    companion object {
        private val log = LoggerFactory
                 .getLogger(PublisherConfiguration::class.java)
    }
}
```

# 接收器配置

这里需要注意的几件事是

*   确保为您的接收器设置群组 id
*   我已经用`String`数据类型的键和`ByteArray`数据类型的值配置了发布者，因此配置了`ReceiverOptions`中的`StringDeserializer`和`ByteArrayDeserializer`
*   这些配置只是为了举例。根据您的使用情况和负载相应地调整它们
*   消费主题作为`KafkaReceiver`的一部分进行配置

```
@Component
@ConfigurationProperties("kafka")
class ReceiverConfiguration {

    lateinit var kafkaBootstrapServers: String
    lateinit var groupId: String
    lateinit var clientId: String
    lateinit var receiverTopic: String
    lateinit var autoOffsetReset: String

    var concurrency: Int = 1
    var maxPollTimeoutMs: Int = 10000
    var backtrackTimeSeconds: Int = 15 * 60

    var retryBackoffMs: Int = 1000
    var maxPollRecords: Int = 250
    var maxPollIntervalMs: Int = 300_000
    var sessionTimeoutMs: Int = 10_000
    var heartbeatIntervalMs: Int = 3000
    var requestTimeoutMs: Int = 30_000
    var autoCommitIntervalMs: Int = 5000
    var commitBatchSize: Int = 0
    var commitIntervalMs: Long = 5000L

    fun getReceiverOptions(): ReceiverOptions<String?, ByteArray> {
        val properties = Properties()
        properties[ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG] = kafkaBootstrapServers
        properties[ConsumerConfig.GROUP_ID_CONFIG] = groupId
        properties[ConsumerConfig.CLIENT_ID_CONFIG] = clientId
        properties[ConsumerConfig.RETRY_BACKOFF_MS_CONFIG] = retryBackoffMs
        properties[ConsumerConfig.AUTO_OFFSET_RESET_CONFIG] = autoOffsetReset
        properties[ConsumerConfig.MAX_POLL_RECORDS_CONFIG] = maxPollRecords
        properties[ConsumerConfig.MAX_POLL_INTERVAL_MS_CONFIG] = maxPollIntervalMs
        properties[ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG] = sessionTimeoutMs
        properties[ConsumerConfig.HEARTBEAT_INTERVAL_MS_CONFIG] = heartbeatIntervalMs
        properties[ConsumerConfig.REQUEST_TIMEOUT_MS_CONFIG] = requestTimeoutMs
        properties[ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG] = StringDeserializer::class.java
        properties[ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG] = ByteArrayDeserializer::class.java
        properties[ConsumerConfig.AUTO_COMMIT_INTERVAL_MS_CONFIG] = autoCommitIntervalMs

        log.info("Kafka receiver properties configured : {}", properties)

        return ReceiverOptions.create<String?, ByteArray>(properties)
            .commitInterval(Duration.ofMillis(commitIntervalMs))
            .commitBatchSize(commitBatchSize)
    }

    @Bean
    fun kafkaReceiver(receiverConfiguration: ReceiverConfiguration): KafkaReceiver<String, ByteArray> {
        return KafkaReceiver.create<String, ByteArray>(
            receiverConfiguration.getReceiverOptions().subscription(
                listOf(
                    receiverTopic
                )
            )
        )
    }

    companion object {
        private val log = LoggerFactory.getLogger(ReceiverConfiguration::class.java)
    }
}
```

# 启动和停止消费者

`kafkaReceiver.receive().subscribe()`启动消费者，将它放在`Workflow`类的`PostConstruct`中可以确保一旦应用程序上下文被加载并且 bean 创建完成，接收者就开始消费消息。

```
private val disposables = Disposables.composite()

@PostConstruct
fun connect() {
    disposables.add(
        receive()
            .subscribe(
                **{** log.info("Ended subscription to Kafka Receiver") **}**,
                **{** err **->** log.error("Error in Kafka Receiver flow", err) **}** )
    )
}

@PreDestroy
fun disconnect() {
    this.disposables.dispose()
}
```

# 整合测试

使用`EmbeddedKafka`注释配置带有输入和输出主题的 IT 测试

```
@EmbeddedKafka(partitions = 1,
    topics = ["input", "output"]
)
```

除了我们在 PublisherConfiguration 和 ReceiverConfiguration 类中创建的 KafkaReceiver 和 KafkaSender 之外，我们还需要一个测试 kafka sender 来将消息发布到`input`主题，以测试工作流应用测试 kafka sender 在`@BeforeClass`设置中的配置，就像其他 kafka sender 发布到输出主题一样。

由于 kafka 接收器在应用程序启动后立即启动，我们需要确保以这种方式在`@BeforeEach`中断开接收器，我们可以在我们的测试用例中订阅接收器以进行实际验证。

```
@BeforeEach
fun setUp() {workflow.disconnect()
}
```

最后，作为测试的一部分，在验证之后处置订户。

完整项目在此—[https://github . com/seetharamani/sample-reactor-Kafka-spring boot](https://github.com/seetharamani/sample-reactor-kafka-springboot)