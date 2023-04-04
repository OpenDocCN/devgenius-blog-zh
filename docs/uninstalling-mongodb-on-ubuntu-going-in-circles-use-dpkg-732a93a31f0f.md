# 在 Ubuntu 上卸载 MongoDB:兜圈子？用 dpkg！

> 原文：<https://blog.devgenius.io/uninstalling-mongodb-on-ubuntu-going-in-circles-use-dpkg-732a93a31f0f?source=collection_archive---------5----------------------->

Tldr 使用' dpkg -l | grep mongo '列出所有软件包，然后使用' sudo dpkg -r packageName '逐个删除它们。

![](img/8a0ad06c99cc6b22e4b1e9fe4b340331.png)

鲁拜图·阿扎德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 问题是

最近，我试图安装 python 3.9，但是在我尝试的过程中，我碰到了相当不幸的未解决的依赖问题。

```
sudo apt install python3.9
[sudo] password for shouv:
Reading package lists... Done
Building dependency tree
Reading state information... Done
You might want to run 'apt --fix-broken install' to correct these.
The following packages have unmet dependencies:
 mongodb-org-database : Depends: mongodb-org-server but it is not going to be installed
                        Depends: mongodb-org-mongos but it is not going to be installed
 mongodb-org-tools : Depends: mongodb-database-tools but it is not going to be installed
 python3.9 : Depends: python3.9-minimal (= 3.9.14-1+focal1) but it is not going to be installed
             Depends: libpython3.9-stdlib (= 3.9.14-1+focal1) but it is not going to be installed
E: Unmet dependencies. Try 'apt --fix-broken install' with no packages (or specify a solution).
```

然后我想哼，apt 坏了，简单的一行修复——只需要复制并运行命令来修复坏的安装。

但是 Ubuntu 神有其他的计划……

```
sudo apt --fix-broken install
Reading package lists... Done
Building dependency tree
Reading state information... Done
Correcting dependencies... Done
The following packages were automatically installed and are no longer required:
  golang-1.13-go golang-1.13-race-detector-runtime golang-1.13-src golang-race-detector-runtime golang-src
  libboost-filesystem1.71.0 libboost-iostreams1.71.0 libboost-program-options1.71.0 libgoogle-perftools4 libsnappy1v5
  libtcmalloc-minimal4 libyaml-cpp0.6 mongo-tools mongodb-server-core
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  mongodb-database-tools mongodb-org-mongos mongodb-org-server
The following NEW packages will be installed:
  mongodb-database-tools mongodb-org-mongos mongodb-org-server
0 upgraded, 3 newly installed, 0 to remove and 411 not upgraded.
6 not fully installed or removed.
Need to get 0 B/92.8 MB of archives.
After this operation, 187 MB of additional disk space will be used.
Do you want to continue? [Y/n] Y
(Reading database ... 138597 files and directories currently installed.)
Preparing to unpack .../mongodb-org-server_5.0.12_amd64.deb ...
Unpacking mongodb-org-server (5.0.12) ...
dpkg: error processing archive /var/cache/apt/archives/mongodb-org-server_5.0.12_amd64.deb (--unpack):
 trying to overwrite '/usr/bin/mongod', which is also in package mongodb-server-core 1:3.6.9+really3.6.8+90~g8e540c0b6d-0ubuntu5
dpkg-deb: error: paste subprocess was killed by signal (Broken pipe)
Preparing to unpack .../mongodb-org-mongos_5.0.12_amd64.deb ...
Unpacking mongodb-org-mongos (5.0.12) ...
dpkg: error processing archive /var/cache/apt/archives/mongodb-org-mongos_5.0.12_amd64.deb (--unpack):
 trying to overwrite '/usr/bin/mongos', which is also in package mongodb-server-core 1:3.6.9+really3.6.8+90~g8e540c0b6d-0ubuntu5
dpkg-deb: error: paste subprocess was killed by signal (Broken pipe)
Preparing to unpack .../mongodb-database-tools_100.6.0_amd64.deb ...
Unpacking mongodb-database-tools (100.6.0) ...
dpkg: error processing archive /var/cache/apt/archives/mongodb-database-tools_100.6.0_amd64.deb (--unpack):
 trying to overwrite '/usr/bin/bsondump', which is also in package mongo-tools 3.6.3-0ubuntu1
dpkg-deb: error: paste subprocess was killed by signal (Broken pipe)
Errors were encountered while processing:
 /var/cache/apt/archives/mongodb-org-server_5.0.12_amd64.deb
 /var/cache/apt/archives/mongodb-org-mongos_5.0.12_amd64.deb
 /var/cache/apt/archives/mongodb-database-tools_100.6.0_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

所以 mongo-tools 固执的不让 bsondup 被覆盖，现在怎么办？

所以我想，让我们净化吧！

```
sudo apt-get autoremove --purge mongo*
Reading package lists... Done
Building dependency tree
Reading state information... Done
You might want to run 'apt --fix-broken install' to correct these.
The following packages have unmet dependencies:
 mongodb-org-database : Depends: mongodb-org-server but it is not going to be installed
                        Depends: mongodb-org-mongos but it is not going to be installed
 mongodb-org-tools : Depends: mongodb-database-tools but it is not going to be installed
