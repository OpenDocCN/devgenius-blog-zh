# 在 Linux 上管理 RAID

> 原文：<https://blog.devgenius.io/managing-raid-on-linux-d05dc4a482f0?source=collection_archive---------13----------------------->

在[上一篇文章](/virtual-machine-disk-performance-testing-on-cloud-b7a7c2e62a6f)中，我们测试了单盘的性能。然而，在现实世界中，磁盘有时会发生故障，导致磁盘丢失。 [RAID](https://en.wikipedia.org/wiki/RAID) 的创建是为了使用廉价的磁盘保护数据免受磁盘故障的影响。一般有硬件 RAID 和软件 RAID。这篇文章讲的是软件 RAID。有关硬件和软件 RAID 之间的差异，请参考附录中的文章。

# RAID 级别

要理解 RAID，您需要理解条带化、镜像和奇偶校验。它们在文章中有描述。

[](http://www.freeraidrecovery.com/library/what-is-raid.aspx) [## RAID 基础知识-什么是 RAID？

### RAID 是独立(或廉价)磁盘冗余阵列的缩写。事实上，RAID 是结合…

www.freeraidrecovery.com](http://www.freeraidrecovery.com/library/what-is-raid.aspx) [](https://www.storagetutorials.com/understanding-concept-striping-mirroring-parity/) [## 了解条带化、镜像和奇偶校验的概念

### 如果您想成为一名存储管理员或 SAN、NAS 专家，那么您应该掌握一些基本的…

www.storagetutorials.com](https://www.storagetutorials.com/understanding-concept-striping-mirroring-parity/) 

RAID 有不同的级别。

[](https://en.wikipedia.org/wiki/Standard_RAID_levels) [## 标准 RAID 级别-维基百科

### 在计算机存储中，标准 RAID 级别包括一组基本的 RAID(“独立磁盘冗余阵列”或…

en.wikipedia.org](https://en.wikipedia.org/wiki/Standard_RAID_levels) [](https://www.thegeekstuff.com/2010/08/raid-levels-tutorial/) [## 用图表解释 RAID 0、RAID 1、RAID 5、RAID 10

### RAID 代表廉价(独立)磁盘冗余阵列。在大多数情况下，你会使用…

www.thegeekstuff.com](https://www.thegeekstuff.com/2010/08/raid-levels-tutorial/) 

以我的经验来看，RAID 0，1，5，10 是比较常见的。如果需要容错，可以用 1，5，10。一般如果有钱，RAID 10 提供容错以及良好的读/写性能。你可以看到下面的区别。

[](https://www.dataplugs.com/en/raid-level-comparison-raid-0-raid-1-raid-5-raid-6-raid-10/) [## RAID 级别比较:RAID 0、RAID 1、RAID 5、RAID 6 和 RAID 10

### 在之前的文章中，我们已经讨论了硬件 RAID 和软件 RAID 的区别。我们将比较…

www.dataplugs.com](https://www.dataplugs.com/en/raid-level-comparison-raid-0-raid-1-raid-5-raid-6-raid-10/) [](https://techgenix.com/raid-10-vs-raid-5/) [## RAID 10 与 RAID 5:何时使用每个级别，为什么

### RAID 是一种技术，它通过预先确定的配置或级别将不同形式的磁盘阵列组合在一起，以…

techgenix.com](https://techgenix.com/raid-10-vs-raid-5/) 

在 Linux 中， [mdadm](https://raid.wiki.kernel.org/index.php/A_guide_to_mdadm) 用于管理软件 RAID。接下来，我们将展示如何使用 mdadm 创建软件 RAID。

# 调配 Linux 虚拟机

让我们在 Azure 上创建 Ubuntu VM。

```
provider "azurerm" {
 # The "feature" block is required for AzureRM provider 2.x.
 # If you are using version 1.x, the "features" block is not allowed.
 version = "~>2.0"
 features {}
}
locals {
 myterraformgroup = "rg1"
}resource "azurerm_virtual_network" "myterraformnetwork" {
 name = "myVnet"
 address_space = ["10.0.0.0/16"]
 location = "eastus"
 resource_group_name = local.myterraformgroup
tags = {
 environment = "Terraform Demo"
 }
}
# Bastion subnet
resource "azurerm_subnet" "bastionsubnet" {
 name = "AzureBastionSubnet"
 resource_group_name = local.myterraformgroup
 virtual_network_name = azurerm_virtual_network.myterraformnetwork.name
 address_prefixes = ["10.0.200.0/27"]
}
resource "azurerm_public_ip" "bastionip" {
 name = "bastionip"
 location = azurerm_virtual_network.myterraformnetwork.location
 resource_group_name = local.myterraformgroup
 allocation_method = "Static"
 sku = "Standard"
}
resource "azurerm_bastion_host" "bastionhost" {
 name = "myvmbastion"
 location = azurerm_virtual_network.myterraformnetwork.location
 resource_group_name = local.myterraformgroup
ip_configuration {
 name = "configuration"
 subnet_id = azurerm_subnet.bastionsubnet.id
 public_ip_address_id = azurerm_public_ip.bastionip.id
 }
}
# Create worker subnet
resource "azurerm_subnet" "myterraformsubnet" {
 name = "mySubnet"
 resource_group_name = local.myterraformgroup
 virtual_network_name = azurerm_virtual_network.myterraformnetwork.name
 address_prefixes = ["10.0.1.0/24"]
}
# Create Network Security Group and rule
resource "azurerm_network_security_group" "myterraformnsg" {
 name = "myNetworkSecurityGroup"
 location = "eastus"
 resource_group_name = local.myterraformgroup
security_rule {
 name = "SSH"
 priority = 1001
 direction = "Inbound"
 access = "Allow"
 protocol = "Tcp"
 source_port_range = "*"
 destination_port_range = "22"
 source_address_prefix = "*"
 destination_address_prefix = "*"
 }
tags = {
 environment = "Terraform Demo"
 }
}
# Create network interface
resource "azurerm_network_interface" "myterraformnic" {
 name = "myNIC"
 location = "eastus"
 resource_group_name = local.myterraformgroup
ip_configuration {
 name = "myNicConfiguration"
 subnet_id = azurerm_subnet.myterraformsubnet.id
 private_ip_address_allocation = "Dynamic"
 }
tags = {
 environment = "Terraform Demo"
 }
}
# Connect the security group to the network interface
resource "azurerm_network_interface_security_group_association" "example" {
 network_interface_id = azurerm_network_interface.myterraformnic.id
 network_security_group_id = azurerm_network_security_group.myterraformnsg.id
}
# Generate random text for a unique storage account name
resource "random_id" "randomId" {
 keepers = {
 # Generate a new ID only when a new resource group is defined
 resource_group = local.myterraformgroup
 }
byte_length = 8
}
# Create storage account for boot diagnostics
resource "azurerm_storage_account" "mystorageaccount" {
 name = "diag${random_id.randomId.hex}"
 resource_group_name = local.myterraformgroup
 location = "eastus"
 account_tier = "Standard"
 account_replication_type = "LRS"
tags = {
 environment = "Terraform Demo"
 }
}
# Create (and display) an SSH key
resource "tls_private_key" "example_ssh" {
 algorithm = "RSA"
 rsa_bits = 4096
}
output "tls_private_key" { 
 value = nonsensitive(tls_private_key.example_ssh.private_key_pem)
}
# Create virtual machine
resource "azurerm_linux_virtual_machine" "myterraformvm" {
 name = "myVM"
 location = "eastus"
 resource_group_name = local.myterraformgroup
 network_interface_ids = [azurerm_network_interface.myterraformnic.id]
 size = "Standard_D4as_v5"
os_disk {
 name = "myOsDisk"
 caching = "ReadWrite"
 storage_account_type = "Premium_LRS"
 }
source_image_reference {
 publisher = "Canonical"
 offer = "UbuntuServer"
 sku = "18.04-LTS"
 version = "latest"
 }
computer_name = "myvm"
 admin_username = "azureuser"
 disable_password_authentication = true
admin_ssh_key {
 username = "azureuser"
 public_key = tls_private_key.example_ssh.public_key_openssh
 }
boot_diagnostics {
 storage_account_uri = azurerm_storage_account.mystorageaccount.primary_blob_endpoint
 }
tags = {
 environment = "Terraform Demo"
 }
}
resource "azurerm_managed_disk" "example1" {
 name = "myVM-disk1"
 location = "eastus"
 resource_group_name = local.myterraformgroup
 storage_account_type = "Premium_LRS"
 create_option = "Empty"
 disk_size_gb = 2048
}
resource "azurerm_virtual_machine_data_disk_attachment" "example1" {
 managed_disk_id = azurerm_managed_disk.example1.id
 virtual_machine_id = azurerm_linux_virtual_machine.myterraformvm.id
 lun = "10"
 caching = "ReadWrite"
}
resource "azurerm_managed_disk" "example2" {
 name = "myVM-disk2"
 location = "eastus"
 resource_group_name = local.myterraformgroup
 storage_account_type = "Premium_LRS"
 create_option = "Empty"
 disk_size_gb = 2048
}
resource "azurerm_virtual_machine_data_disk_attachment" "example2" {
 managed_disk_id = azurerm_managed_disk.example2.id
 virtual_machine_id = azurerm_linux_virtual_machine.myterraformvm.id
 lun = "20"
 caching = "ReadWrite"
}
```

# 管理磁盘和 RAID

我们创建了 2 个 2TB 磁盘，lsblk 显示了以下结果(2 个磁盘以粗体显示)。

```
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
sda 8:0 0 30G 0 disk 
├─sda1 8:1 0 29.9G 0 part /
├─sda14 8:14 0 4M 0 part 
└─sda15 8:15 0 106M 0 part /boot/efi
**sdb 8:16 0 2T 0 disk 
sdc 8:32 0 2T 0 disk** 
sr0 11:0 1 628K 0 rom
```

## 安装 mdadm

```
sudo apt update -y
sudo apt install mdadm
```

下面的文章对 mdadm 有详细的解释。

[](https://www.digitalocean.com/community/tutorials/how-to-create-raid-arrays-with-mdadm-on-ubuntu-16-04) [## 如何在 Ubuntu 16.04 | DigitalOcean 上使用 mdadm 创建 RAID 阵列

### mdadm 实用程序可用于使用 Linux 的软件 RAID 功能创建和管理存储阵列…

www.digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-create-raid-arrays-with-mdadm-on-ubuntu-16-04)  [## Ubuntu 联机帮助页:mdadm -管理 MD 设备，也称为 Linux 软件 RAID

### RAID 设备是从两个或更多真实块设备创建的虚拟设备。这允许多个设备(通常…

manpages.ubuntu.com](https://manpages.ubuntu.com/manpages/xenial/man8/mdadm.8.html) 

*注意:数组一旦创建就可以使用。不需要等待初始重新同步完成。*

mdadm create raid 命令非常简单:指定 raid 设备、raid 级别、磁盘数量，并指定磁盘列表。

## RAID 1

```
# create raid 1
sudo mdadm — create — verbose /dev/md1 — level=1 — raid-devices=2 /dev/sdb /dev/sdc
```

使用/proc/mdstat 进行监视

```
# monitor
cat /proc/mdstat
```

结果

```
Personalities : [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10] 
md1 : active raid1 sdc[1] sdb[0]
 2147351552 blocks super 1.2 [2/2] [UU]
 [>………………..] resync = 0.6% (14161664/2147351552) finish=223.1min speed=159297K/sec
 bitmap: 16/16 pages [64KB], 65536KB chunkunused devices: <none>Progress is 0.6%
```

使用 mdadm 监控

```
sudo mdadm — detail /dev/md1
```

结果。您可以看到它显示了重新同步状态和 RAID 底层磁盘

```
/dev/md1:
 Version : 1.2
 Creation Time : Wed Jul 13 16:18:50 2022
 Raid Level : raid1
 Array Size : 2147351552 (2047.87 GiB 2198.89 GB)
 Used Dev Size : 2147351552 (2047.87 GiB 2198.89 GB)
 Raid Devices : 2
 Total Devices : 2
 Persistence : Superblock is persistentIntent Bitmap : InternalUpdate Time : Wed Jul 13 16:23:06 2022
 State : clean, resyncing 
 Active Devices : 2
 Working Devices : 2
 Failed Devices : 0
 Spare Devices : 0Consistency Policy : bitmap*Resync Status : 1% complete*Name : myvm:1 (local to host myvm)
 UUID : 4a5bacfc:311c2ef7:c31657c1:e2b3ac18
 Events : 51Number Major Minor RaidDevice State
 *0 8 16 0 active sync /dev/sdb
 1 8 32 1 active sync /dev/sdc*
```

创建文件系统

```
sudo mkfs.xfs /dev/md1
```

lsblk 结果

```
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
sda 8:0 0 30G 0 disk 
├─sda1 8:1 0 29.9G 0 part /
├─sda14 8:14 0 4M 0 part 
└─sda15 8:15 0 106M 0 part /boot/efi
sdb 8:16 0 2T 0 disk 
└─md1 9:1 0 2T 0 raid1 
sdc 8:32 0 2T 0 disk 
└─md1 9:1 0 2T 0 raid1 
sr0 11:0 1 628K 0 rom
```

装载 RAID

```
sudo mkdir /mnt/md1
sudo mount /dev/md1 /mnt/md1
```

现在当你跑的时候

```
df -h
```

你会看到 RAID 挂载。因为镜像使用一个磁盘作为镜像，所以实际可用空间只有 1 个磁盘

```
Filesystem Size Used Avail Use% Mounted on
udev 7.8G 0 7.8G 0% /dev
tmpfs 1.6G 704K 1.6G 1% /run
/dev/sda1 29G 1.6G 28G 6% /
tmpfs 7.9G 0 7.9G 0% /dev/shm
tmpfs 5.0M 0 5.0M 0% /run/lock
tmpfs 7.9G 0 7.9G 0% /sys/fs/cgroup
/dev/sda15 105M 4.4M 100M 5% /boot/efi
tmpfs 1.6G 0 1.6G 0% /run/user/1000
**/dev/md1 2.0T 2.1G 2.0T 1% /mnt/md1**
```

## RAID 0

RAID 0 是条带化的，因此可用磁盘空间等于底层磁盘的总磁盘空间。

```
sudo mdadm — create — verbose /dev/md0 — level=0 — raid-devices=2 /dev/sdb /dev/sdc# make fielsystem
sudo mkfs.xfs -f /dev/md0
```

lsblk 结果

```
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
sda 8:0 0 30G 0 disk 
├─sda1 8:1 0 29.9G 0 part /
├─sda14 8:14 0 4M 0 part 
└─sda15 8:15 0 106M 0 part /boot/efi
sdb 8:16 0 2T 0 disk 
└─md0 9:0 0 4T 0 raid0 
sdc 8:32 0 2T 0 disk 
└─md0 9:0 0 4T 0 raid0 
sr0 11:0 1 628K 0 rom
```

增加

```
sudo mkdir /mnt/md0
sudo mount /dev/md0 /mnt/md0
```

文件系统磁盘空间

```
# df -h
Filesystem Size Used Avail Use% Mounted on
udev 7.8G 0 7.8G 0% /dev
tmpfs 1.6G 704K 1.6G 1% /run
/dev/sda1 29G 1.6G 28G 6% /
tmpfs 7.9G 0 7.9G 0% /dev/shm
tmpfs 5.0M 0 5.0M 0% /run/lock
tmpfs 7.9G 0 7.9G 0% /sys/fs/cgroup
/dev/sda15 105M 4.4M 100M 5% /boot/efi
tmpfs 1.6G 0 1.6G 0% /run/user/1000
**/dev/md0 4.0T 4.2G 4.0T 1% /mnt/md0**
```

## 其他 mdadm 命令

```
sudo mdadm — stop /dev/md1
sudo mdadm — remove /dev/md1
```

如果您需要自动化 mdadm 脚本，如果您 mdadm —停止并 mdadm —再次创建，您将看到“<>似乎是 raid 阵列的一部分，继续创建阵列”，有趣的是，mdadm 没有强制或静默继续的标志，这里有一个智能的[文章](https://stackoverflow.com/questions/8707156/scripting-mdadm-when-a-component-device-may-contain-ext2-file-system-already)来解决这个问题。

# 附录

硬件 RAID 与软件 RAID

[](https://www.geeksforgeeks.org/difference-between-hardware-raid-vs-software-raid/) [## 硬件 RAID 与软件 RAID 的区别

### 独立磁盘冗余阵列(RAID)是一种虚拟磁盘技术，它将多个物理驱动器组合成一个…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/difference-between-hardware-raid-vs-software-raid/)  [## 4.3.硬件 RAID 与软件 RAID

### 4.3.硬件 RAID 与软件 RAID 4.3。硬件 RAID 与软件 RAID 有两种可能的 RAID 方法…

web.mit.edu](https://web.mit.edu/rhel-doc/5/RHEL-5-manual/Deployment_Guide-en-US/s1-raid-approaches.html) [](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-s390info-raid) [## 22.3.使用 mdadm 配置基于 RAID 的多路径存储 Red Hat Enterprise Linux 5 | Red…

### 与组成 raidtools 包集的其他工具类似，mdadm 命令可用于执行所有必要的…

access.redhat.com](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-s390info-raid)