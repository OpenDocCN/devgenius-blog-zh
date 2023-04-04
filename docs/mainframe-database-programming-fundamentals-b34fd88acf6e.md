# 大型机数据库编程基础 1

> 原文：<https://blog.devgenius.io/mainframe-database-programming-fundamentals-b34fd88acf6e?source=collection_archive---------5----------------------->

## 结构化编程方法

![](img/ed76d88a99bcf29886d6deb4d1a9e568.png)

莱尼·屈尼在 Unsplash 的照片

`Structured Programming`方法是一种提高程序代码可靠性和清晰性的技术(参见 [Hunt 1979](https://link.springer.com/article/10.3758/BF03205654) )。这就涉及到`Program Control Structures`的使用，比如`Sequential`、`Selection`、`Loop`。此外，程序代码被分解成小的可管理的块，即`modularization`。这篇文章描述了结构化编程方法在使用 ADABAS 数据库和自然编程语言构建大型机数据库程序中的应用。代码示例摘自 Nat914win 官方文档(参考 [Nat914win fs-about](https://documentation.softwareag.com/natural/nat914win/firststeps/fs-about.htm) )。

```
PRE-REQUISITE:
(1) ADABAS and NATURAL Server Community Edition (Docker Version). 
(2) NaturalONE IDE.
(refer [here](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-1-6597688406ad) to read the [setup guide](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-1-6597688406ad) for the above items).
(3) Adabas DDM for EMPLOYEES and VEHICLES database files.
(refer [here](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-6-48b4b2fd3e6d) to get [Natural NSD format files](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-6-48b4b2fd3e6d) for the above items).
```

# (1)程序控制结构

**(1.1)顺序控制结构**

默认情况下，大多数程序从顺序控制结构开始，即指令语句一个接一个地排列。

![](img/b0a90e91c43a03e20f92e7f76b1e9405.png)

照片由 [Siora 摄影](https://unsplash.com/@siora18?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

一个简单的例子就是经典的 Hello World！程序如下(参考 [Nat914win fs-hello](https://documentation.softwareag.com/natural/nat914win/firststeps/fs-hello.htm) )。这里，指令语句从顶部开始执行，然后向下移动到底部。*符号表示计算机在执行过程中将跳过的行。显示语句指示计算机向用户输出一些内容，例如`Hello world!`。END 语句标志着程序执行的结束。

```
* The "Hello world!" example in Natural.
*
DISPLAY "Hello world!"
END /* End of program
```

**(1.2)选择控制结构**

该控制结构指示计算机从可用分支中选择一个代码块分支。

![](img/fb31d2edb102708305047d6c3f387ed9.png)

由[edu·格兰德](https://unsplash.com/@edgr?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

下面的例子(参考 [Nat914win if](https://documentation.softwareag.com/natural/nat911win/sm/if.htm) )演示了选择控制结构的使用。在本例中，程序被指示查找位于城市“法兰克福”的雇员，如果他们的工资高于 40，000，并且他们的出生日期在 1945 年 1 月 1 日之后，则打印他们的姓名、出生日期、工资和他们的汽车品牌。这个程序应用一个嵌套的 IF-ELSE 结构来支持其复杂的查询条件分支。

```
** Example 'IFEX1S': IF (structured mode)                               
************************************************************************
DEFINE DATA LOCAL                                                       
1 EMPLOY-VIEW VIEW OF EMPLOYEES                                         
  2 PERSONNEL-ID                                                        
  2 NAME                                                                
  2 FIRST-NAME                                                          
  2 SALARY (1)                                                          
  2 BIRTH                                                               
1 VEHIC-VIEW VIEW OF VEHICLES                                           
  2 PERSONNEL-ID                                                        
  2 MAKE                                                                
*                                                                       
1 #BIRTH (D)                                                            
END-DEFINE                                                              
*                                                                       
MOVE EDITED '19450101' TO #BIRTH (EM=YYYYMMDD)                          
SUSPEND IDENTICAL SUPPRESS                                              
LIMIT 20                                                                
*                                                            
FND. FIND EMPLOY-VIEW WITH CITY = 'FRANKFURT'                
          SORTED BY NAME BIRTH                               
  IF SALARY (1) LT 40000                                     
    WRITE NOTITLE '*****' NAME 30X 'SALARY LT 40000'         
  ELSE                                                       
    IF BIRTH GT #BIRTH                                       
      FIND VEHIC-VIEW WITH PERSONNEL-ID = PERSONNEL-ID (FND.)
        DISPLAY (IS=ON)                                      
                NAME BIRTH (EM=YYYY-MM-DD)                   
                SALARY (1) MAKE (AL=8)                       
      END-FIND                                               
    END-IF                                                   
  END-IF                                                  
END-FIND                                                     
END
```

**(1.3)回路控制结构**

循环控制结构用于重复执行某些指令语句。重复结构有助于优化指令语句的安排，即消除不必要的语句冗余。

![](img/fb1bc91adbb076fb4eb4b07bf55b4b82.png)

照片由 [Ruben Christen](https://unsplash.com/@ruben_christen?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

下面的例子(参考 [Nat914win repeat](https://documentation.softwareag.com/natural/nat914win/sm/repeat.htm) )演示了使用循环结构来重复某些计算机处理任务。这里，要求计算机用户输入计算机的雇员号，以便它可以启动数据库搜索任务。搜索任务将一直重复，直到计算机用户决定终止它，即通过输入一个`SPACE`字符。

```
** Example 'RPTEX1S': REPEAT (structured mode)                          
************************************************************************
DEFINE DATA LOCAL                                                       
1 EMPLOY-VIEW VIEW OF EMPLOYEES                                         
  2 PERSONNEL-ID                                                        
  2 NAME                                                                
*                                                                       
1 #PERS-NR (A8)                                                         
END-DEFINE                                                              
*                                                                       
REPEAT                                                            
  INPUT 'ENTER A PERSONNEL NUMBER:' #PERS-NR                            
  IF #PERS-NR = ' '                                                     
    ESCAPE BOTTOM                                                       
  END-IF                                                                
  /*                                                                    
  FIND EMPLOY-VIEW WITH PERSONNEL-ID = #PERS-NR                         
    IF NO RECORD FOUND                                                  
      REINPUT 'NO RECORD FOUND'                                         
    END-NOREC           
    DISPLAY NOTITLE NAME
  END-FIND              
END-REPEAT          
*                       
END
```

# (2)程序模块化

程序模块化是将程序分解成具有特定目标的更小的可管理的代码组的过程。通过这种方式，诸如编码和调试之类的开发工作将会更加容易。这提高了编程效率。

![](img/1725af1db3872855b46df6ce23448e59.png)

照片由 [TStudio_lv](https://unsplash.com/@tstudio_lv?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

下面的例子(参考 [Nat914win inlinesub](https://documentation.softwareag.com/natural/nat914win/firststeps/fs-inlinesub) )演示了一种叫做`SUBROUTINE`的代码块的使用。它有一个特定的目标，即标记一个特殊的员工。为了实现这一目标，它分配了一个带有值' * '的数据#标记。假设在将来，如果开发人员计划用不同的方法来标记数据，他们可以只修改子程序，而不必关心程序的其他部分。这节省了他们的编码时间，同时最小化了代码变更的风险。

```
DEFINE DATA
LOCAL
  1 #NAME-START        (A20)
  1 #NAME-END          (A20)
  1 #MARK              (A1)
  1 EMPLOYEES-VIEW VIEW OF EMPLOYEES
    2 FULL-NAME
      3 NAME (A20)
    2 DEPT (A6)
    2 LEAVE-DATA
      3 LEAVE-DUE (N2)
END-DEFINE
*
RP1\. REPEAT
*
INPUT (AD=MT)
  "Start:" #NAME-START /
  "End:  " #NAME-END*
  IF #NAME-START = '.' THEN
    ESCAPE BOTTOM (RP1.)
  END-IF
*
  IF #NAME-END = ' ' THEN
    MOVE #NAME-START TO #NAME-END
  END-IF
*
  RD1\. READ EMPLOYEES-VIEW BY NAME
    STARTING FROM #NAME-START
    ENDING AT #NAME-END
*    
    IF LEAVE-DUE >= 20 THEN
      PERFORM MARK-SPECIAL-EMPLOYEES
    ELSE
      RESET #MARK
    END-IF
*
    DISPLAY NAME 3X DEPT 3X LEAVE-DUE 3X '>=20' #MARK
*
  END-READ
*
  IF *COUNTER (RD1.) = 0 THEN
    REINPUT 'No employees meet your criteria.'
  END-IF
*
END-REPEAT
*
DEFINE SUBROUTINE MARK-SPECIAL-EMPLOYEES
  MOVE '*' TO #MARK
END-SUBROUTINE
*
END
```

上面讨论的例子展示了关于大型机数据库编程的结构化编程的基本方面。在自然语言编程中，还有多种类型的程序控制结构和程序模块化策略。

未完待续[下篇](https://medium.com/@mohamad.razzi.my/mainframe-database-programming-27803b92a3a3)。

本帖是“Adabas & Natural 入门”系列的一部分，该系列包括:
(1) [设置 Adabas & Natural 社区版(Docker 版)](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-1-6597688406ad)。
(2) [通过 Adabas REST Web app 访问 Adabas 数据库](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-2-34621e576fa4)。
(3) [Adabas“周期组”和“多值”以 JSON 数据格式表示](/getting-started-with-adabas-natural-part-3-a334822db12)。
(4) [使用 Adabas TCP-IP 节点包访问 Adabas 数据库](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-4-728e6977ad4f)。
(5) [使用 NaturalONE IDE 的大型机编程(Natural)介绍](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-5-1665a0be42ab)。
(6) [使用自然编程和自然 IDE 访问 Adabas 数据库](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-6-48b4b2fd3e6d)。
(7) [大型机数据库编程基础](https://medium.com/@mohamad.razzi.my/mainframe-database-programming-fundamentals-b34fd88acf6e)。
(8) [大型机数据库编程中级](https://medium.com/@mohamad.razzi.my/mainframe-database-programming-27803b92a3a3)。
(9) [使用自然 AJAX 框架开发 AJAX 网页](https://medium.com/@mohamad.razzi.my/developing-ajax-web-pages-e270eb59fc92)。
(10)[AJAX 实际上是如何工作的](https://medium.com/@mohamad.razzi.my/how-does-ajax-actually-work-2f57cf4ddc55)？
(11) [向 ADABAS REST Web app 发送带有 JWT 令牌的 AJAX 请求](https://medium.com/dev-genius/how-does-ajax-actually-work-2f57cf4ddc55)。
(12)通过 Java Servlets 和 Java Web Services (SOAP)向 ADABAS REST 服务发送 HTTP 请求。

# 参考资料:

亨特，K.P. (1979)。结构化编程导论。行为研究方法&仪器，11，229–233。

`Nat914win fs-about`:
[https://documentation . software ag . com/natural/NAT 914 win/first steps/fs-about . htm](https://documentation.softwareag.com/natural/nat914win/firststeps/fs-about.htm)

`Nat914win fs-hello`:
https://documentation . software ag . com/natural/NAT 914 win/first steps/fs-hello . htm

`Nat914win if`: [https://documentation . software ag . com/natural/NAT 911 win/sm/if . htm](https://documentation.softwareag.com/natural/nat911win/sm/if.htm)

`Nat914win repeat`:
[https://documentation . software ag . com/natural/NAT 914 win/sm/repeat . htm](https://documentation.softwareag.com/natural/nat914win/sm/repeat.htm)

`Nat914win inlinesub`:
[https://documentation . software ag . com/natural/NAT 914 win/first steps/fs-inline sub](https://documentation.softwareag.com/natural/nat914win/firststeps/fs-inlinesub)