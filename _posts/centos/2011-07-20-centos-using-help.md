---
layout: post
title: CentOS 的使用帮助
tags: [centos]
---

## 1. 备份

  * [BackupPC](http://wiki.centos.org/zh/HowTos/BackupPC)

  * [以远程备份选项进行循环式备份](http://wiki.centos.org/zh/HowTos/Rotational_backup_with_remote_backup_options)

  * [利用 Amanda 进行备份](http://wiki.centos.org/zh/HowTos/AmandaBackups)

  * [利用 rsnapshot 进行备份](http://wiki.centos.org/zh/HowTos/RsnapshotBackups)

## 2. 目录服务

  * [设置 CentOS 目录服务器](http://wiki.centos.org/zh/HowTos/DirectoryServerSetup)

## 3. 无盘客户端

  * [运用 NFS 及 PXE 的无盘客户端](http://wiki.centos.org/zh/HowTos/DisklessClients)

  * [运用 K12LTSP 的无盘客户端](http://wiki.centos.org/zh/HowTos/DisklessClients/K12LTSP)

## 4. 电邮

  * [vpopmail](http://wiki.centos.org/zh/HowTos/vpopmail)

  * [Postfix](http://wiki.centos.org/zh/HowTos/postfix)：一个简单的 Postfix／Dovcot 电邮服务器

  * [Postfix 规限](http://wiki.centos.org/zh/HowTos/postfix_restrictions)：利用 Postfix 简单地过滤垃圾邮件

  * [Postgrey](http://wiki.centos.org/zh/HowTos/postgrey)：在 Postfix 上以 Postgrey 的灰名单过滤垃圾邮件

  * [Postfix SASL & SSL/TLS](http://wiki.centos.org/zh/HowTos/postfix_sasl)：为 postfix 及 dovecot 设置 SASL 与 SSL/TLS

  * [Postfix SASL Relayhost](http://wiki.centos.org/zh/HowTos/postfix_sasl_relayhost)：以 SASL 连接至转发站

  * [Amavisd](http://wiki.centos.org/zh/HowTos/Amavisd)：为 ClamAV 及 SpamAssassin 设置 Amavisd

## 5. 加密

  * [将 /tmp、/swap 及 /home 加密](http://wiki.centos.org/zh/HowTos/EncryptTmpSwapHome)

  * [将你的文件系统加密](http://wiki.centos.org/zh/HowTos/EncryptedFilesystem)

## 6. FTP

  * [支持非系统用户的 vsftpd](http://wiki.centos.org/zh/HowTos/Chroot_Vsftpd_with_non-system_users)

  * 无须 chroot 的[简易 SFTP 设置](http://wiki.centos.org/zh/HowTos/sftp)

## 7. 网页服务器

  * [在 CentOS 设置一个 SSL 加密的网页服务器](http://wiki.centos.org/zh/HowTos/Https)

  * [在 CentOS 上为 apache httpd 设置 kerberos 验证](http://wiki.centos.org/zh/HowTos/HttpKerberosAuth)

## 8. 内核

  * [我需要内核的源代码](http://wiki.centos.org/zh/HowTos/I_need_the_Kernel_Source)

  * [我需要创建一个自设的内核](http://wiki.centos.org/zh/HowTos/Custom_Kernel)

  * [创建你自己的内核模块](http://wiki.centos.org/zh/HowTos/BuildingKernelModules)

## 9. 网络

  * [在 CentOS 设置 iptables](http://wiki.centos.org/zh/HowTos/Network/IPTables)

  * [利用 Heartbeat／DRBD 设置一个高可用性的群集](http://wiki.centos.org/zh/HowTos/Ha-Drbd)

  * [令无线网络在你的笔记型（或桌上）计算机上运作](http://wiki.centos.org/zh/HowTos/Laptops/Wireless)

  * [启用 NetworkManager](http://wiki.centos.org/zh/HowTos/Laptops/NetworkManager)

  * [启用 wpa_supplicant 而不需要 NetworkManager](http://wiki.centos.org/zh/HowTos/Laptops/WpaSupplicant)

## 10. 安全性

  * [将 CentOS 加固](http://wiki.centos.org/zh/HowTos/OS_Protection)

  * [SELinux 指南](http://wiki.centos.org/zh/HowTos/SELinux)

  * [保卫你系统上的 SSH](http://wiki.centos.org/zh/HowTos/Network/SecuringSSH)

  * [在 CentOS 设置 iptables](http://wiki.centos.org/zh/HowTos/Network/IPTables)

  * [在 CentOS 5.x 上使用 vpnc](http://wiki.centos.org/zh/HowTos/vpnc)

## 11. 远程 X 工作阶段

  * [FreeNX](http://wiki.centos.org/zh/HowTos/FreeNX)

  * [VNC](http://wiki.centos.org/zh/HowTos/VNC-Server)

## 12. 移植

  * [移植指南](http://wiki.centos.org/zh/HowTos/MigrationGuide)

  * [将 ServerCD 4.4 移植至 CentOS 5](http://wiki.centos.org/zh/HowTos/MigrationGuide/ServerCD_4.4_to_5)

  * [CentOS 结束支持的选择](http://wiki.centos.org/zh/HowTos/EOL)

## 13. 组件管理

  * [Yum](http://wiki.centos.org/zh/PackageManagement/Yum)

  * [RPM](http://wiki.centos.org/zh/PackageManagement/Rpm)

  * [组件管理](http://wiki.centos.org/zh/PackageManagement)

  * [Fastest Mirror](http://wiki.centos.org/zh/PackageManagement/Yum/FastestMirror)：一个断定及采用最快的 CentOS 镜像的 yum 插件

  * [Priorities](http://wiki.centos.org/zh/PackageManagement/Yum/Priorities)：一个容让软件库被赋予 1 至 99 级优先次序的 yum 插件 …… 不可与 protectbase 同时使用，但已内置同样的功能

  * [Protect Base](http://wiki.centos.org/zh/PackageManagement/Yum/ProtectBase)：一个防止外置软件库取替 CentOS 组件的 yum 插件

  * [YumCheckOrInstallUpdates](http://wiki.centos.org/zh/YumCheckOrInstallUpdates)：自动检查或安装更新的脚本

  * [由源代码安装](http://wiki.centos.org/zh/PackageManagement/SourceInstalls)：如何（不）在 CentOS 上由源代码安装软件

  * [重建一个源代码 RPM（SRPM）组件](http://wiki.centos.org/zh/HowTos/RebuildSRPM)：如何在 CentOS 上重建一个源代码 RPM 组件

  * [RHEL 上的 Yum](http://wiki.centos.org/zh/HowTos/PackageManagement/YumOnRHEL)：在 RHEL 的机器上采用 CentOS 的软件库

  * [创建一个供更新或安装用的本地镜像](http://wiki.centos.org/zh/HowTos/CreateLocalMirror)

  * [为你自己的组件创建一个本地软件库](http://wiki.centos.org/zh/HowTos/CreateLocalRepos)

  * [CentOS 软件库](http://wiki.centos.org/zh/AdditionalResources/Repositories)

  * [Spacewalk](http://wiki.centos.org/zh/HowTos/PackageManagement/Spacewalk)

  * [创建一台网络安装服务器](http://wiki.centos.org/zh/HowTos/NetworkInstallServer)

## 14. 虚拟化

  * [引言](http://wiki.centos.org/zh/HowTos/Virtualization/Introduction)

  * [CentOS-6 的 Xen4 快速入门](http://wiki.centos.org/zh/HowTos/Xen/Xen4QuickStart)

  * [将一个原生的 CentOS 安装移进一台 Xen DomU 内](http://wiki.centos.org/zh/HowTos/Xen/MoveNative2DomU)

  * [安装及使用一台 CentOS 5 Xen DomU](http://wiki.centos.org/zh/HowTos/Xen/InstallingCentOSDomU)

  * [安装及运用一台全虚拟化的 Xen 客端系统](http://wiki.centos.org/zh/HowTos/Xen/InstallingHVMDomU)

  * [在 CentOS 上安装及运用 KVM](http://wiki.centos.org/zh/HowTos/KVM)

  * [同时使用 Nvidia 驱动程序及 Xen](http://wiki.centos.org/zh/HowTos/Xen/NvidiaWithXen)

  * [CentOS 上旳 Vserver](http://wiki.centos.org/zh/HowTos/Virtualization/Vserver)

  * [CentOS 上的 OpenVZ](http://wiki.centos.org/zh/HowTos/Virtualization/OpenVZ)

  * [CentOS 上的 VirtualBox](http://wiki.centos.org/zh/HowTos/Virtualization/VirtualBox)

  * [CentOS 作为 VirtualBox 的客端操作系统](http://wiki.centos.org/zh/HowTos/Virtualization/VirtualBox/CentOSguest)

  * [通过 LibVirt 在虚拟机器上运用 Spice（CentOS-6.2 或以上）](http://wiki.centos.org/zh/HowTos/Spice-libvirt)

  * [CentOS-6 上的嵌套式虚拟](http://wiki.centos.org/zh/HowTos/NestedVirt)

  * [CentOS-6 上的 Linux 容器（LXC）虚拟化](http://wiki.centos.org/zh/HowTos/LXC-on-CentOS6)

## 15. PXE

  * [CentOS 上执行 PXE](http://wiki.centos.org/zh/HowTos/PXE)

## 16. 存储器、逻辑磁盘区的管理、及磁盘数组

  * [如何在 CentOS 5 上设置一个软件磁盘数组](http://wiki.centos.org/zh/HowTos/SoftwareRAIDonCentOS5)

  * [如何在一个可分割的软件磁盘数组上安装 CentOS](http://wiki.centos.org/zh/HowTos/Install_On_Partitionable_RAID1)

  * [如何利用修复光盘将一个 CentOS 5 系统转换成 RAID1](http://wiki.centos.org/zh/HowTos/CentOS5ConvertToRAID)

  * [优化磁盘／文件系统](http://wiki.centos.org/zh/HowTos/Disk_Optimization)

## 17. 杂项

  * [CentOS 上的 Subversion](http://wiki.centos.org/zh/HowTos/Subversion)

  * [CentOS 4 及 CentOS 5 上的 Java](http://wiki.centos.org/zh/HowTos/JavaOnCentOS)

  * [如何安装来自 Java.com 的 Java Runtime Environment（JRE）并设置插件](http://wiki.centos.org/zh/HowTos/JavaRuntimeEnvironment)

  * [设置一张新的显示卡](http://wiki.centos.org/zh/HowTos/ConfigureNewVideoCard)

  * [设置一支 USB 存储器来安装 CentOS](http://wiki.centos.org/zh/HowTos/InstallFromUSBkey)

  * [CentOS 5 及 6 上安装 Grub](http://wiki.centos.org/zh/HowTos/GrubInstallation)

  * [在 CentOS 上安装 FOG 计算机映像方案](http://wiki.centos.org/zh/HowTos/Fog)

## 18. 非 CentOS 的应用程序

  * [CentOS 上的 Nagios](http://wiki.centos.org/zh/HowTos/Nagios)

  * [CentOS 4.x 上的 RT 3.4.x](http://wiki.centos.org/zh/HowTos/RT_3.4.x_On_CentOS_4.x)

  * [CentOS 4.x 上的 Cacti](http://wiki.centos.org/zh/HowTos/Cacti_on_CentOS_4.x)

  * [CentOS 5.x 上的 OCS Inventory NG](http://wiki.centos.org/zh/HowTos/OCSNG)

  * [CentOS 上的 Skype](http://wiki.centos.org/zh/HowTos/Skype)

## 19. 在笔记本上执行 CentOS

  * [笔记本上的 CentOS](http://wiki.centos.org/zh/HowTos/Laptops)

## 20. Intel 结构的 Apple 计算机上的 CentOS

  * [iMac／Mactel 上的 CentOS](http://wiki.centos.org/zh/HowTos/Mactel)

## 21. CentOS 指南

一些关于 CentOS 标准的文档：

  * [如何编辑 CentOS 的 Wiki](http://wiki.centos.org/zh/HowTos/Wiki/Editing)

  * [如何将你的 RPM 贡献给 CentOS 计划](http://wiki.centos.org/zh/HowTos/Packages/ContributeYourRPMs)

  * [如何为 CentOS 创建公共镜像](http://wiki.centos.org/zh/HowTos/CreatePublicMirrors)

* * *