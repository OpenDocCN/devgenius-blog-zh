# 如何在 Oracle Linux 9 中安装 ansi ble—ansi ble install

> 原文：<https://blog.devgenius.io/how-to-install-ansible-in-oracle-linux-9-ansible-install-9bc842cfac2?source=collection_archive---------11----------------------->

## 如何使用 DNF 和“appstream”系统存储库在 Oracle Linux 9 中安装和维护最新的 Ansible。

# 如何在 Oracle Linux 版本 9 中安装 Ansible

今天我们将讨论使用`appstream`系统库在 AlmaLinux 9 中安装和维护 Ansible 的更简单的方法。我是卢卡·伯尔顿，欢迎来到今天的《神秘飞行员》节目。

# 如何在 Oracle Linux 9 中安装 Ansible

*   `ansible-core`包含在应用流存储库中
*   `ansible`套餐不可用

今天我们讨论的是如何在 Oracle Linux 9 中安装 Ansible。

在 Oracle Linux version 9 中安装和维护最新 Ansible 的更简单的方法是使用 AppStream 分发存储库中包含的`ansible-core`包。请注意,`ansible`套餐不再提供。没有必要使用额外的 EPEL 包存储库。

参见: [Ansible 术语——ansi ble 与 ansible-core 包](https://www.ansiblepilot.com/articles/ansible-terminology-ansible-vs-ansible-core-packages/)。

# 链接

*   [https://www.oracle.com/linux/](https://www.oracle.com/linux/)

# 演示

让我们快速进入如何在 Oracle Linux 中安装最新版本 Ansible 的现场演示。

我将使用 AppStream 分发存储库在 Oracle Linux 9 中安装`ansible-core`包。

# 密码

*   install-ansi ble-Oracle Linux 9 . sh

```
#!/bin/bash
sudo yum install ansible-core
```

# 执行

```
$ ssh [devops@oraclelinux.example.com](mailto:devops@oraclelinux.example.com)
[devops@demo ~]$ sudo su
[root@demo devops]# cat /etc/redhat-release 
Red Hat Enterprise Linux release 9.0 (Ootpa)
[root@demo devops]# cat /etc/os-release 
NAME="Oracle Linux Server"
VERSION="9.0"
ID="ol"
ID_LIKE="fedora"
VARIANT="Server"
VARIANT_ID="server"
VERSION_ID="9.0"
PLATFORM_ID="platform:el9"
PRETTY_NAME="Oracle Linux Server 9.0"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:oracle:linux:9:0:server"
HOME_URL="[https://linux.oracle.com/](https://linux.oracle.com/)"
BUG_REPORT_URL="[https://github.com/oracle/oracle-linux](https://github.com/oracle/oracle-linux)"ORACLE_BUGZILLA_PRODUCT="Oracle Linux 9"
ORACLE_BUGZILLA_PRODUCT_VERSION=9.0
ORACLE_SUPPORT_PRODUCT="Oracle Linux"
ORACLE_SUPPORT_PRODUCT_VERSION=9.0
[root@demo devops]# hostnamectl 
 Static hostname: demo.example.com
       Icon name: computer-vm
         Chassis: vm 🖴
      Machine ID: 71df5c9e92ab486f9e8a3f59b9bd5068
         Boot ID: ef2f06382bbe40c89fd104e8a48aa778
  Virtualization: oracle
Operating System: Oracle Linux Server 9.0           
     CPE OS Name: cpe:/o:oracle:linux:9:0:server
          Kernel: Linux 5.15.0-0.30.19.el9uek.x86_64
    Architecture: x86-64
 Hardware Vendor: innotek GmbH
  Hardware Model: VirtualBox
[root@demo devops]# uname -a
Linux demo.example.com 5.15.0-0.30.19.el9uek.x86_64 #2 SMP Wed Jun 15 22:59:49 PDT 2022 x86_64 x86_64 x86_64 GNU/Linux
[root@demo devops]# dnf search ansible
Last metadata expiration check: 0:09:16 ago on Tue 12 Jul 2022 09:06:33 AM UTC.
================================= Name & Summary Matched: ansible =================================
ansible-freeipa-tests.noarch : ansible-freeipa tests
ansible-pcp.noarch : Ansible Metric collection for Performance Co-Pilot
ansible-pcp.src : Ansible Metric collection for Performance Co-Pilot
ansible-test.x86_64 : Tool for testing ansible plugin and module code
====================================== Name Matched: ansible ======================================
ansible-core.src : SSH-based configuration management, deployment, and task execution system
ansible-core.x86_64 : SSH-based configuration management, deployment, and task execution system
ansible-freeipa.noarch : Roles and playbooks to deploy FreeIPA servers, replicas and clients
ansible-freeipa.src : Roles and playbooks to deploy FreeIPA servers, replicas and clients
[root@demo devops]# dnf list ansible
Last metadata expiration check: 0:09:27 ago on Tue 12 Jul 2022 09:06:33 AM UTC.
Error: No matching Packages to list
[root@demo devops]# dnf list ansible-core
Last metadata expiration check: 0:09:34 ago on Tue 12 Jul 2022 09:06:33 AM UTC.
Available Packages
ansible-core.src                              2.12.2-1.el9                            ol9_appstream
ansible-core.x86_64                           2.12.2-1.el9                            ol9_appstream
[root@demo devops]# dnf info ansible-core
Last metadata expiration check: 0:09:45 ago on Tue 12 Jul 2022 09:06:33 AM UTC.
Available Packages
Name         : ansible-core
Version      : 2.12.2
Release      : 1.el9
Architecture : src
Size         : 8.0 M
Source       : None
Repository   : ol9_appstream
Summary      : SSH-based configuration management, deployment, and task execution system
URL          : [http://ansible.com](http://ansible.com)
License      : GPLv3+
Description  : Ansible is a radically simple model-driven configuration management,
             : multi-node deployment, and remote task execution system. Ansible works
             : over SSH and does not require any software or daemons to be installed
             : on remote nodes. Extension modules can be written in any language and
             : are transferred to managed machines automatically.Name         : ansible-core
Version      : 2.12.2
Release      : 1.el9
Architecture : x86_64
Size         : 3.6 M
Source       : ansible-core-2.12.2-1.el9.src.rpm
Repository   : ol9_appstream
Summary      : SSH-based configuration management, deployment, and task execution system
URL          : [http://ansible.com](http://ansible.com)
License      : GPLv3+
Description  : Ansible is a radically simple model-driven configuration management,
             : multi-node deployment, and remote task execution system. Ansible works
             : over SSH and does not require any software or daemons to be installed
             : on remote nodes. Extension modules can be written in any language and
             : are transferred to managed machines automatically.[root@demo devops]# dnf install ansible-core
Last metadata expiration check: 0:09:57 ago on Tue 12 Jul 2022 09:06:33 AM UTC.
Dependencies resolved.
===================================================================================================
 Package                      Architecture Version                   Repository               Size
===================================================================================================
Installing:
 ansible-core                 x86_64       2.12.2-1.el9              ol9_appstream           3.6 M
Installing dependencies:
 git                          x86_64       2.31.1-2.el9.2            ol9_appstream           149 k
 git-core                     x86_64       2.31.1-2.el9.2            ol9_appstream           3.7 M
 git-core-doc                 noarch       2.31.1-2.el9.2            ol9_appstream           3.4 M
 perl-Carp                    noarch       1.50-460.el9              ol9_appstream            34 k
 perl-Class-Struct            noarch       0.66-479.el9              ol9_appstream            32 k
 perl-DynaLoader              x86_64       1.47-479.el9              ol9_appstream            35 k
 perl-Encode                  x86_64       4:3.08-462.el9            ol9_appstream           1.8 M
 perl-Errno                   x86_64       1.30-479.el9              ol9_appstream            24 k
 perl-Error                   noarch       1:0.17029-7.el9           ol9_appstream            57 k
 perl-Exporter                noarch       5.74-461.el9              ol9_appstream            37 k
 perl-Fcntl                   x86_64       1.13-479.el9              ol9_appstream            30 k
 perl-File-Basename           noarch       2.85-479.el9              ol9_appstream            27 k
 perl-File-Find               noarch       1.37-479.el9              ol9_appstream            35 k
 perl-File-Path               noarch       2.18-4.el9                ol9_appstream            36 k
 perl-File-Temp               noarch       1:0.231.100-4.el9         ol9_appstream            67 k
 perl-File-stat               noarch       1.09-479.el9              ol9_appstream            27 k
 perl-Getopt-Long             noarch       1:2.52-4.el9              ol9_appstream            71 k
 perl-Getopt-Std              noarch       1.12-479.el9              ol9_appstream            25 k
 perl-Git                     noarch       2.31.1-2.el9.2            ol9_appstream            48 k
 perl-HTTP-Tiny               noarch       0.076-460.el9             ol9_appstream            63 k
 perl-IO                      x86_64       1.43-479.el9              ol9_appstream           119 k
 perl-IPC-Open3               noarch       1.21-479.el9              ol9_appstream            32 k
 perl-MIME-Base64             x86_64       3.16-4.el9                ol9_appstream            38 k
 perl-POSIX                   x86_64       1.94-479.el9              ol9_appstream           106 k
 perl-PathTools               x86_64       3.78-461.el9              ol9_appstream           108 k
 perl-Pod-Escapes             noarch       1:1.07-460.el9            ol9_appstream            21 k
 perl-Pod-Perldoc             noarch       3.28.01-461.el9           ol9_appstream           116 k
 perl-Pod-Simple              noarch       1:3.42-4.el9              ol9_appstream           272 k
 perl-Pod-Usage               noarch       4:2.01-4.el9              ol9_appstream            48 k
 perl-Scalar-List-Utils       x86_64       4:1.56-461.el9            ol9_appstream            83 k
 perl-SelectSaver             noarch       1.02-479.el9              ol9_appstream            21 k
 perl-Socket                  x86_64       4:2.031-4.el9             ol9_appstream            62 k
 perl-Storable                x86_64       1:3.21-460.el9            ol9_appstream           100 k
 perl-Symbol                  noarch       1.08-479.el9              ol9_appstream            24 k
 perl-Term-ANSIColor          noarch       5.01-461.el9              ol9_appstream            54 k
 perl-Term-Cap                noarch       1.17-460.el9              ol9_appstream            27 k
 perl-TermReadKey             x86_64       2.38-11.el9               ol9_appstream            42 k
 perl-Text-ParseWords         noarch       3.30-460.el9              ol9_appstream            17 k
 perl-Text-Tabs+Wrap          noarch       2013.0523-460.el9         ol9_appstream            29 k
 perl-Time-Local              noarch       2:1.300-7.el9             ol9_appstream            41 k
 perl-constant                noarch       1.33-461.el9              ol9_appstream            28 k
 perl-if                      noarch       0.60.800-479.el9          ol9_appstream            23 k
 perl-interpreter             x86_64       4:5.32.1-479.el9          ol9_appstream            86 k
 perl-lib                     x86_64       0.65-479.el9              ol9_appstream            24 k
 perl-libs                    x86_64       4:5.32.1-479.el9          ol9_appstream           2.7 M
 perl-mro                     x86_64       1.23-479.el9              ol9_appstream            38 k
 perl-overload                noarch       1.31-479.el9              ol9_appstream            55 k
 perl-overloading             noarch       0.02-479.el9              ol9_appstream            22 k
 perl-parent                  noarch       1:0.238-460.el9           ol9_appstream            15 k
 perl-podlators               noarch       1:4.14-460.el9            ol9_appstream           135 k
 perl-subs                    noarch       1.03-479.el9              ol9_appstream            21 k
 perl-vars                    noarch       1.05-479.el9              ol9_appstream            23 k
 python3-babel                noarch       2.9.1-2.el9               ol9_appstream           6.6 M
 python3-jinja2               noarch       2.11.3-4.el9              ol9_appstream           330 k
 python3-markupsafe           x86_64       1.1.1-12.el9              ol9_appstream            52 k
 python3-packaging            noarch       20.9-5.el9                ol9_appstream           117 k
 python3-pytz                 noarch       2021.1-4.el9              ol9_appstream            74 k
 python3-pyyaml               x86_64       5.4.1-6.el9               ol9_baseos_latest       258 k
 python3-resolvelib           noarch       0.5.4-5.el9               ol9_appstream            59 k
 sshpass                      x86_64       1.09-4.el9                ol9_appstream            33 kTransaction Summary
===================================================================================================
Install  61 PackagesTotal download size: 25 M
Installed size: 92 M
Is this ok [y/N]: y
Downloading Packages:
(1/61): git-2.31.1-2.el9.2.x86_64.rpm                              183 kB/s | 149 kB     00:00    
(2/61): python3-pyyaml-5.4.1-6.el9.x86_64.rpm                       51 kB/s | 258 kB     00:05    
(3/61): ansible-core-2.12.2-1.el9.x86_64.rpm                       687 kB/s | 3.6 MB     00:05    
(4/61): git-core-2.31.1-2.el9.2.x86_64.rpm                         811 kB/s | 3.7 MB     00:04    
(5/61): perl-Carp-1.50-460.el9.noarch.rpm                          292 kB/s |  34 kB     00:00    
(6/61): perl-Class-Struct-0.66-479.el9.noarch.rpm                  345 kB/s |  32 kB     00:00    
(7/61): perl-DynaLoader-1.47-479.el9.x86_64.rpm                    286 kB/s |  35 kB     00:00    
(8/61): perl-Errno-1.30-479.el9.x86_64.rpm                         197 kB/s |  24 kB     00:00    
(9/61): perl-Error-0.17029-7.el9.noarch.rpm                        224 kB/s |  57 kB     00:00    
(10/61): perl-Exporter-5.74-461.el9.noarch.rpm                     164 kB/s |  37 kB     00:00    
(11/61): perl-Fcntl-1.13-479.el9.x86_64.rpm                        153 kB/s |  30 kB     00:00    
(12/61): perl-File-Basename-2.85-479.el9.noarch.rpm                167 kB/s |  27 kB     00:00    
(13/61): git-core-doc-2.31.1-2.el9.2.noarch.rpm                    2.1 MB/s | 3.4 MB     00:01    
(14/61): perl-Encode-3.08-462.el9.x86_64.rpm                       1.5 MB/s | 1.8 MB     00:01    
(15/61): perl-File-Find-1.37-479.el9.noarch.rpm                    201 kB/s |  35 kB     00:00    
(16/61): perl-File-Path-2.18-4.el9.noarch.rpm                      322 kB/s |  36 kB     00:00    
(17/61): perl-File-Temp-0.231.100-4.el9.noarch.rpm                 585 kB/s |  67 kB     00:00    
(18/61): perl-File-stat-1.09-479.el9.noarch.rpm                    218 kB/s |  27 kB     00:00    
(19/61): perl-Getopt-Std-1.12-479.el9.noarch.rpm                   277 kB/s |  25 kB     00:00    
(20/61): perl-Getopt-Long-2.52-4.el9.noarch.rpm                    643 kB/s |  71 kB     00:00    
(21/61): perl-Git-2.31.1-2.el9.2.noarch.rpm                        476 kB/s |  48 kB     00:00    
(22/61): perl-HTTP-Tiny-0.076-460.el9.noarch.rpm                   564 kB/s |  63 kB     00:00    
(23/61): perl-IPC-Open3-1.21-479.el9.noarch.rpm                    339 kB/s |  32 kB     00:00    
(24/61): perl-IO-1.43-479.el9.x86_64.rpm                           973 kB/s | 119 kB     00:00    
(25/61): perl-MIME-Base64-3.16-4.el9.x86_64.rpm                    322 kB/s |  38 kB     00:00    
(26/61): perl-PathTools-3.78-461.el9.x86_64.rpm                    842 kB/s | 108 kB     00:00    
(27/61): perl-POSIX-1.94-479.el9.x86_64.rpm                        736 kB/s | 106 kB     00:00    
(28/61): perl-Pod-Escapes-1.07-460.el9.noarch.rpm                  219 kB/s |  21 kB     00:00    
(29/61): perl-Pod-Perldoc-3.28.01-461.el9.noarch.rpm               1.1 MB/s | 116 kB     00:00    
(30/61): perl-Pod-Usage-2.01-4.el9.noarch.rpm                      539 kB/s |  48 kB     00:00    
(31/61): perl-Pod-Simple-3.42-4.el9.noarch.rpm                     1.3 MB/s | 272 kB     00:00    
(32/61): perl-Scalar-List-Utils-1.56-461.el9.x86_64.rpm            783 kB/s |  83 kB     00:00    
(33/61): perl-SelectSaver-1.02-479.el9.noarch.rpm                  244 kB/s |  21 kB     00:00    
(34/61): perl-Socket-2.031-4.el9.x86_64.rpm                        468 kB/s |  62 kB     00:00    
(35/61): perl-Storable-3.21-460.el9.x86_64.rpm                     744 kB/s | 100 kB     00:00    
(36/61): perl-Symbol-1.08-479.el9.noarch.rpm                       222 kB/s |  24 kB     00:00    
(37/61): perl-Term-Cap-1.17-460.el9.noarch.rpm                     318 kB/s |  27 kB     00:00    
(38/61): perl-Term-ANSIColor-5.01-461.el9.noarch.rpm               495 kB/s |  54 kB     00:00    
(39/61): perl-TermReadKey-2.38-11.el9.x86_64.rpm                   400 kB/s |  42 kB     00:00    
(40/61): perl-Text-ParseWords-3.30-460.el9.noarch.rpm              141 kB/s |  17 kB     00:00    
(41/61): perl-Text-Tabs+Wrap-2013.0523-460.el9.noarch.rpm          260 kB/s |  29 kB     00:00    
(42/61): perl-Time-Local-1.300-7.el9.noarch.rpm                    363 kB/s |  41 kB     00:00    
(43/61): perl-constant-1.33-461.el9.noarch.rpm                     240 kB/s |  28 kB     00:00    
(44/61): perl-if-0.60.800-479.el9.noarch.rpm                       221 kB/s |  23 kB     00:00    
(45/61): perl-interpreter-5.32.1-479.el9.x86_64.rpm                707 kB/s |  86 kB     00:00    
(46/61): perl-lib-0.65-479.el9.x86_64.rpm                          331 kB/s |  24 kB     00:00    
(47/61): perl-mro-1.23-479.el9.x86_64.rpm                          378 kB/s |  38 kB     00:00    
(48/61): perl-overload-1.31-479.el9.noarch.rpm                     665 kB/s |  55 kB     00:00    
(49/61): perl-overloading-0.02-479.el9.noarch.rpm                  223 kB/s |  22 kB     00:00    
(50/61): perl-parent-0.238-460.el9.noarch.rpm                      155 kB/s |  15 kB     00:00    
(51/61): perl-subs-1.03-479.el9.noarch.rpm                         247 kB/s |  21 kB     00:00    
(52/61): perl-podlators-4.14-460.el9.noarch.rpm                    930 kB/s | 135 kB     00:00    
(53/61): perl-vars-1.05-479.el9.noarch.rpm                         246 kB/s |  23 kB     00:00    
(54/61): python3-jinja2-2.11.3-4.el9.noarch.rpm                    1.5 MB/s | 330 kB     00:00    
(55/61): python3-markupsafe-1.1.1-12.el9.x86_64.rpm                302 kB/s |  52 kB     00:00    
(56/61): python3-packaging-20.9-5.el9.noarch.rpm                   330 kB/s | 117 kB     00:00    
(57/61): python3-pytz-2021.1-4.el9.noarch.rpm                      315 kB/s |  74 kB     00:00    
(58/61): python3-resolvelib-0.5.4-5.el9.noarch.rpm                 289 kB/s |  59 kB     00:00    
(59/61): sshpass-1.09-4.el9.x86_64.rpm                             173 kB/s |  33 kB     00:00    
(60/61): perl-libs-5.32.1-479.el9.x86_64.rpm                       1.4 MB/s | 2.7 MB     00:01    
(61/61): python3-babel-2.9.1-2.el9.noarch.rpm                      2.0 MB/s | 6.6 MB     00:03    
---------------------------------------------------------------------------------------------------
Total                                                              2.2 MB/s |  25 MB     00:11     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                           1/1 
  Installing       : perl-Pod-Escapes-1:1.07-460.el9.noarch                                   1/61 
  Installing       : perl-Text-Tabs+Wrap-2013.0523-460.el9.noarch                             2/61 
  Installing       : perl-File-Path-2.18-4.el9.noarch                                         3/61 
  Installing       : perl-if-0.60.800-479.el9.noarch                                          4/61 
  Installing       : perl-Time-Local-2:1.300-7.el9.noarch                                     5/61 
  Installing       : perl-Class-Struct-0.66-479.el9.noarch                                    6/61 
  Installing       : perl-POSIX-1.94-479.el9.x86_64                                           7/61 
  Installing       : perl-Term-ANSIColor-5.01-461.el9.noarch                                  8/61 
  Installing       : perl-Term-Cap-1.17-460.el9.noarch                                        9/61 
  Installing       : perl-IPC-Open3-1.21-479.el9.noarch                                      10/61 
  Installing       : perl-File-Temp-1:0.231.100-4.el9.noarch                                 11/61 
  Installing       : perl-HTTP-Tiny-0.076-460.el9.noarch                                     12/61 
  Installing       : perl-subs-1.03-479.el9.noarch                                           13/61 
  Installing       : perl-Pod-Simple-1:3.42-4.el9.noarch                                     14/61 
  Installing       : perl-Socket-4:2.031-4.el9.x86_64                                        15/61 
  Installing       : perl-SelectSaver-1.02-479.el9.noarch                                    16/61 
  Installing       : perl-Symbol-1.08-479.el9.noarch                                         17/61 
  Installing       : perl-File-stat-1.09-479.el9.noarch                                      18/61 
  Installing       : perl-Pod-Perldoc-3.28.01-461.el9.noarch                                 19/61 
  Installing       : perl-podlators-1:4.14-460.el9.noarch                                    20/61 
  Installing       : perl-Fcntl-1.13-479.el9.x86_64                                          21/61 
  Installing       : perl-Text-ParseWords-3.30-460.el9.noarch                                22/61 
  Installing       : perl-mro-1.23-479.el9.x86_64                                            23/61 
  Installing       : perl-IO-1.43-479.el9.x86_64                                             24/61 
  Installing       : perl-overloading-0.02-479.el9.noarch                                    25/61 
  Installing       : perl-Pod-Usage-4:2.01-4.el9.noarch                                      26/61 
  Installing       : perl-Errno-1.30-479.el9.x86_64                                          27/61 
  Installing       : perl-File-Basename-2.85-479.el9.noarch                                  28/61 
  Installing       : perl-Getopt-Std-1.12-479.el9.noarch                                     29/61 
  Installing       : perl-MIME-Base64-3.16-4.el9.x86_64                                      30/61 
  Installing       : perl-Scalar-List-Utils-4:1.56-461.el9.x86_64                            31/61 
  Installing       : perl-constant-1.33-461.el9.noarch                                       32/61 
  Installing       : perl-Storable-1:3.21-460.el9.x86_64                                     33/61 
  Installing       : perl-overload-1.31-479.el9.noarch                                       34/61 
  Installing       : perl-parent-1:0.238-460.el9.noarch                                      35/61 
  Installing       : perl-Getopt-Long-1:2.52-4.el9.noarch                                    36/61 
  Installing       : perl-vars-1.05-479.el9.noarch                                           37/61 
  Installing       : perl-Exporter-5.74-461.el9.noarch                                       38/61 
  Installing       : perl-PathTools-3.78-461.el9.x86_64                                      39/61 
  Installing       : perl-Encode-4:3.08-462.el9.x86_64                                       40/61 
  Installing       : perl-Carp-1.50-460.el9.noarch                                           41/61 
  Installing       : perl-libs-4:5.32.1-479.el9.x86_64                                       42/61 
  Installing       : perl-interpreter-4:5.32.1-479.el9.x86_64                                43/61 
  Installing       : git-core-2.31.1-2.el9.2.x86_64                                          44/61 
  Installing       : git-core-doc-2.31.1-2.el9.2.noarch                                      45/61 
  Installing       : perl-DynaLoader-1.47-479.el9.x86_64                                     46/61 
  Installing       : perl-TermReadKey-2.38-11.el9.x86_64                                     47/61 
  Installing       : perl-Error-1:0.17029-7.el9.noarch                                       48/61 
  Installing       : perl-File-Find-1.37-479.el9.noarch                                      49/61 
  Installing       : perl-lib-0.65-479.el9.x86_64                                            50/61 
  Installing       : perl-Git-2.31.1-2.el9.2.noarch                                          51/61 
  Installing       : git-2.31.1-2.el9.2.x86_64                                               52/61 
  Installing       : sshpass-1.09-4.el9.x86_64                                               53/61 
  Installing       : python3-resolvelib-0.5.4-5.el9.noarch                                   54/61 
  Installing       : python3-pytz-2021.1-4.el9.noarch                                        55/61 
  Installing       : python3-babel-2.9.1-2.el9.noarch                                        56/61 
  Installing       : python3-packaging-20.9-5.el9.noarch                                     57/61 
  Installing       : python3-markupsafe-1.1.1-12.el9.x86_64                                  58/61 
  Installing       : python3-jinja2-2.11.3-4.el9.noarch                                      59/61 
  Installing       : python3-pyyaml-5.4.1-6.el9.x86_64                                       60/61 
  Installing       : ansible-core-2.12.2-1.el9.x86_64                                        61/61 
  Running scriptlet: ansible-core-2.12.2-1.el9.x86_64                                        61/61 
  Verifying        : python3-pyyaml-5.4.1-6.el9.x86_64                                        1/61 
  Verifying        : ansible-core-2.12.2-1.el9.x86_64                                         2/61 
  Verifying        : git-2.31.1-2.el9.2.x86_64                                                3/61 
  Verifying        : git-core-2.31.1-2.el9.2.x86_64                                           4/61 
  Verifying        : git-core-doc-2.31.1-2.el9.2.noarch                                       5/61 
  Verifying        : perl-Carp-1.50-460.el9.noarch                                            6/61 
  Verifying        : perl-Class-Struct-0.66-479.el9.noarch                                    7/61 
  Verifying        : perl-DynaLoader-1.47-479.el9.x86_64                                      8/61 
  Verifying        : perl-Encode-4:3.08-462.el9.x86_64                                        9/61 
  Verifying        : perl-Errno-1.30-479.el9.x86_64                                          10/61 
  Verifying        : perl-Error-1:0.17029-7.el9.noarch                                       11/61 
  Verifying        : perl-Exporter-5.74-461.el9.noarch                                       12/61 
  Verifying        : perl-Fcntl-1.13-479.el9.x86_64                                          13/61 
  Verifying        : perl-File-Basename-2.85-479.el9.noarch                                  14/61 
  Verifying        : perl-File-Find-1.37-479.el9.noarch                                      15/61 
  Verifying        : perl-File-Path-2.18-4.el9.noarch                                        16/61 
  Verifying        : perl-File-Temp-1:0.231.100-4.el9.noarch                                 17/61 
  Verifying        : perl-File-stat-1.09-479.el9.noarch                                      18/61 
  Verifying        : perl-Getopt-Long-1:2.52-4.el9.noarch                                    19/61 
  Verifying        : perl-Getopt-Std-1.12-479.el9.noarch                                     20/61 
  Verifying        : perl-Git-2.31.1-2.el9.2.noarch                                          21/61 
  Verifying        : perl-HTTP-Tiny-0.076-460.el9.noarch                                     22/61 
  Verifying        : perl-IO-1.43-479.el9.x86_64                                             23/61 
  Verifying        : perl-IPC-Open3-1.21-479.el9.noarch                                      24/61 
  Verifying        : perl-MIME-Base64-3.16-4.el9.x86_64                                      25/61 
  Verifying        : perl-POSIX-1.94-479.el9.x86_64                                          26/61 
  Verifying        : perl-PathTools-3.78-461.el9.x86_64                                      27/61 
  Verifying        : perl-Pod-Escapes-1:1.07-460.el9.noarch                                  28/61 
  Verifying        : perl-Pod-Perldoc-3.28.01-461.el9.noarch                                 29/61 
  Verifying        : perl-Pod-Simple-1:3.42-4.el9.noarch                                     30/61 
  Verifying        : perl-Pod-Usage-4:2.01-4.el9.noarch                                      31/61 
  Verifying        : perl-Scalar-List-Utils-4:1.56-461.el9.x86_64                            32/61 
  Verifying        : perl-SelectSaver-1.02-479.el9.noarch                                    33/61 
  Verifying        : perl-Socket-4:2.031-4.el9.x86_64                                        34/61 
  Verifying        : perl-Storable-1:3.21-460.el9.x86_64                                     35/61 
  Verifying        : perl-Symbol-1.08-479.el9.noarch                                         36/61 
  Verifying        : perl-Term-ANSIColor-5.01-461.el9.noarch                                 37/61 
  Verifying        : perl-Term-Cap-1.17-460.el9.noarch                                       38/61 
  Verifying        : perl-TermReadKey-2.38-11.el9.x86_64                                     39/61 
  Verifying        : perl-Text-ParseWords-3.30-460.el9.noarch                                40/61 
  Verifying        : perl-Text-Tabs+Wrap-2013.0523-460.el9.noarch                            41/61 
  Verifying        : perl-Time-Local-2:1.300-7.el9.noarch                                    42/61 
  Verifying        : perl-constant-1.33-461.el9.noarch                                       43/61 
  Verifying        : perl-if-0.60.800-479.el9.noarch                                         44/61 
  Verifying        : perl-interpreter-4:5.32.1-479.el9.x86_64                                45/61 
  Verifying        : perl-lib-0.65-479.el9.x86_64                                            46/61 
  Verifying        : perl-libs-4:5.32.1-479.el9.x86_64                                       47/61 
  Verifying        : perl-mro-1.23-479.el9.x86_64                                            48/61 
  Verifying        : perl-overload-1.31-479.el9.noarch                                       49/61 
  Verifying        : perl-overloading-0.02-479.el9.noarch                                    50/61 
  Verifying        : perl-parent-1:0.238-460.el9.noarch                                      51/61 
  Verifying        : perl-podlators-1:4.14-460.el9.noarch                                    52/61 
  Verifying        : perl-subs-1.03-479.el9.noarch                                           53/61 
  Verifying        : perl-vars-1.05-479.el9.noarch                                           54/61 
  Verifying        : python3-babel-2.9.1-2.el9.noarch                                        55/61 
  Verifying        : python3-jinja2-2.11.3-4.el9.noarch                                      56/61 
  Verifying        : python3-markupsafe-1.1.1-12.el9.x86_64                                  57/61 
  Verifying        : python3-packaging-20.9-5.el9.noarch                                     58/61 
  Verifying        : python3-pytz-2021.1-4.el9.noarch                                        59/61 
  Verifying        : python3-resolvelib-0.5.4-5.el9.noarch                                   60/61 
  Verifying        : sshpass-1.09-4.el9.x86_64                                               61/61Installed:
  ansible-core-2.12.2-1.el9.x86_64                 git-2.31.1-2.el9.2.x86_64                       
  git-core-2.31.1-2.el9.2.x86_64                   git-core-doc-2.31.1-2.el9.2.noarch              
  perl-Carp-1.50-460.el9.noarch                    perl-Class-Struct-0.66-479.el9.noarch           
  perl-DynaLoader-1.47-479.el9.x86_64              perl-Encode-4:3.08-462.el9.x86_64               
  perl-Errno-1.30-479.el9.x86_64                   perl-Error-1:0.17029-7.el9.noarch               
  perl-Exporter-5.74-461.el9.noarch                perl-Fcntl-1.13-479.el9.x86_64                  
  perl-File-Basename-2.85-479.el9.noarch           perl-File-Find-1.37-479.el9.noarch              
  perl-File-Path-2.18-4.el9.noarch                 perl-File-Temp-1:0.231.100-4.el9.noarch         
  perl-File-stat-1.09-479.el9.noarch               perl-Getopt-Long-1:2.52-4.el9.noarch            
  perl-Getopt-Std-1.12-479.el9.noarch              perl-Git-2.31.1-2.el9.2.noarch                  
  perl-HTTP-Tiny-0.076-460.el9.noarch              perl-IO-1.43-479.el9.x86_64                     
  perl-IPC-Open3-1.21-479.el9.noarch               perl-MIME-Base64-3.16-4.el9.x86_64              
  perl-POSIX-1.94-479.el9.x86_64                   perl-PathTools-3.78-461.el9.x86_64              
  perl-Pod-Escapes-1:1.07-460.el9.noarch           perl-Pod-Perldoc-3.28.01-461.el9.noarch         
  perl-Pod-Simple-1:3.42-4.el9.noarch              perl-Pod-Usage-4:2.01-4.el9.noarch              
  perl-Scalar-List-Utils-4:1.56-461.el9.x86_64     perl-SelectSaver-1.02-479.el9.noarch            
  perl-Socket-4:2.031-4.el9.x86_64                 perl-Storable-1:3.21-460.el9.x86_64             
  perl-Symbol-1.08-479.el9.noarch                  perl-Term-ANSIColor-5.01-461.el9.noarch         
  perl-Term-Cap-1.17-460.el9.noarch                perl-TermReadKey-2.38-11.el9.x86_64             
  perl-Text-ParseWords-3.30-460.el9.noarch         perl-Text-Tabs+Wrap-2013.0523-460.el9.noarch    
  perl-Time-Local-2:1.300-7.el9.noarch             perl-constant-1.33-461.el9.noarch               
  perl-if-0.60.800-479.el9.noarch                  perl-interpreter-4:5.32.1-479.el9.x86_64        
  perl-lib-0.65-479.el9.x86_64                     perl-libs-4:5.32.1-479.el9.x86_64               
  perl-mro-1.23-479.el9.x86_64                     perl-overload-1.31-479.el9.noarch               
  perl-overloading-0.02-479.el9.noarch             perl-parent-1:0.238-460.el9.noarch              
  perl-podlators-1:4.14-460.el9.noarch             perl-subs-1.03-479.el9.noarch                   
  perl-vars-1.05-479.el9.noarch                    python3-babel-2.9.1-2.el9.noarch                
  python3-jinja2-2.11.3-4.el9.noarch               python3-markupsafe-1.1.1-12.el9.x86_64          
  python3-packaging-20.9-5.el9.noarch              python3-pytz-2021.1-4.el9.noarch                
  python3-pyyaml-5.4.1-6.el9.x86_64                python3-resolvelib-0.5.4-5.el9.noarch           
  sshpass-1.09-4.el9.x86_64Complete!
[root@demo devops]# ansible --version
ansible [core 2.12.2]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.9/site-packages/ansible
  ansible collection location = /root/.ansible/collections:/usr/share/ansible/collections
  executable location = /bin/ansible
  python version = 3.9.10 (main, Feb  9 2022, 00:00:00) [GCC 11.2.1 20220127 (Red Hat 11.2.1-9.4.0.1)]
  jinja version = 2.11.3
  libyaml = True
```

# 执行前

```
[root@demo devops]# dnf list ansible-core
Last metadata expiration check: 0:09:34 ago on Tue 12 Jul 2022 09:06:33 AM UTC.
Available Packages
ansible-core.src                              2.12.2-1.el9                            ol9_appstream
ansible-core.x86_64                           2.12.2-1.el9                            ol9_appstream
[root@demo devops]# dnf info ansible-core
Last metadata expiration check: 0:09:45 ago on Tue 12 Jul 2022 09:06:33 AM UTC.
Available Packages
Name         : ansible-core
Version      : 2.12.2
Release      : 1.el9
Architecture : src
Size         : 8.0 M
Source       : None
Repository   : ol9_appstream
Summary      : SSH-based configuration management, deployment, and task execution system
URL          : [http://ansible.com](http://ansible.com)
License      : GPLv3+
Description  : Ansible is a radically simple model-driven configuration management,
             : multi-node deployment, and remote task execution system. Ansible works
             : over SSH and does not require any software or daemons to be installed
             : on remote nodes. Extension modules can be written in any language and
             : are transferred to managed machines automatically.Name         : ansible-core
Version      : 2.12.2
Release      : 1.el9
Architecture : x86_64
Size         : 3.6 M
Source       : ansible-core-2.12.2-1.el9.src.rpm
Repository   : ol9_appstream
Summary      : SSH-based configuration management, deployment, and task execution system
URL          : [http://ansible.com](http://ansible.com)
License      : GPLv3+
Description  : Ansible is a radically simple model-driven configuration management,
             : multi-node deployment, and remote task execution system. Ansible works
             : over SSH and does not require any software or daemons to be installed
             : on remote nodes. Extension modules can be written in any language and
             : are transferred to managed machines automatically.
```

# 执行后

```
[root@demo devops]# dnf list ansible-core
Last metadata expiration check: 0:10:51 ago on Tue 12 Jul 2022 09:06:33 AM UTC.
Installed Packages
ansible-core.x86_64                           2.12.2-1.el9                           [@ol9_appstream](http://twitter.com/ol9_appstream)
Available Packages
ansible-core.src                              2.12.2-1.el9                           ol9_appstream 
[root@demo devops]# dnf info ansible-core
Last metadata expiration check: 0:11:07 ago on Tue 12 Jul 2022 09:06:33 AM UTC.
Installed Packages
Name         : ansible-core
Version      : 2.12.2
Release      : 1.el9
Architecture : x86_64
Size         : 9.3 M
Source       : ansible-core-2.12.2-1.el9.src.rpm
Repository   : [@System](http://twitter.com/System)
From repo    : ol9_appstream
Summary      : SSH-based configuration management, deployment, and task execution system
URL          : [http://ansible.com](http://ansible.com)
License      : GPLv3+
Description  : Ansible is a radically simple model-driven configuration management,
             : multi-node deployment, and remote task execution system. Ansible works
             : over SSH and does not require any software or daemons to be installed
             : on remote nodes. Extension modules can be written in any language and
             : are transferred to managed machines automatically.Available Packages
Name         : ansible-core
Version      : 2.12.2
Release      : 1.el9
Architecture : src
Size         : 8.0 M
Source       : None
Repository   : ol9_appstream
Summary      : SSH-based configuration management, deployment, and task execution system
URL          : [http://ansible.com](http://ansible.com)
License      : GPLv3+
Description  : Ansible is a radically simple model-driven configuration management,
             : multi-node deployment, and remote task execution system. Ansible works
             : over SSH and does not require any software or daemons to be installed
             : on remote nodes. Extension modules can be written in any language and
             : are transferred to managed machines automatically.
```

[](https://github.com/lucab85/ansible-pilot) [## GitHub-lucab 85/ansi ble-Pilot:ansi ble Pilot YouTube 频道代码库

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/lucab85/ansible-pilot) 

# 概述

现在您知道了如何使用 AppStream 存储库在 Oracle Linux 中安装 Ansible 的最新版本。

我希望你喜欢读这篇文章。如果你愿意支持我成为一名作家，可以考虑注册[成为](https://ansiblepilot.medium.com/membership)中的一员。每月只需 5 美元，你就可以无限制地使用 Medium。

订阅 [YouTube 频道](https://www.youtube.com/channel/UC5MNbTYRHSCu9vAki3z9SmA)、 [Medium](https://ansiblepilot.medium.com/) 和[网站](https://www.ansiblepilot.com/)，不要错过 Ansible 试播的下一集。

# Ansible 的最佳资源

# 视频课程

*   [通过 200 多个示例学习 Ansible Automation&实践课程:通过一些真实的例子学习 Ansible，了解如何使用最常见的模块和 ansi ble 行动手册](https://www.udemy.com/course/ansible-by-examples-devops/?referralCode=8E065F6D6F8622A3DEC8)

# 书

*   [举例说明:针对 Linux 和 Windows 系统管理员和开发人员的 200 多个自动化示例](https://leanpub.com/ansiblebyexamples)
*   [负责 Windows 示例:30 多个针对 Windows 系统管理员和开发人员的自动化示例](https://www.amazon.com/dp/B09VCTP1L4)
*   [ansi ble For Linux by Examples:100 多个针对 Linux 系统管理员和开发人员的自动化示例](https://www.amazon.com/dp/B09VCX4CVD)
*   [Ansible Linux 文件系统示例:30 多个针对现代 IT 基础设施的 Linux 文件和目录操作自动化示例](https://www.amazon.com/dp/B09X4TF8P8)
*   [通过示例负责容器和 Kubernetes:10 多个自动化示例来自动化容器、Kubernetes 和 OpenShift](https://www.amazon.com/dp/B09XVDWD4W)
*   [负责安全示例:100 多个自动化示例，用于自动化现代 IT 基础架构的安全和验证合规性](https://www.amazon.com/dp/B09VHFLSPP)
*   [可行的技巧和窍门:10 多个可行的例子来节省时间和自动化更多的任务](https://www.amazon.com/dp/B09ZJ4GYM2)
*   [Ansible Linux 用户&按示例分组:20 多个关于现代 IT 基础设施的 Linux 用户和分组操作的自动化示例](https://www.amazon.com/dp/B09X6BYY8W)
*   [ansi ble For VMware by Examples:10 多个自动化您的 VMware 基础架构的示例(Ansible by Examples)](https://www.amazon.com/dp/B0B18QG44S)
*   [Ansible For PostgreSQL by Examples:10 多个自动化 PostgreSQL 数据库的示例](https://www.amazon.com/dp/B0B3RZZQVG)

# 捐赠

[](https://patreon.com/lucaberton) [## 卢卡·伯尔顿正在为 Ansible | Patreon 创建软件开源

### 今天就成为卢卡·伯尔顿的赞助人:获得世界上最大会员的独家内容和体验…

patreon.com](https://patreon.com/lucaberton) [](https://github.com/sponsors/lucab85) [## GitHub 赞助商上的赞助商@lucab85

### 我是一个活跃的开源贡献者，参与到了 Ansible 社区中，尽管我到处都是。@lucab85 的…

github.com](https://github.com/sponsors/lucab85) ![](img/8d20f9a30c97b0a0b09ee0ac9eba4cdf.png)