E: Unmet dependencies. Try 'apt --fix-broken install' with no packages (or specify a solution).
```

你现在应该明白了。每次我试图删除/卸载/清除它给我未满足的依赖错误，并指示我修复损坏的安装。而且每次我试图修复坏掉的安装 mongo 都会保护自己不被删除。

因此，在尝试了许多解决方案并兜了一会儿圈子后，我想尝试一下老 dkpg，它成功了！

# 解决方案

所以从列出你所有的 mongo 包开始。

```
dpkg -l | grep mongo
ii  mongo-tools                          3.6.3-0ubuntu1                              amd64        collection of tools for administering MongoDB servers
iU  mongodb-mongosh                      1.5.0                                       amd64        MongoDB Shell CLI REPL Package
iU  mongodb-org                          5.0.9                                       amd64        MongoDB open source document-oriented database system (metapackage)
iU  mongodb-org-database                 5.0.9                                       amd64        MongoDB open source document-oriented database system (metapackage)
iU  mongodb-org-database-tools-extra     5.0.9                                       amd64        Extra MongoDB database tools
iU  mongodb-org-shell                    5.0.9                                       amd64        MongoDB shell client
iU  mongodb-org-tools                    5.0.9                                       amd64        MongoDB tools
rc  mongodb-server                       1:3.6.9+really3.6.8+90~g8e540c0b6d-0ubuntu5 all          object/document-oriented database (managed server package)
ii  mongodb-server-core                  1:3.6.9+really3.6.8+90~g8e540c0b6d-0ubuntu5 amd64        object/document-oriented database (server binaries package)
```

然后一个一个的去掉！

```
sudo dpkg -r mongodb-org
(Reading database ... 138573 files and directories currently installed.)
Removing mongodb-org (5.0.9) ...
```

但是要注意，有些包可能依赖于另一个包——在这种情况下，您需要首先删除它所依赖的包。

比如我撞见了这个。

```
sudo dpkg -r mongodb-mongosh
dpkg: dependency problems prevent removal of mongodb-mongosh:
 mongodb-org depends on mongodb-mongosh; however:
  Package mongodb-mongosh is to be removed.dpkg: error processing package mongodb-mongosh (--remove):
 dependency problems - not removing
Errors were encountered while processing:
 mongodb-mongosh
```

所以我们需要先删除 mongodb-org，然后才能删除 mongodb-mongosh。

希望这有所帮助，使用 dpkg 可以很好地解决类似的问题。愿编程大神保护我们不要兜圈子(但我们都知道无论如何都会发生！).