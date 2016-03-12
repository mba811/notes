---
layout: post
title: Ubuntu 服务器入门指南
tags: [ubuntu]
---

From: <http://wiki.ubuntu.org.cn/Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97>

### 出自Ubuntu中文 

Ubuntu 服务器指南 

## 目录 

  * 1 关于本指南
    * 1.1 排版约定
    * 1.2 贡献和反馈
  * 2 介绍
  * 3 安装
    * 3.1 准备安装
      * 3.1.1 系统要求
      * 3.1.2 备份
    * 3.2 从 CD 安装
  * 4 包管理
    * 4.1 介绍
    * 4.2 Apt-Get
    * 4.3 Aptitude
    * 4.4 配置
      * 4.4.1 3.5.1. 引用
  * 5 联网
    * 5.1 网络配置
      * 5.1.1 4.1.1. 以太网
      * 5.1.2 4.1.2. 管理 DNS 记录
      * 5.1.3 4.1.3. 管理主机
    * 5.2 TCP/IP
      * 5.2.1 4.2.1. TCP/IP 介绍
      * 5.2.2 4.2.2. TCP/IP 配置
      * 5.2.3 4.2.3. IP 路由
      * 5.2.4 4.2.4. TCP 和 UDP
      * 5.2.5 4.2.5.ICMP
      * 5.2.6 4.2.6. 守护程序
    * 5.3 OpenSSH 服务器
      * 5.3.1 4.4.1. 介绍
      * 5.3.2 4.4.2. 安装
      * 5.3.3 4.4.3. 配置
      * 5.3.4 4.4.4. 引用
    * 5.4 FTP 服务器
      * 5.4.1 4.5.1. vsftpd - FTP 服务器安装
      * 5.4.2 4.5.2. vsftpd - FTP 服务器配置
    * 5.5 网络文件系统 (NFS)
      * 5.5.1 4.6.1. 安装
      * 5.5.2 4.6.2. 配置
      * 5.5.3 4.6.3. NFS 客户端配置
      * 5.5.4 4.6.4. 引用
    * 5.6 动态主机配置协议 (DHCP)
      * 5.6.1 4.7.1. 安装
      * 5.6.2 4.7.2. 配置
      * 5.6.3 4.7.3. 引用
    * 5.7 域名解析服务 (DNS)
      * 5.7.1 4.8.1. 安装
      * 5.7.2 4.8.2. 配置
      * 5.7.3 4.8.3. 引用
    * 5.8 CUPS － 打印服务器
      * 5.8.1 4.9.1. 安装
      * 5.8.2 4.9.2. 配置
      * 5.8.3 4.9.3. 引用
    * 5.9 HTTPD - Apache2 Web 服务器
      * 5.9.1 4.10.1. 安装
      * 5.9.2 4.10.2. 配置
        * 5.9.2.1 4.10.2.1. 基本设置
        * 5.9.2.2 4.10.2.2. 缺省设置
        * 5.9.2.3 4.10.2.3. 虚拟主机设置
        * 5.9.2.4 4.10.2.4. 服务器设置
        * 5.9.2.5 4.10.2.5. Apache 模块
      * 5.9.3 4.10.3. HTTPS 配置
        * 5.9.3.1 4.10.3.1. 证书和安全
        * 5.9.3.2 4.10.3.2. 证书类型
        * 5.9.3.3 4.10.3.3. 生成一个证书签署请求 (CSR)
        * 5.9.3.4 4.10.3.4. 创建一个自己签署的证书
        * 5.9.3.5 4.10.3.5. 安装证书
        * 5.9.3.6 4.10.3.6. 访问服务器
      * 5.9.4 4.10.4. 引用
    * 5.10 Squid - 代理服务器
      * 5.10.1 4.11.1. 安装
      * 5.10.2 4.11.2. 配置
      * 5.10.3 4.11.3. 引用
    * 5.11 版本控制系统
      * 5.11.1 4.12.1. Subversion
        * 5.11.1.1 4.12.1.1. 安装
        * 5.11.1.2 4.12.1.2. 服务器配置
        * 5.11.1.3 4.12.1.3. 访问方式
        * 5.11.1.4 = 4.12.1.3.1. 直接访问库 (file://) =
        * 5.11.1.5 = 4.12.1.3.2. 通过 WebDAV 协议 (http://) 访问 =
        * 5.11.1.6 = 4.12.1.3.3. 通过带有 SSL 加密的 WebDAV 协议来访问 (https://) =
        * 5.11.1.7 = 4.12.1.3.4. 通过自身协议访问 (svn://) =
        * 5.11.1.8 = 4.12.1.3.5. 通过带有 SSL 加密的自身协议 (svn+ssh://) 访问 =
      * 5.11.2 4.12.2. CVS 服务器
        * 5.11.2.1 4.12.2.1. 安装
        * 5.11.2.2 4.12.2.2. 配置
        * 5.11.2.3 4.12.2.3. 添加项目
      * 5.11.3 4.12.3. 引用
    * 5.12 数据库
      * 5.12.1 4.13.1. MySQL
        * 5.12.1.1 4.13.1.1. 安装
        * 5.12.1.2 4.13.1.2. 配置
      * 5.12.2 4.13.2. PostgreSQL
        * 5.12.2.1 4.13.2.1. 安装
        * 5.12.2.2 4.13.2.2. 配置
    * 5.13 邮件服务
      * 5.13.1 4.14.1. Postfix
        * 5.13.1.1 4.14.1.1. 安装
        * 5.13.1.2 4.14.1.2. 基本配置
        * 5.13.1.3 4.14.1.3. SMTP 认证
        * 5.13.1.4 4.14.1.4. 配置 SASL
        * 5.13.1.5 4.14.1.5. 测试
      * 5.13.2 4.14.2. Exim4
        * 5.13.2.1 4.14.2.1. 安装
        * 5.13.2.2 4.14.2.2. 配置
      * 5.13.3 4.14.3. Dovecot 服务器
        * 5.13.3.1 4.14.3.1. 安装
        * 5.13.3.2 4.14.3.2. 配置
        * 5.13.3.3 4.14.3.3. Dovecot SSL 配置
        * 5.13.3.4 4.14.3.4. 邮件服务器的防火墙配置
      * 5.13.4 4.14.4. Mailman
        * 5.13.4.1 4.14.4.1. 安装
        * 5.13.4.2 = 4.14.4.1.1. Apache2 =
        * 5.13.4.3 = 4.14.4.1.2. Exim4 =
        * 5.13.4.4 = 4.14.4.1.3. Mailman =
        * 5.13.4.5 4.14.4.2. 配置
        * 5.13.4.6 = 4.14.4.2.1. Apache2 =
        * 5.13.4.7 = 4.14.4.2.2. Exim4 =
        * 5.13.4.8 = 4.14.4.2.3. 主 =
        * 5.13.4.9 = 4.14.4.2.4. 传输 =
        * 5.13.4.10 = 4.14.4.2.5. 路由器 =
        * 5.13.4.11 = 4.14.4.2.6. Mailman =
        * 5.13.4.12 4.14.4.3. 管理
        * 5.13.4.13 4.14.4.4. 用户
        * 5.13.4.14 4.14.4.5. 引用
  * 6 Windows 联网
    * 6.1 介绍
    * 6.2 安装 SAMBA
    * 6.3 配置 SAMBA
      * 6.3.1 5.3.1. 服务器
        * 6.3.1.1 5.3.1.1. Active Directory
        * 6.3.1.2 = 5.3.1.1.1. LDAP =
        * 6.3.1.3 = 5.3.1.1.2. Kerberos =
        * 6.3.1.4 5.3.1.2. 计算机帐号
        * 6.3.1.5 5.3.1.3. 文件权限
      * 6.3.2 5.3.2. 客户端
        * 6.3.2.1 5.3.2.1. 用户帐号
        * 6.3.2.2 5.3.2.2. 组
        * 6.3.2.3 5.3.2.3. 组策略

### [[编辑][1]] 关于本指南

   [1]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=1 (编辑段落：关于本指南)

#### [[编辑][2]] 排版约定

   [2]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=2 (编辑段落：排版约定)

本文档将使用下列记号： 备注，一般表示一段有趣的，有时是技术性的，并和上下文相关的信息。 提示，提供有完成某件事情的建议或捷径。 警示，提醒读者可能面临的问题，并给出避免这些问题的帮助信息。 警告，告知读者在特定情形下可能出现的风险。 

用于打印的交叉引用排版约定效果如下： 

  *     *       *         * 到其它文档和网站的链接看上去像这样。

PDF、HTML 以及 XHTML 版本的文档将使用超链接来处理交叉引用。 

输入内容的排版约定效果如下： 

  *     *       *         * 文件名或者目录路径，将以等宽字体(monospace)显示。
        * 您在终端(Terminal)里输入的命令显示效果如下：

`command to type`

  *     *       *         * 您在用户界面上点击、选择或选中的选项，将以等宽字体(monospace)显示。

菜单选择、鼠标动作及键盘快捷键： 

  *     *       *         * 菜单选择的序列操作显示效果如下：

文件(F) → 打开(O) 

  *     *       *         * 鼠标动作假定您使用右手模式的鼠标设定。文中述及的“单击”和“双击”皆使用鼠标左键。文中述及的“单击右键”指的是使用鼠标右键。文中述及的“单击中键”指的是使用鼠标中键、按下鼠标滚轮，或者是以模拟中键的方式同时按下鼠标左、右键，这取决于您的鼠标设计。
        * 键盘快捷键组合的显示效果如下：

Ctrl+N 。根据惯例，“Control”、“Shift” 以及 “Alternate” 按键将以 Ctrl、Shift 以及 Alt 来表示，需要特别指出的是，其中第一个按键在按下第二个键的过程中应该一直被按住。 

#### [[编辑][3]] 贡献和反馈

   [3]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=3 (编辑段落：贡献和反馈)

本文档由 [Ubuntu 文档小组][4] 编写。您可以通过 Ubuntu 文档小组的邮件列表发送自己的意见或评论来完善本文档。欲知该小组的信息、其邮件列表、项目等等详情，您可以访问 [Ubuntu 文档小组官方网站][4]。本文档的简体中文版由 [UbuntuChina 文档组][5]翻译，欲知更多信息或加入我们，请访问 [Ubuntu 中文 Wiki][6]。 

   [4]: https://wiki.ubuntu.com/DocumentationTeam (https://wiki.ubuntu.com/DocumentationTeam)
   [5]: http://wiki.ubuntu.org.cn/UbuntuChina%E6%96%87%E6%A1%A3%E7%BB%84 (http://wiki.ubuntu.org.cn/UbuntuChina%E6%96%87%E6%A1%A3%E7%BB%84)
   [6]: http://wiki.ubuntu.org.cn/ (http://wiki.ubuntu.org.cn/)

如果您发现了文档中存在的问题，或者想提些建议，您可以直接在[Ubuntu Bug][7] 跟踪系统上提交一份bug报告。您的帮助是这些文档成功的关键！[翻译相关的 bug 可以联系[UbuntuChina 文档组][8]成员。] 

   [7]: https://launchpad.net/products/ubuntu-doc/+bugs (https://launchpad.net/products/ubuntu-doc/+bugs)
   [8]: http://wiki.ubuntu.org.cn/UbuntuChina%E6%96%87%E6%A1%A3%E7%BB%84 (http://wiki.ubuntu.org.cn/UbuntuChina%E6%96%87%E6%A1%A3%E7%BB%84)

非常感谢 

- 您的 Ubuntu 文档小组 

### [[编辑][9]] 介绍

   [9]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=4 (编辑段落：介绍)

欢迎阅读 Ubuntu 服务器指南！ 

Ubuntu 服务器指南 包括了在您的 Ubuntu 系统中如何安装和配置满足您需要的不同服务器的相关信息。它是一个循序渐进、面向任务的配置和定制您系统的指南。本手册讨论的主题如下所示： 

  *     *       *         * 网络配置
        * Apache2 的配置
        * 数据库
        * Windows 联网

本手册主要分为以下几块： 

  *     *       *         * 安装
        * 包管理
        * 联网
        * Windows 联网

本指南假定您已经对您的 Ubuntu 系统有个基本的了解。如果您需要安装 Ubuntu 的详细帮助，将参考 Ubuntu 安装指南。 

本手册的 HTML 和 PDF 版本可以在 [Ubuntu 文档网站][10] 在线获得。 

   [10]: http://help.ubuntu.com/ (http://help.ubuntu.com/)

您可以在[our Lulu store][11]上购买到本指南的纸质品，只需支付打印和邮寄费用。 

   [11]: http://www.lulu.com/ubuntu-doc (http://www.lulu.com/ubuntu-doc)

### [[编辑][12]] 安装

   [12]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=5 (编辑段落：安装)

本章提供了安装 Ubuntu 6.06 LTS Server Edition（服务器版）的快速入门。更多细节说明，请参见 Ubuntu 安装指南。 

#### [[编辑][13]] 准备安装

   [13]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=6 (编辑段落：准备安装)

准备安装，本部分内容说明在开始安装之前要考虑的各个方面。 

##### [[编辑][14]] 系统要求

   [14]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=7 (编辑段落：系统要求)

Ubuntu 6.06 LTS Server Edition （服务器版）支持三种主要的体系架构： Intel x86、AMD64 和 PowerPC。下表列出了被推荐硬件明细表。您可以根据需要使用比这更少的（硬件）进行管理。然而，大多数用户不应当忽略这些建议，否则风险自负。 表 2-1 最小建议配置 

安装类型 
RAM 
硬盘空间 

服务器 
64 MB 
500 MB 

Ubuntu 6.06 LTS Server Edition （服务器版）的默认自述文档已经在下面列出了。当然，安装的尺寸大小极大程序上取决于您在安装过程中安装服务的多少。对于大多数管理员来说，默认的服务对于服务器一般的使用已经足够了。 

服务器 

这是一个小型服务器服务列表，它为各种服务器应用程序提供了一个通用基础。它是最低限度的并被设计成可以在其上添加想要的服务，如文件/打印服务、web 主机、邮件主机等。要满足这些服务至少需要 500 MB 的磁盘空间，但考虑添加更多的空间是要取决于在您服务器上您想要提供的服务。 

记住这些尺寸并不包括其他的素材如用户文件、邮件、日志和数据。当为您自己的文件和数据考虑空间时最好能留足。 

##### [[编辑][15]] 备份

   [15]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=8 (编辑段落：备份)

  *     *       *         * 在您开始之前，请确保备份了您现在系统上的每个文件。如果第一次时已经有一个操作系统安装在您的计算机上，那么最合适的办法就是把您的磁盘重新分区，为 Ubuntu 留出空间。无论哪次对您的磁盘进行分区您都应该做好丢失磁盘上所有东西的准备，因为您可能会误操作或者在分区过程中出错，如系统掉电等。在安装中所使用的程序是相当可靠的，大多数已经用了几年，但它们执行的也是破坏性的操作，一个操作出错可能会把您有价值的数据丢失掉。

如果您是想把电脑做成多重引导的系统，请先确定您手头上有电脑里已经存在的这些操作系统的安装介质。特别是当您把启动盘重新分区以后，您可能会发现必须重新安装原有操作系统的启动引导程序，某些情况下，还得重新安装该操作系统并恢复受影响分区上的文件。 

#### [[编辑][16]] 从 CD 安装

   [16]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=9 (编辑段落：从 CD 安装)

将您的安装 CD 插入 您的 CD-ROM 设备并重启计算机。当从 CD-ROM 重启时安装系统将立即开始。一旦初始化之后，您的第一个安装屏幕将出现。 

此时，阅读屏幕上的文字。您也许想看看安装程序提供的帮助屏。如果您想这么做的话，请按 F1 键。 

要执行缺省的服务器安装程序，选择 “安装到硬盘” 并按 回车 键。安装过程将开始。简单地根据屏幕上的指示，您的 Ubuntu 系统将被安装。 

或者，您要安装一个 LAMP 服务器 (Linux, Apache, MySQL, PHP/Perl/Python)，选择 “安装 LAMP 服务器”，并根据指示进行安装。施文 

### [[编辑][17]] 包管理

   [17]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=10 (编辑段落：包管理)

Ubuntu 提供一套全面的包管理系统用于软件的安装、升级、配置和卸载。除了让您 Ubuntu 计算机可以访问组织好的超过 17,000 个软件包的软件库之外，包管理工具还可以解决依赖关系并提供软件更新检查。 

一些工具可以和 Ubuntu 包管理系统进行交互，从便于系统管理员做自动化处理的简单命令行工具到便于 Ubuntu 新手使用的简单图形界面。 

#### [[编辑][18]] 介绍

   [18]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=11 (编辑段落：介绍)

Ubuntu　的包管理系统是从　Debian GNU/Linux 发行版中洐生出来的。包文件包括在您 Ubuntu 系统中实现特定功能或软件所必需的文件、元数据和指令。 

Debian 包文件一般用 '.deb' 作后缀，而且位于建立在不同介质上由包组成的 软件库 中，这些介质包括 CD-ROM 光盘和网站。包通常是预编译的二进制形式，因此安装速度快而且软件也无需编译。 

许多复杂的包使用 依赖包 这一概念，依赖包是主包为实现完整功能而要求的附加包。例如，语音合成包 Festival 依赖 festvox-kalpc16k 包，该依赖包提供被应用程序使用的众多声音之一。为了能使 Festival 正常运行，所有依赖包都必须与 Festival 主包同时安装。Ubuntu 软件管理工具将会自动完成这一切。 ff 

#### [[编辑][19]] Apt-Get

   [19]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=12 (编辑段落：Apt-Get)

apt-get 命令是一个强大的命令行工具，用于同 Ubuntu 的 Advanced Packaging Tool (APT) 一起执行诸如安装新软件包、升级已有软件包、更新包列表索引，甚至是升级整个 Ubuntu 系统等功能。 

作为一个简单的命令行工具，apt-get 对于服务器管理员来说比 Ubuntu 中的其他软件包管理工具有着相当多的优点。这些优点包括便于在简单终端连接 (SSH) 中使用，同时能够用于系统管理脚本中，以便能被cron 动作计划工具自动运行。 

apt-get 工具的一些常见用法示例： 

关于 APT 用法的更多信息，可阅读全面的[Debian APT 用户手册][20] 或输入： 

   [20]: http://www.debian.org/doc/user-manuals#apt-howto (http://www.debian.org/doc/user-manuals#apt-howto)

`apt-get help`

#### [[编辑][21]] Aptitude

   [21]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=13 (编辑段落：Aptitude)

Aptitude 是一个菜单驱动，基于文本的 Advanced Packaging Tool (APT) 系统前端。包管理的许多常用功能，如安装，卸载和升级，可以在Aptitude 中单键执行命令，它通常是小写字母。 

Aptitude 最适用于非图形的终端环境，确保命令关键字的适当功能。您可以作为一个普遍用户在终端提示符后用以下命令开始运行 Aptitude： 

`sudo aptitude`

当 Aptitude 开始之后，你将看在屏幕顶部的一个菜单条，其下有两个窗，顶窗包含包的类别，如 新软件包 和 未安装软件包 。底窗包含包和包类别的相关信息。 

使用 Aptitude 作包管理相对直观，用户界面便于执行常用任务。下面是在 Aptitude 中进行包管理时常见用法如下： 

  *     *       *         * 安装软件包：要安装包，通过未安装软件包包类别找到该软件包，如通过键盘箭头键和 ENTER 键定位并高亮你想安装的软件包。在高亮你要安装的软件包之后，将其标示为安装。现在按 g 键显示软件包的操作提示。再按 g 键，您将被提示要成为 root 用户以完成安装。按 ENTER 键将显示 Password: 提示。输入您的用户密码成为 root 用户。最后，再一次按 g 键，您将被提示下载软件包。在Continue 提示上按 ENTER 键，开始下载和安装软件包。
        * 卸载软件包：要卸载软件包，通过已安装软件包包类别找到该软件包，如通过键盘箭头键和 ENTER 键定位并高亮你想卸载的软件包。在高亮你要卸载的软件包之后，按 - 键，文件包条目将变成 pink，标示其为卸载。现在按 g 键显示软件包的操作提示。再按 g 键，您将被提示要成为 root 用户以完成卸载。按 ENTER 键将显示 Password: 提示。输入您的用户密码成为 root 用户。最后，再一次按 g 键，您将被提示下载软件包。在Continue 提示上按 ENTER 键，开始卸载软件包。
        * 更新软件包索引：要更新软件包索引，简单按 u 您将被提示要成为 root 用户以完成更新。按 ENTER 键将显示 Password: 提示，输入您的用户密码成为 root 用户。开始更新软件包索引，当出现下载对话框时在 OK 提示上按 ENTER 键以结束更新过程。
        * 升级软件包：要升级软件包，如上所述更新软件包索引，然后按U 键标示所有能升级的软件包。现在按 g 键显示软件包的操作提示。再按 g 键，您将被提示要成为 root 用户以完成安装。按 ENTER 键将显示 Password: 提示。输入您的用户密码成为 root 用户。最后，再一次按 g 键，您将被提示下载软件包。在Continue 提示上按 ENTER 键，开始升级软件包。

当实际查看软件时列出软件包当前状态，在顶窗软件包列表中显示信息的第一列使用下列关键字来描述软件包状态： 

  *     *       *         * i: 安装软件包
        * c: 软件包没有安装，但在系统中有软件包的残留配置
        * p: 从系统彻底删除
        * v: 虚拟软件包
        * B: 已损坏的软件包
        * u: 解压文件，但尚未配置软件包
        * C: 半配置 - 配置失败需要修复
        * H: 半安装 - 卸载失败需要修复

要退出 Aptitude，只需简单按 q 键并确认您想退出即可。在 Aptitude 菜单中按 F10 键可以列出其他许多功能。 

#### [[编辑][22]] 配置

   [22]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=14 (编辑段落：配置)

Advanced Packaging Tool (APT) 系统软件库的配置被保存在 /etc/apt/sources.list 文件中。这儿有个该文件的示例， 

[file:///usr/share/ubuntu-docs/ubuntu/serverguide/sample/sources.list 这里] 是一个典型的 /etc/apt/sources.list 文件范例。 

您可以编辑该文件来使软件库生效或失效。举个例子，要不想无论何时在发生文件包操作都会引起要求插入 Ubuntu CD-ROM ，只需要简单地将在文件顶部的 CD-ROM 相应行注释掉即可： 
    
    
    ==== 其他软件库 ====
    除了可以使用官方支持的 Ubuntu 软件包库之外，还存在拥有几千个潜在软件包的由其它社区维护的软件库。这些软件库中最流行的两个是 Universe 和 Multiverse 软件库。这些软件库并不被 Ubuntu 官方支持，这就是它们为什么在缺省时不能的原因，但它们提供的包通常是可以在您的 Ubuntu 计算机上安全使用的。
    
    在 Multiverse 软件库中的包通常有许可证的问题，这使得它们不能和自由操作系统一起分发，它们在您所在的地区可能是违法的。
    
    建议不要在 Universe 或 Multiverse 软件库中包含官方支持的软件包。尤其是在升级这些包时可能会不安全。      
    
    许多其他软件包源也是可用的，有时甚至只提供一个软件包，这种情况主要发生在由单个应用程序的开发人员所提供软件包源上。然而当您在使用非标准软件包源时您应该非常小心谨慎，在执行任何安装之前仔细考查源和软件包，因为有些软件包源和其中的软件包可能会使您的系统在某些方面运行不稳定或不正常。
    
    要使 Universe 和 Multiverse 库可用，编辑/etc/apt/sources.list 文件并将去掉相关行的注释：
    <pre><nowiki>
    
    deb http://archive.ubuntu.com/ubuntu dapper universe multiverse
    deb-src http://archive.ubuntu.com/ubuntu dapper universe multiverse
    

##### [[编辑][23]] 3.5.1. 引用

   [23]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=15 (编辑段落：3.5.1. 引用)

[如何添加软件库(Ubuntu Wiki)][24]

   [24]: https://wiki.ubuntu.com/AddingRepositoriesHowto (https://wiki.ubuntu.com/AddingRepositoriesHowto)

### [[编辑][25]] 联网

   [25]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=16 (编辑段落：联网)

网络是由两个或两个以上的设备通过物理线缆或无线连接而成并在连接设备之间共享和分发信息。这些设备包括计算机系统、打印机或用有线或无线连接起来的其它相关设备。 

Ubuntu 服务器指南的这部分提供与联网相关的一般和特定信息，包括网络概念的简介以及对常用网络协议及服务器应用程序的详细讨论。 

#### [[编辑][26]] 网络配置

   [26]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=17 (编辑段落：网络配置)

Ubuntu 提供了许多图形化工具来配制您的网络设备。本文适用于服务器管理员并聚焦在命令行中管理您的网络。 

##### [[编辑][27]] 4.1.1. 以太网

   [27]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=18 (编辑段落：4.1.1. 以太网)

大多数以太网配置都集中在单个文件 /etc/network/interfaces 中。如果您没有以太网设备，那么在该文件中将只出现环回口，该文件看上去类似这样： 
    
    
    auto lo
    iface lo inet loopback
    address 127.0.0.1
    netmask 255.0.0.0
    

如果您只有一个以太网设备 eth0，被配置成从 DHCP 服务器得到设置，并且在引导时自动激活，那么只需要再添加两行： 
    
    auto eth0↵
    iface eth0 inet dhcp
    

第一行说明 eth0 将会在您启动时自动激活。第二行说明该接口 (“iface”) eth0 将有得到一个 IPv4 地址空间 (如果是一个 IPv6 的设备将须将 “inet” 用 “inet6” 代替) 并且它将自动从 DHCP 中自动获得它的配置。假定您的网络和 DHCP 服务都已经被正确配置，该机的网络将不需要更多的配置。DHCP 服务器将提供默认网关 (通过 route 命令来实现) 、设备的 IP 地址 (通过 ifconfig 命令来实现)以及网络使用的 DNS 服务器 (在 /etc/resolv.conf 文件中实现)。 

要把您的以太网设备配置成静态 IP 地址和自定义配置的话，则要求更多的信息。假设您想指定 IP 地址 192.168.0.2 给设备 eth1，其掩码是 255.255.255.0。您的默认网关的 IP 地址是 192.168.0.1。您可以在 /etc/network/interfaces 中输入类似下面的语句： 
    
    iface eth1 inet static
            address 192.168.0.2
            netmask 255.255.255.0
            gateway 192.168.0.1
    

在这个例子中，您将需要在 /etc/resolv.conf 中手工指定您的DNS服务器，看起来如下： 
    
    search mydomain.com
    nameserver 192.168.0.1
    nameserver 4.2.2.2
    

search 语句在试图解析网络名时把 mydomain.com 添到主机名查询中。举个例子，如果您的网络域名是 mydomain.com 并且您试图去 ping 主机 “mybox”，DNS 查询将在解析时改为 “mybox.mydomain.com”。nameserver 语句指定用于将主机名解析成 IP 地址的的 DNS 服务器。如果您使用自己的名称服务器，在这里输入它。否则询问您的 Internet 服务供应商要使用的主、辅 DNS 服务器，并把它们如上所示输入到 /etc/resolv.conf 中。 

配置更多的接口是可能的，包括拨号的 PPP 接口、IPv6 网络、VPN 设备等。更多信息和支持选项请参考 man 5 interfaces。记住 ifup/ifdown 脚本使用的/etc/network/interfaces 是比其他一些 Linux 发行版更高级的配置模式。传统的低级工具如ifconfig、route 和 dhclient 也为了 ad hoc 配置对您来说也是可用的。 

##### [[编辑][28]] 4.1.2. 管理 DNS 记录

   [28]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=19 (编辑段落：4.1.2. 管理 DNS 记录)

本部分说明如何配置用来将IP地址解析成主机名或相反功能的名称服务，而不是说如何将整个系统配置成一个名称服务器。 

要管理 DNS 条目，您可以在 /etc/resolv.conf 文件中添加、编辑或删除 DNS 名称服务器。一个 范例文件 在下面给出： 
    
    search com
    nameserver 204.11.126.131
    nameserver 64.125.134.133
    nameserver 64.125.134.132
    nameserver 208.185.179.218
    

search 关键字指定为未完成主机名添加的字符串，在这里我们使用com。因此当我们运行：ping ubuntu 时它被理解成 ping ubuntu.com。 

nameserver 关键字指定名称服务器的 IP 地址，它将被用来解析 IP 地址或主机名。该文件可以有多个名称服务器记录。名称服务器将按相同顺序进行网络查询。 

如果 DNS 服务器名称是通过 DHCP 或 PPPOE 动态取回的(从您 ISP 取回)，那么不要在该文件中添加名称服务器记录。它将被自动更新。 

（补-Donnie）根据本人测试和查询man的结果，即便使用static地址也会导致/etc/resolv.conf被更新而丢失配置，需要在/etc/network/interfaces中的网卡后添加 dns-nameservers ip1 ip2 ip3 ，其中的ip指dns服务器IP地址，空格隔开 

##### [[编辑][29]] 4.1.3. 管理主机

   [29]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=20 (编辑段落：4.1.3. 管理主机)

要管理主机，您可以在 /etc/hosts 文件中添加、编辑或删除主机。该文件包括 IP 地址和相对应的主机名。当您的系统要解析一个主机到 IP 地址或从一个 IP 地址获取主机名时，它将在使用名称服务器之前参考 /etc/hosts 文件。如果该 IP 地址已经在 /etc/hosts 文件中被列出，那么将不再使用名称服务器。这一动作可以通过编辑 /etc/nsswitch.conf 来改变，不过后果自负。 

如果您网络所包含计算机的 IP 地址没有在 DNS 中列出，建议您将它们加入到 /etc/hosts 文件中。 

#### [[编辑][30]] TCP/IP

   [30]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=21 (编辑段落：TCP/IP)

传输控制协议和网际协议 (TCP/IP) 是在 20世纪70年代被美国国防部高级研究规划局 (DARPA)作为在不同类型计算机及计算机网络之间的通信手段而被开发的一个标准协议簇。TCP/IP 是 Internet 的驱动力，因此它是全球最流行的网络协议簇。 

##### [[编辑][31]] 4.2.1. TCP/IP 介绍

   [31]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=22 (编辑段落：4.2.1. TCP/IP 介绍)

TCP/IP 的两个协议组件处理计算机网络的不同方面。网际协议，TCP/IP 中的 "IP" 是一个连接协议，只处理使用 IP 数据报 作为网络信息基本单元的网络包路由。IP 数据报由报头和其后的消息组成。传输控制协议 是 TCP/IP 中的 "TCP"，可以使网络主机之间建立用于交换数据流的连接。TCP 也保证连接之间的数据传送以及其在网络主机上的接收顺序与其从另一台网络主机上的发送顺序一致。 

##### [[编辑][32]] 4.2.2. TCP/IP 配置

   [32]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=23 (编辑段落：4.2.2. TCP/IP 配置)

TCP/IP 协议配置由必须设置的几个元素组成，可以通过编辑相应的配置文件或配置方案如动态主机配置协议 (DHCP) 来设置，它可以配置成提供适当的 TCP/IP 配置来自动设置网络客户机。这些配置值必须正确设置，以便于您的 Ubuntu 系统进行相应网络操作。 

TCP/IP 常用配置元素及其作用如下所示： 

  *     *       *         * IP 地址 IP 地址是唯一标识字符串，它由四部分由点号分隔的，范围从 0 到 255 的十进制数组成。 每部分由8个比特表示，整个地址总长为32个比特。这种格式被称为 dotted quad notation。
        * 掩码 子网掩码 (或简称掩码) 是一个局部位掩码，或用指定的 子网掩码 来将IP 地址中的网络分隔出来的一组标识。举个例子，在 C 类网络中，标准的掩码是 255.255.255.0 屏蔽了 IP 地址的前三个字节，并允许 IP 地址的最后一个字节指定子网中的主机。
        * 网络地址 网络地址表示包括IP 地址网络部分的字节。 例如， 一个 A 类网络的主机 12.128.1.2 将使用 12.0.0.0 作为网络地址，使用 12 来表示 IP 地址的第一个字节 (网络部分)， 余下的三个为 0 的字节表示可能的主机值的。网络主机使用象 192.168.1.100 这样非常普遍的不可路由的私有 IP 地址将使用 192.168.1.0 网络地址，用前三个字节来指定 C 类 192.168.1 网络，而用一个 0 来表示网络上所有可能的主机。
        * 广播地址 广播地址是一个允许向给定子网中的所有主机而不是一台特定的网络主机同时发送网络数据的 IP 地址。一般标准 IP 网络的地址是 255.255.255.255，但这个广播地址不能用来为 Internet 网上的每台主机发送一个广播消息，因为路由器会阻止它。更适当的广播地址设置是匹配特定子网的。例如，在流行的私有 C 类 IP 网 192.168.1.0 中，广播地址应该设为 192.168.1.255。广播消息一般都是由网络协议产生的，如地址识别协议 (ARP) 和路由信息协议 (RIP)。
        * 网关地址 网关地址是一个通过该地址可能会到达指定网络或网络主机的 IP 地址。如果一台网络主机希望与另一台网络主机通讯，而该机并不在同一网络中，而是要传输到另一个网络或主机上，如 Internet 主机。网关地址设置必须正确，否则您的系统将不能到达不在同一网络中的任何主机。
        * 名称服务器地址 名称服务器地址表示域名服务 (DNS) 系统的 IP 地址。该系统将网络主机名解析成 IP 地址。可以按顺序来指定三个不同优先级的名称服务器地址：主 名称服务器，次 名称服务器，和 第三 名称服务器。按顺序为您系统将网络主机名解析成相应的 IP 地址，你必须指定合法的名称服务器地址，该地址应该在您系统的 TCP/IP 配置中被授权使用。在许多情况下这些地址可以也应该被您的网络服务供应商提供，但也可以使用许多免费的、可供公众访问的名称服务器，如 IP 从 4.2.2.1 到 4.2.2.6 的 Level3 (Verizon) 服务器。

IP 地址、掩码、网络地址、广播地址以及网关地址一般都是在文件 /etc/network/interfaces 中通过相应的语句来指定的。名称服务器地址一般是在文件 /etc/resolv.conf 中通过 nameserver 语句来指定的。更多详情，请分别查阅 interfaces 或 resolv.conf 的系统手册页。 

查阅 interfaces 系统手册页，可用以下命令： 

`man interfaces`

查阅 resolv.conf 系统手册页，用以下命令： 

`man resolv.conf`

##### [[编辑][33]] 4.2.3. IP 路由

   [33]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=24 (编辑段落：4.2.3. IP 路由)

IP 路由是在 TCP/IP 网络上为可能发送的网络数据指明或发现路径。路由使用一组路由表来指示网络数据包从源地址转发到目的地，经常是通过许多叫做路由器的网络节点做中转。IP 路由是 Internet 上路径发现的主要方式。IP 路由分为两种形式：静态路由 和 动态路由。 

静态路由包含向系统路由表中手工添加的 IP 路由，一般是通过 route 命令来向路由表手工添加的。静态路由与动态路由相比有许多优点，如在小网络中实施简单，有可预测性 (路由表总是事先算好，因此路由在每次使用时都相当一致)，在其它路由器和网络链路处理上比动态路由协议开销小。然而，静态路由也有一些缺点。如静态路由只限于小网络而且不能很好地进行调整。静态路由由于路由固定的特性，因此根本无法根据路由来适应网络中断和故障。 

动态路由有赖于从一个源到目的有多条可用 IP 路由的大型网络，利用特定的路由协议，如路由信息协议 (RIP)，可以自动调整路由表以生成可能的动态路由。动态路由相对静态路由有几个优点,如拥有较大的伸缩性和能根据网络路由来适应网络中断和故障。另外，几乎无须手工配置路由表，因为路由器可以相互学到其他已有并且可用的路由器。这一特性也消除了由于人为错误而在路由表中引入错误的可能。然而，动态路由也并不完美，其表现出来的缺点如相当复杂以及由于路由器通讯所带来的额外的网络开销，并不能使最终用户由此获益，并却一直消耗着网络带宽。 

##### [[编辑][34]] 4.2.4. TCP 和 UDP

   [34]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=25 (编辑段落：4.2.4. TCP 和 UDP)

TCP 是一个基于连接的协议，提供纠错并通过 流量控制 来传输数据。流量控制决定什么时间一个数据流需要停止，例如在出现诸如 冲突 等问题时重发先前发送的数据包，以确保完整和准确的数据传输。TCP 常用于重要信息的交换，如数据库传输。 

另一方面，用户数据报协议 (UDP) 是一个 无连接 协议，很少用于重要数据的传输，因为缺乏流量控制或其他一些确保可靠数据传输的方法。UDP 常用在如音视频流这样的应用程序，由于它缺少纠错和流控，因此相对于 TCP 来说更快，而且丢失少量包通常也不会造成灾难性的后果。 

##### [[编辑][35]] 4.2.5.ICMP

   [35]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=26 (编辑段落：4.2.5.ICMP)

Internet 控制消息协议是在Request For Comments (RFC) #792 中定义的，是对网际协议 (IP) 的一个扩充。支持的网络包包括控制、错误和信息的消息。ICMP 常被用在诸如判断一台网络主机或设备可用性的 ping 工具这样的网络应用程序。在网络主机和设备如路由器之间使用 ICMP 所返回的错误消息示例包括 Destination Unreachable 和 Time Exceeded。 

##### [[编辑][36]] 4.2.6. 守护程序

   [36]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=27 (编辑段落：4.2.6. 守护程序)

守护程序是特殊的系统应用程序，一般常驻在后台并等待来自其他应用程序请求其所提供的功能。许多守护程序都是网络中心；在 Ubuntu 系统后台执行的许多守护程序都可以提供网络的相关功能。这些网络守护程序包括 超文本传输协议守护程序 (httpd)，用于提供网站服务器功能；Secure SHell 守护程序 (sshd)，用于提供安全远程登录 shell 和文件传输功能；Internet Message Access Protocol 守护程序 (imapd)，用于提供 E-Mail 服务。 

#### [[编辑][37]] OpenSSH 服务器

   [37]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=28 (编辑段落：OpenSSH 服务器)

##### [[编辑][38]] 4.4.1. 介绍

   [38]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=29 (编辑段落：4.4.1. 介绍)

Ubuntu 服务器指南的这部分内容介绍一个强大的远程控制网络计算机和在它们之间传输数据的工具集 OpenSSH。您也可以学到一些 OpenSSH 服务器应用程序的配置以及如何在您 Ubuntu 系统修改它们。 

OpenSSH 是Secure Shell (SSH) 协议工具集中的一个自由可用的版本，用以远程控制一台计算机或在计算机之间传输文件。完成这些功能的传统工具，如 telnet 或 rcp 等，是不安全的，它们在使用时用明文来传输用户的密码。OpenSSH 提供一个服务器守护程序和客户端工具来保障安全、加密的远程控制和文件传输操作，以有效地取代传统的工具。 

OpenSSH 服务器组组件 sshd 持续监听来自任何客户端工具的连接请求。当一个连接请求发生时，sshd 根据客户端连接的类型来设置当前连接。例如，如果远程计算机是通过 ssh 客户端应用程序来连接的话，OpenSSH 服务器将在认证之后设置一个远程控制会话。如果一个远程用户通过 scp 来连接 OpenSSH 服务器，OpenSSH 服务器将在认证之后开始服务器和客户机之间的安全文件拷贝。OpenSSH 可以支持多种认证模式，包括纯密码、公钥以及Kerberos 票据。 

##### [[编辑][39]] 4.4.2. 安装

   [39]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=30 (编辑段落：4.4.2. 安装)

OpenSSH 客户端及服务器应用程序的安装是简单的。要在您 Ubuntu 系统中安装 OpenSSH 客户端应用程序，可以在终端提示符后使用以下命令： 
    
    sudo apt-get install openssh-client
    

要安装 OpenSSH 服务器应用程序及相关的支持文件，可以在终端提示符后使用以下命令： 
    
    sudo apt-get install openssh-server
    

##### [[编辑][40]] 4.4.3. 配置

   [40]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=31 (编辑段落：4.4.3. 配置)

您可以通过编辑 /etc/ssh/sshd_config 文件来配置 OpenSSH 服务器应用程序的缺省过程。关于该文件中使用的配置语句信息，您可以在终端提示符后运行下列命令来查阅相应的手册页： 
    
    man sshd_config
    

  
在 sshd 配置文件中有许多语句来控制那些诸如通信设置和认证模式。下面是一个通过编辑 /etc/ssh/ssh_config 文件来改变配置语句的例子。 

在编辑配置文件之前，您应该生成一个原始文件的拷贝并对其写保护，以便您可以参考原始文件并在必要时重用它。 

拷贝 /etc/ssh/sshd_config 文件并对其写保护可以通过在终端提示符后运行下列命令： 
    
    sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.original
    sudo chmod a-w /etc/ssh/sshd_config.original
    

  
以下是您可能更改配置语句的范例: 

  *     *       *         * 要设置您 OpenSSH 在 TCP 2222 端口而不是缺省的 TCP 20 端口监听，可以如下使用改变 Port 语句：

`Port 2222`

  *     *       *         * 要让 sshd 允许基于公钥登录证书，可以简单添加或修改该行语句：

`PubkeyAuthentication yes` 到 /etc/ssh/sshd_config 文件中。如果已经存在，确保该行语句没有被注释。 

  *     *       *         * 要使您的 OpenSSH 服务器显示 /etc/issue.net 文件的内容以作为预登录 Banner，只需简单地将下行添加或修改：

`Banner /etc/issue.net` 到 /etc/ssh/sshd_config 文件中即可。 

在修改 /etc/ssh/sshd_config 文件之后，保存该文件并重启 sshd 服务器应用程序以使之生效。可以在终端提示符后使用下列命令： 
    
    sudo /etc/init.d/ssh restart
    

许多其他的 sshd 配置语句可以使服务器应用程序按您的要求运行。然而，给您一个忠告，如果您访问服务器的唯一方法就是使用 ssh，而且您在通过 /etc/ssh/sshd_config 文件来配置 sshd 时犯了一个错误，那么在重启该服务之后您可能会发现您被锁在服务器外面了，或者是 sshd 服务在处理一个不正确的配置语句时拒绝启用。因此当在远程服务器上编辑该文件时要格外的小心。 

##### [[编辑][41]] 4.4.4. 引用

   [41]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=32 (编辑段落：4.4.4. 引用)

[OpenSSH 网站][42]

   [42]: http://www.openssh.org/ (http://www.openssh.org/)

[高级 OpenSSH 维基页][43]

   [43]: https://wiki.ubuntu.com/AdvancedOpenSSH (https://wiki.ubuntu.com/AdvancedOpenSSH)

#### [[编辑][44]] FTP 服务器

   [44]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=33 (编辑段落：FTP 服务器)

文件传输协议 (FTP) 是一个 TCP 协议，用于在计算机之间上传和下载文件。FTP 工作在客户端/服务器模式下。服务器组件被称为 FTP 守护程序。它持续不断地监听来自远程客户端的 FTP 请求。当一个请求到达时，它管理登录和建立连接。在整个会话期间它执行 FTP 客户端发送来的任何命令。 

可以通过两种方式来管理 FTP 服务器的访问： 

  *     *       *         * 匿名
        * 授权

在匿名模式中，远程客户端可以使用 "anonymous" 或 "ftp" 缺省用户帐号并通过发送一个邮件地址做为密码来访问 FTP 服务器。在授权模式下一个用户必须拥有帐号和密码。用户所访问 FTP 服务器中目录和文件的权限是根据登录时所用帐号来定义的。一般来说，FTP 守护程序将隐藏在 FTP 服务器的根目录中并将其改到 FTP 家目录。这样就可以向远程传话隐藏文件系统的其他部分。 

##### [[编辑][45]] 4.5.1. vsftpd - FTP 服务器安装

   [45]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=34 (编辑段落：4.5.1. vsftpd - FTP 服务器安装)

vsftpd 是可在 Ubuntu 中使用的 FTP 守护程序之一。它在安装、设置和维护方面十分方便。要安装 vsftpd 您可以使用下列命令： 
    
    sudo apt-get install vsftpd 
    

##### [[编辑][46]] 4.5.2. vsftpd - FTP 服务器配置

   [46]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=35 (编辑段落：4.5.2. vsftpd - FTP 服务器配置)

你可以编辑 vsftpd 配置文件，/etc/vsftpd.conf，来配置缺省设置。缺省状态下只允许匿名 FTP。如果您希望禁用该选项，您可以将下面这行： 
    
    anonymous_enable=YES
    

改为 
    
    anonymous_enable=NO
    

  * 缺省状态下，本地系统用户是不允许登录 FTP 服务器的。要改变该设置，您可以将下面这行反注释：
  * 缺省状态下，允许用户从 FTP 下载文件，但不允许他们上传文件到 FTP 服务器。为了能够上传文件到 FTP 服务器，需要改变该设置，您可以将下面这行反注释掉：
  * 配置文件包括许多配置参数。关于配置文件中的每个参数的信息都可以得到，或者您可以参考手册页，man 5 vsftpd.conf 说明每个参数的细节。

一旦您配置好了 vsftpd 您就可以运行该守护程序了。您可以执行下列命令来运行vsftpd 守护进程： 
    
    sudo /etc/init.d/vsftpd start 
    

请注意在配置文件中缺省的设置主要是出于安全考虑。上面每一个改变都会使系统的安全性更小，所以请只在您需要时才改变他们。 

#### [[编辑][47]] 网络文件系统 (NFS)

   [47]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=36 (编辑段落：网络文件系统 (NFS))

NFS 允许系统将其目录和文件共享给网络上的其他系统。通过 NFS，用户和应用程序可以访问远程系统上的文件，就象它们是本地文件一样。 

NFS 最值得注意的优点有： 

  *     *       *         * 本地工作站可以使用更少的磁盘空间，因为常用数据可以被保存在一台机器上，并让网络上的其他机器可以访问它。
        * 不需要为用户在每台网络机器上放一个用户目录。用户目录可以在 NFS 服务器上设置并使其在整个网络上可用。
        * 存储设备如软盘、光驱及 USB 设备可以被网络上其它机器使用。这可能可以减少网络上移动设备的数量。

##### [[编辑][48]] 4.6.1. 安装

   [48]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=37 (编辑段落：4.6.1. 安装)

在终端提示符后键入以下命令安装 NFS 服务器： 
    
    sudo apt-get install nfs-kernel-server
    

##### [[编辑][49]] 4.6.2. 配置

   [49]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=38 (编辑段落：4.6.2. 配置)

您可以配置要输出的目录，您可以在 /etc/exports 文件中添加该目录。例如： 
    
    /ubuntu *(ro,sync,no_root_squash)
    /home *(rw,sync,no_root_squash)
    

您可以用主机名来代替 *。尽量指定主机名以便使那些不想其访问的系统访问 NFS 挂载的资源。 

您可以在终端提示符后运行以下命令来启动 NFS 服务器： 
    
    sudo /etc/init.d/nfs-kernel-server start
    

##### [[编辑][50]] 4.6.3. NFS 客户端配置

   [50]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=39 (编辑段落：4.6.3. NFS 客户端配置)

使用 mount 命令来挂载其他机器共享的 NFS 目录。可以在终端提示符后输入以下类似的命令： 
    
    sudo mount example.hostname.com:/ubuntu /local/ubuntu
    

挂载点 /local/ubuntu 目录必须已经存在。而且在 /local/ubuntu 目录中没有文件或子目录。 

另一个挂载其他机器的 NFS 共享的方式就是在 /etc/fstab 文件中添加一行。该行必须指明 NFS 服务器的主机名、服务器输出的目录名以及挂载 NFS 共享的本机目录。 

以下是在 /etc/fstab 中的常用语法： 
    
    example.hostname.com:/ubuntu /local/ubuntu nfs rsize=8192,wsize=8192,timeo=14,intr
    

##### [[编辑][51]] 4.6.4. 引用

   [51]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=40 (编辑段落：4.6.4. 引用)

[Linux NFS 常见问答][52]

   [52]: http://nfs.sourceforge.net/ (http://nfs.sourceforge.net/)

#### [[编辑][53]] 动态主机配置协议 (DHCP)

   [53]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=41 (编辑段落：动态主机配置协议 (DHCP))

动态主机配置协议 (DHCP) 是一种网络服务，相对于手工为每台网络主机配置，它使网络主机可能自动被服务器指定设置。被配置成 DHCP 客户端的计算机并不能控制其从 DHCP 服务器得到的设置，且该配置对于计算机用户来说是透明的。 

由 DHCP 服务器提供给 DHCP 客户端最常用的设置包括： 

  *     *       *         * IP 地址和掩码
        * DNS
        * WINS

然而，一个 DHCP 服务器也支持配置如下属性，如： 

  *     *       *         * 主机名
        * 域名
        * 默认网关
        * 时间服务器
        * 打印服务器

使用 DHCP 的好处在于当网络发生改变如 DNS 服务器地址改变时，只需要在 DHCP 服务器中改变即可，所有网络主机将在其 DHCP 客户端下一次轮询 DHCP 服务器时被重新配置。另一个好处就是，它在将新计算机整合到网络时也更容易，因为不需要再检查 IP 地址的有效性。同时也减少 IP 地址的冲突。 

一个 DHCP 服务器可以用两个模式来提供配置设置 

  * MAC 地址

该模式需要用 DHCP 去标明连接到网上的每块网卡唯一的硬件地址，然后在 DHCP 客户端每次使用该网络设备发送给 DHCP 服务器请求时提供给它一个固定的配置。 

  * 地址池

该模式需要定义一个 IP 地址池 (有时也叫范围或作用域) ，以便 DHCP 客户端可以被动态提供它们的配置from which DHCP clients are supplied their configuration properties dynamically and on a fist come first serve basis。当一个 DHCP 客户端有段时间不再在网络上时，该配置将过期并释放回地址池以便为其他 DHCP 客户端使用。 

ubuntu 提供 DHCP 服务器及其客户端。服务器叫 dhcpd (动态主机配置协议守护程序)。Ubuntu 提供的客户端叫 dhclient，应该安装在所有自动配置的计算机上。这两个程序很容易安装和配置，并可在系统引导时自动启用。 

##### [[编辑][54]] 4.7.1. 安装

   [54]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=42 (编辑段落：4.7.1. 安装)

要安装 dhcpd，可以在终端提示符后输入以下命令: 
    
    sudo apt-get install dhcpd
    

您将看到下面的输出，说明接下来做什么： 
    
    Please note that if you are installing the DHCP server for the first
    time you need to configure. Please stop (/etc/init.d/dhcp
    stop) the DHCP server daemon, edit /etc/dhcpd.conf to suit your needs
    and particular configuration, and restart the DHCP server daemon
    (/etc/init.d/dhcp start).
    
    You also need to edit /etc/default/dhcp to specify the interfaces dhcpd
    should listen to. By default it listens to eth0.
    
    NOTE: dhcpd's messages are being sent to syslog. Look there for
    diagnostics messages.
    
    Starting DHCP server: dhcpd failed to start - check syslog for diagnostics. 
    

##### [[编辑][55]] 4.7.2. 配置

   [55]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=43 (编辑段落：4.7.2. 配置)

安装结束后的错误消息可能会带来小小的困惑，不过下面几步将帮助您配置服务： 通常，您想做的是随机指定一个 IP 地址。这可以通过以下设置来实现： 
    
    default-lease-time 600;
    max-lease-time 7200;
    option subnet-mask 255.255.255.0;
    option broadcast-address 192.168.1.255;
    option routers 192.168.1.254;
    option domain-name-servers 192.168.1.1, 192.168.1.2;
    option domain-name "mydomain.org";
    
    subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.10 192.168.1.100;
    range 192.168.1.150 192.168.1.200;
    } 
    
    

这将导致 DHCP 服务器从 192.168.1.10-192.168.1.100 或 192.168.1.150-192.168.1.200 范围中分配客户端一个 IP 地址。如果客户端没有要求一个特定的时间帧的话它将租用 600秒的 IP 地址。否则最大 (允许) 租用时间为 7200 秒。服务器也 "建议" 客户端使用 255.255.255.0 做为它的子网掩码，192.168.1.255 作为它的广播地址，192.168.1.254 作为路由器/网关，同时将 192.168.1.1 和 192.168.1.2 作为它的 DNS 服务器。 

如果您需要为您的 Windows 客户机指定一个 WINS 服务器，您需要包含 netbios-name-servers 选项，如： 
    
    option netbios-name-servers 192.168.1.1; 
    

Dhcpd 配置设置可以从 DHCP 快速指南中得到，该指南可以在 这里 找到。 

##### [[编辑][56]] 4.7.3. 引用

   [56]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=44 (编辑段落：4.7.3. 引用)

[DHCP 常见问答][57]

   [57]: http://www.dhcp-handbook.com/dhcp_faq.html (http://www.dhcp-handbook.com/dhcp_faq.html)

#### [[编辑][58]] 域名解析服务 (DNS)

   [58]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=45 (编辑段落：域名解析服务 (DNS))

域名解析服务 (DNS) 是一个 Internet 服务，相互映射 IP 地址和完全限定域名 (FQDN) 。通过这种方式，使用 DNS 将不再需要记住 IP 地址。运行 DNS 的计算机称为 名称服务器。Ubuntu 提供 BIND (伯克利 Internet 名称守护程序)，一个在 GNU/Linux 上最常用的维护名称服务器的程序。 

##### [[编辑][59]] 4.8.1. 安装

   [59]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=46 (编辑段落：4.8.1. 安装)

在终端提示符后输入以下命令来安装 dns： 
    
    sudo apt-get install bind9
    

##### [[编辑][60]] 4.8.2. 配置

   [60]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=47 (编辑段落：4.8.2. 配置)

DNS 配置文件被保存在 /etc/bind 目录中。主配置文件叫 /etc/bind/named.conf。缺省配置文件的内容如下所示： 
    
    // This is the primary configuration file for the BIND DNS server named.
    //
    // Please read /usr/share/doc/bind/README.Debian for information on the 
    // structure of BIND configuration files in Debian for BIND versions 8.2.1 
    // and later, *BEFORE* you customize this configuration file.
    //
    
    include "/etc/bind/named.conf.options";
    
    // reduce log verbosity on issues outside our control
    logging {
            category lame-servers { null; };
            category cname { null; };
    };
    
    // prime the server with knowledge of the root servers
    zone "." {
    type hint;
    file "/etc/bind/db.root";
    };
    
    // be authoritative for the localhost forward and reverse zones, and for
    // broadcast zones as per RFC 1912
    
    zone "localhost" {
    type master;
    file "/etc/bind/db.local";
    };
    
    zone "127.in-addr.arpa" {
    type master;
    file "/etc/bind/db.127";
    };
    
    zone "0.in-addr.arpa" {
    type master;
    file "/etc/bind/db.0";
    };
    
    zone "255.in-addr.arpa" {
    type master;
    file "/etc/bind/db.255";
    };
    
    // add local zone definitions here
    include "/etc/bind/named.conf.local";
    

include 行指定包含 DNS 选项的文件名。在选项文件中的directory 行告诉 DNS 在哪儿寻找文件。所有 BIND 用到的文件都与该目录相关。 名为 /etc/bind/db.root 的文件描述世界上的根名称服务器。这些服务器按时更新并不时被维护。 

zone 部分定义一个主服务器，并将其保存在 file 标签所指定的文件中。每个 zone 文件包括 3 个资源记录 (RRs)：一个 SOA RR、一个 NS RR 以及一个 PTR RR。SOA 是授权开始的缩写。"@" 符是一个特定的符号表示原点。NS 是名称服务器 RR。PTR 是域名指针。要开始 DNS 服务器，可以在终端提示符后运行以下命令： 
    
    sudo /etc/init.d/bind start
    

详情您可以参考在参考部分所提及的文档。 

##### [[编辑][61]] 4.8.3. 引用

   [61]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=48 (编辑段落：4.8.3. 引用)

[DNS 指南][62]

   [62]: http://www.tldp.org/HOWTO/DNS-HOWTO.html (http://www.tldp.org/HOWTO/DNS-HOWTO.html)

#### [[编辑][63]] CUPS － 打印服务器

   [63]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=49 (编辑段落：CUPS － 打印服务器)

Ubuntu 打印及打印服务主要是 通用 UNIX 打印服务 (CUPS)。该打印系统是自由可用、可移植的打印层，正在成为绝大多数 GNU/Linux 发行版新的打印标准。 

CUPS 管理打印作业和队列，并使用标准的 Internet 打印协议 (IPP) 提供网络打印，该协议提供最大范围的打印机支持，从点阵打印机到激光打印机以及位于两者之间的许多打印机。CUPS 也支持 PostScript Printer Description (PPD) 和网络打印机的自动检测，以及提供基于 Web 的简单配置和管理工具。 

##### [[编辑][64]] 4.9.1. 安装

   [64]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=50 (编辑段落：4.9.1. 安装)

在您 Ubuntu 计算机上安装 CUPS，只需简单使用 sudo 和apt-get 命令并将要安装包作为第一个参数即可。一个完全的 CUPS 安装需要安装许多从属包，但它们也可以在同一个命令行指定。在终端提示符后输入以下命令以安装 CUPS： 
    
    sudo apt-get install cupsys cupsys-client
    

认证您的用户密码之后，这些包将被下载并正确安装。在安装结束之后，CUPS 服务器将自动开始。为了发现并修复问题，您可以通过错误日志文件 /var/log/cups/error_log 来查看 CUPS 服务器的错误。如果错误日志并没有显示足够的用于找到和解决您所遇问题的信息，通过将配置文件 (下面要讨论) 中将LogLevel 语句从缺省的 "info" 改为 "debug" 甚至是记录每件事的 "debug2"，以获得 CUPS 日志的详细信息。 

##### [[编辑][65]] 4.9.2. 配置

   [65]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=51 (编辑段落：4.9.2. 配置)

可以通过 /etc/cups/cupsd.conf 文件中的指令来配置通用 UNIX 打印系统服务器的行为的。CUPS 配置文件与 Apache HTTP 服务器的主配置文件语法相同，因此熟悉编辑 Apache 配置文件的用户在编辑 CUPS 配置文件时会感到相当容易。在这里将显示一些您可能想要改变初始值的设置范例。 

在编辑配置文件之前，您应该将原始文件做个副本并将其写保护，以便您可以将原始文件作为参考并在必要时重用它。 

拷贝 /etc/cups/cupsd.conf 文件并对其写保护，可以在终端提示符后执行以下命令： 
    
    sudo cp /etc/cups/cupsd.conf /etc/cups/cupsd.conf.original
    sudo chmod a-w /etc/cups/cupsd.conf.original
    

  *     *       *         * ServerAdmin：要配置指定 CUPS 服务器管理员的邮件地位，只需用你喜欢的文本编辑器简单编辑 /etc/cups/cupsd.conf 配置文件，并修改相应的 ServerAdmin 行即可。举个例子，如果您是 CUPS 服务器的管理员，并且您的邮件地址是'bjoy@somebigco.com'，那么您可以象下面这样修改 ServerAdmin 行：

`ServerAdmin bjoy@somebigco.com`

关于 CUPS 服务器配置文件中配置语句的更多范例，通过在终端提示符后输入以下命令可以查阅相关的系统手册页： 
    
    man cupsd.conf
    

无论您在什么时间修改了 /etc/cups/cupsd.conf 配置文件，您都需要重启 CUPS 服务，在终端提示符后键入以下命令： 
    
    sudo /etc/init.d/cupsys restart
    

CUPS 服务器的其它一些配置在文件 /etc/cups/cups.d/ports.conf 中： 

  *     *       *         * Listen：在 Ubuntu 的缺省状态下，CUPS 服务器安装后只监听 IP 地址为 127.0.0.1 的环回接口。为了让 CUPS 服务器可以在网络适配器真正的 IP 地址上监听，您必须要么指定一个指定主机名、要么指定一个IP 地址，随您选择。可以通过 Listen 语句来添加一个 IP 地址/端口对。例如：如果您的 CUPS 服务器在一个局域网中，其 IP 地址为 192.168.10.250，您想要该子网中的其它系统能够访问它，您可以编辑 /etc/cups/cups.d/ports.conf 并如下所示添加一个 Listen 语句：
    
    Listen 127.0.0.1:631 # existing loopback Listen
    Listen /var/run/cups/cups.sock # existing socket Listen
    Listen 192.168.10.250:631 # Listen on the LAN interface, Port 631 (IPP)
    

在上面的例子里，如果您不想 cupsd 监听环回地址 (127.0.0.1) ，您可能注释或删除了相关语句。但最好保留它以监听局域网 (LAN) 的以太网接口。为了能监听一个特定主机名所绑定的所有的网络接口，您可以为 socrates 主机名创建一个 Listen 条目，如下所示： 
    
    Listen socrates:631 # Listen on all interfaces for the hostname 'socrates'
    

或者忽略 Listen 语句并使用 Port 来代替，如： 
    
    Port 631 # Listen on port 631 on all interfaces
    

##### [[编辑][66]] 4.9.3. 引用

   [66]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=52 (编辑段落：4.9.3. 引用)

[CUPS 网络][67]

   [67]: http://www.cups.org/ (http://www.cups.org/)

#### [[编辑][68]] HTTPD - Apache2 Web 服务器

   [68]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=53 (编辑段落：HTTPD - Apache2 Web 服务器)

Apache 是在 GNU/Linux 系统中最常用的 Web 服务器。Web 服务器为客户机所提交的网页请求服务。客户机一般通过网页浏览器如 Firefox、Opera、Mozilla或IE来请求和查看网页。 

用户输入统一资源定位器 (URL) 来指向一个 Web 服务器，并通过完全限定域名 (FQDN) 和路径来请求资源。例如，要查看 Ubuntu 网站 的主页，用户只需要输入 FQDN 即可。如果要请求关于 有偿技术支持 的特定信息，用户将在 FQDN 后输入路径。 

用于传输网页的最常用协议就是超文本传输协议 (HTTP)。也支持诸如基于安全套接层的超文本传输协议 (HTTPS) 以及用于上传和下载文件的文件传输协议 (FTP) 等协议。 

Apache Web 服务器常与 MySQL 数据库引擎、超文本处理器 (PHP) 脚本语言及其他流行的脚本语言如Python 和 Perl 组合在一起。这一组合被称为 LAMP (Linux, Apache, MySQL and Perl/Python/PHP) ，并形成一个强大健壮的开发基于 Web 应用程序的开发平台。 

##### [[编辑][69]] 4.10.1. 安装

   [69]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=54 (编辑段落：4.10.1. 安装)

Apache2 web 服务器在 Ubuntu Linux 中是可用的。要安装 Apache2： 

  *     *       *         * 在终端提示符后输入下列命令：
    
    sudo apt-get install apache2
    

##### [[编辑][70]] 4.10.2. 配置

   [70]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=55 (编辑段落：4.10.2. 配置)

Apache 可以在纯文本配置文件中通过 语句 来配置的。主配置文件叫 apache2.conf。此外，其外配置文件可以用 Include 语句来添加，也可以使用通配符来包含多个配置文件。任何语句都可能被放在这些配置文件的任何一个文件中。修改过的主配置文件只在 Apach2 启动或重启时才能被其识别。 

服务器也查看包含 mime 文档类型的文件；该文件名通过 TypesConfig 设置，缺省情况下是mime.types。 

缺省的 Apache2 配置文件是 /etc/apache2/apache2.conf。您可以编辑该文件以配置 Apache2 服务器。您可以配置端口号、文档根目录、模块、日志文件及虚拟主机等。 

###### [[编辑][71]] 4.10.2.1. 基本设置

   [71]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=56 (编辑段落：4.10.2.1. 基本设置)

本部分内容说明 Apache2 服务器基本的配置参数。详情请参阅 Apache2 文档 

  *     *       *         * Apache2 提供了一个友好虚拟主机的缺省配置。它配置成单个缺省虚拟主机 (使用 VirtualHost 语句) 。如果您有单个站点，可以修改或直接使用它。如果您有多个站点的话，可以将其作为其它虚拟主机的模板。如果对其不加理会，该缺省虚拟主机将会作为您的缺省网站提供服务，或者如果网站用户所输入的 URL 并没有匹配您任何所定义站点的 ServerName 语句时，将看到该虚拟主机内容。要修改缺省虚拟主机，可以编辑文件 /etc/apache2/sites-available/default。如果您希望配置一个新的虚拟主机或站点，在同一目录中将拷贝该文件并将新文件重命名为您所想要的文件名，如sudo cp /etc/apache2/sites-available/default /etc/apache2/sites-available/mynewsite。编辑新文件并用下面的所描述的语句来配置新的站点：
        * ServerAdmin 语句指定服务器管理员的邮件地址，缺省值是 webmaster@localhost。应该改成您的邮件地址 (如果您是服务器管理员的话)。如果您的网站有问题，Apache2 将显示包含该邮件地址的错误信息以便报告该问题。在 /etc/apache2/sites-available 目录中您网站的配置文件里可以找到该语句。
        * Listen 语句指定端口以及可选的 IP 地址，Apache2 将在其上监听。如果 IP 地址没有被指定，Apache2 将监听所有指向其所运行机器上的 IP 地址。Listen 语句的缺省值是 80。把其改成 127.0.0.1:80 将使 Apache2 只在您的环回接口上临听以致于它相对于 Internet 不可用。也可改变其监听端口如81，或保持原样以便正常操作。该语句可以在它自己的文件 /etc/apache2/ports.conf 中发现并修改。
        * The ServerName 语句是可选的，它指明您站点要应答什么 FQDN。缺省虚拟主机没有指定 ServName，因为它要应答没有匹配其它虚拟主机 ServerName 语句的所有请求。如果您只获得 ubunturocks.com 域名并希望在您的 Ubuntu 服务器上只作它的主机，那么在您虚拟主机配置文件中的 ServerName 语句值应该是 ubunturocks.com。在您先前创建的新虚拟主机文件 (/etc/apache2/sites-available/mynewsite) 中添加该语句。

您也可能要您的网站响应 www.ubunturocks.com 的请求，因为许多用户会假定 www 前缀是适当的。可以使用 ServerAlias 语句来解决这个问题。您也可以在 ServerAlias 中使用通配符。例如，ServerAlias *.ubunturocks.com 将使您的网站响应任何域名以 .ubunturocks.com 结尾的请求。 

  *     *       *         * DocumentRoot 语句指定 Apache 将到哪儿去寻找站点文件，缺省值为 /var/www。没有站点配置在那里，但如果您反注释在 /etc/apache2/apache2.conf 中的 RedirectMatch 语句，请求将被重定向到 /var/www/apache2-default，那里是缺省的 Apache2 站点。在您站点的虚拟主机文件中改变该值，记住在必要时可以创建相应的目录！

/etc/apache2/sites-available 目录并 不会 被 Apache2 解析。在 /etc/apache2/sites-enabled 的软链接指向 "可用的" 站点。使用 a2ensite (Apache2 启用站点) 工具可以创建这些软链接，如：sudo a2ensite mynewsite 这里您站点的配置文件是 /etc/apache2/sites-available/mynewsite。同样，a2dissite 工具将用来禁用站点。 

###### [[编辑][72]] 4.10.2.2. 缺省设置

   [72]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=57 (编辑段落：4.10.2.2. 缺省设置)

这部分内容说明 Apache2 服务器缺省设置的配置。举个例子，如果您添加一个虚拟主机，您为该虚拟主机配置的设置将优先于缺省虚拟主机。如果在该虚拟主机的设置中有个语句没有定义，那么将使用缺省值。 

  *     *       *         * 当用户在目录名后使用斜杠 (/) 来请求一个目录索引时，The DirectoryIndex 将通过服务器提供缺省页服务。

例如，当一个用户请求 [http://www.example.com/this_directory/][73] 页时，他或她要么得到 DirectoryIndex 页，如果它存在的话；要么得到一个服务器生成的目录列表，如果它不存在且设置了 Indexes 选项的话；要么就得到一个无权访问页，如果它不存在且没有设置 Indexes 选项的话。服务器尝试找到在 DirectoryIndex 语句中所列文件之一，并返回它所找到的第一个。如果它没有找到任何一个文件且该目录设置了 Indexes 选项，服务器将生成并返回一个 HTML 格式的列表，包括该目录中的子目录和文件。缺省值可以在 /etc/apache2/apache2.conf 文件中找到，是 " index.html index.cgi index.pl index.php index.xhtml"。因此，如果 Apache2 在所请求的目录中找到任何一个匹配这些名字的文件，第一个将被显示。 

   [73]: http://www.example.com/this_directory/ (http://www.example.com/this_directory/)

  *     *       *         * ErrorDocument 语句允许您为 Apache 指定一个用于特定错误事件的文件。例如，如果用户请求的资源不存在，那么将引发 404 错误，而在 Apache2 的缺省配置中，文件 /usr/share/apache2/error/HTTP_NOT_FOUND.html.var 将被显示。该文件并不在服务器的 DocumentRoot 中，但在 /etc/apache2/apache2.conf 中有个别名语句将到 /error 目录的请求重定向到 /usr/share/apache2/error/ 中。要查看缺省的 ErrorDocument 语句列表，可以使用命令：grep ErrorDocument /etc/apache2/apache2.conf
  *     *       *         * 在缺省状态下，服务器将传输日志记录在文件 /var/log/apache2/access.log 中。您可以在您每个虚拟主机站点的配置文件上用 CustomLog 语句来改变它，或者忽略它以接受在 /etc/apache2/apache2.conf 中指定的缺省设置。您也可以通过 ErrorLog 语句来指定记录错误日志的文件，该文件缺省是 /var/log/apache2/error.log。这些都从传输日志中分离出来以便更好地在您的 Apache2 服务器中发现和解决问题。您也可以指定 LogLevel (缺省值是 "warn") 和 LogFormat (缺省值参见 /etc/apache2/apache2.conf)
  *     *       *         * 一些选项是基于每目录而非每服务器的。Option 是这些语句中的一个。Directory 段是被放在类 XML 标记中，如：
    
    <Directory /var/www/mynewsite>
                                    ...
                            </Directory>
    

在 Directory 段中的 Options 语句接受一个或更多下面的用空格分隔的值 (包括其它)： 

  * ExecCGI - 允许执行 CGI 脚本。如果该选项没有设置，则 CGI 脚本将不能执行。

大多数文件不会作为 CGI 脚本运行的。这是非常危险的。CGI 脚本应该位于您 DocumentRoot 目录以外与之分开的目录中，并且只能该目录才有 ExecCGI 选项设置。缺省状态下，CGI 脚本默认位于 /usr/lib/cgi-bin。 

  * Includes - 允许服务器端包含。服务器端包含允许一个 HTML 文件包含其他文件。这不是一个常用选项。更多信息参见 the Apache2 SSI 指南。
  * IncludesNOEXEC - 允许服务器端包含，但 CGI 脚本中的 #exec 和 #include 指令无效。
  * Indexes - 如果 DirectoryIndex (如 index.html) 在请求的目录没存在的话，按一定方式显示目录内容列表。
  * Multiview - 支持内容协商的多重视图；该选项在缺省状态下出于安全的考虑是被禁用的。参见 Apache2 关于该选项的文档。
  * SymLinksIfOwnerMatch - 仅在软连接与其目的文件或目录拥有相同所有者时才使用。

###### [[编辑][74]] 4.10.2.3. 虚拟主机设置

   [74]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=58 (编辑段落：4.10.2.3. 虚拟主机设置)

虚拟主机允许您在同一台机器上相对不同的 IP 地址、主机名或不同端口号运行不同的服务器。例如，您可以运行使用虚拟主机在同一个 Web 服务器上运行网站 [http://www.example.com][75] 和 [http://www.anotherexample.com。这一选项适用于缺省虚拟主机和基于][76] IP 的虚拟主机的 <VirtualHost> 语句，也适用于基于名称的虚拟主机的 <NameVirtualHost> 语句。 

   [75]: http://www.example.com (http://www.example.com)
   [76]: http://www.anotherexample.com%E3%80%82%E8%BF%99%E4%B8%80%E9%80%89%E9%A1%B9%E9%80%82%E7%94%A8%E4%BA%8E%E7%BC%BA%E7%9C%81%E8%99%9A%E6%8B%9F%E4%B8%BB%E6%9C%BA%E5%92%8C%E5%9F%BA%E4%BA%8E (http://www.anotherexample.com。这一选项适用于缺省虚拟主机和基于)

虚拟主机的语句设置仅应用于特定的虚拟主机。如果一个语句在服务器范围中设置而没有在虚拟主机设置中定义，那么将使用缺省设置。例如，您可以定义网络管理员的邮件地址而无需为每个虚拟主机都分别定义邮件地址。 

为虚拟主机设置 DocumentRoot 语句到包含根文档 (如 index.html) 目录。缺省的 DocumentRoot 是 /var/www。 

在 VirtualHost 段中的 ServerAadmin 语句是指用于错误页的页脚中的邮件地址，如果您想在错误页的页脚显示邮件地址的话。 

###### [[编辑][77]] 4.10.2.4. 服务器设置

   [77]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=59 (编辑段落：4.10.2.4. 服务器设置)

这部分内容说明如何配置基本的服务器设置。 

LockFile - 当服务器编译时使用了 USE_FCNTL_SERIALIZED_ACCEPT 或 USE_FLOCK_SERIALIZED_ACCEPT 参数时，使用 LockFile 语句来设置 lockfile 的路径。它必须保存在本地磁盘上，它应该设置成缺省值，除非日志目录被定位在 NFS 共享上。如果是这种情况，缺省值应该被改为本地磁盘的位置并且其目录只对 root 用户可读。 

PidFile - PidFile 语句设置服务器记录其进程 ID (pid) 的文件。该文件只对 root 用户可读。在大多数情况下，应该保留其缺省值。 

User - 用户语句设置被服务器用于回应请求的用户 ID。该设置决定服务器的权限。一些该用户无法访问的文件也无法被您网站的访问者访问。用户缺省值是 www-data。 

除非您的确知道您在做什么，否则请不要将 User 设为 root 用户。用 root 作为 User 的值将会在您的 Web 服务器中产生极大的安全漏洞。 

Group 语句同 User 语句相似。Group 设置被服务器用于回应请求的用户组。缺省的组也是 www-data。 

###### [[编辑][78]] 4.10.2.5. Apache 模块

   [78]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=60 (编辑段落：4.10.2.5. Apache 模块)

Apache 是一个模块化的服务器。这就意味着在核心服务器中只包括最基本的功能。扩展功能可以通过被引导进 Apache 的模块来实现。缺省情况下，基本模块是在编译时被包含进服务器的。如果服务器编译成可以动态引导模块，那么模块可以单独编译，并在任何时候使用 LoadModule 语句来添加。否则，Apache 必须在添加或删除模块时重新编译。Ubuntu 编译 Apache2 时是允许动态引导模块的。配置语句通过将已有模块放置 <IfModule> 块中以便有条件地包含在配置语句中。您可以在您的 Web 服务器上安装和使用额外的 Apache2 模块。您可以用 apt-get 命令来安装 Apache2 模块。如安装 MYSQL 认证的 Apach2 模块，您可以在终端提示符中运行以下命令： 
    
    sudo apt-get install libapache2-mod-auth-mysql
    

一旦您安装了模块，模块将出现在 /etc/apache2/mods-available 目录中。您可以使用 a2enmod 命令来启用模块。您也可以使用 a2dismod 命令来禁用模块。一旦您启用该模块，该模块将在 /etc/apache2/mods-enabled 目录中出现。 

##### [[编辑][79]] 4.10.3. HTTPS 配置

   [79]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=61 (编辑段落：4.10.3. HTTPS 配置)

The mod_ssl 模块为 Apache2 服务器添加了一个重要的功能 - 通讯加密的能力。因此，当您的浏览器要用 SSL 加密通讯时，需要在浏览器导航栏中在输入的统一资源定位器 (URL) 的开始处使用 https:// 前缀。 

mod_ssl 模块已经包含在 apache2-common 软件包中。如果您安装该软件包，您可以在终端提示符之后执行下列命令启用 mod_ssl 模块： 
    
    sudo a2enmod ssl
    

###### [[编辑][80]] 4.10.3.1. 证书和安全

   [80]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=62 (编辑段落：4.10.3.1. 证书和安全)

要设置您的安全服务器，使用公共钥创建一对公钥私钥对。大多数情况下，您发送您的证书请求 (包括您的公钥)，您公司证明材料以及费用到一个证书颁发机构 (CA)。CA 检证证书请求及您的身份，然后将证书发回您的安全服务器。 

还有种办法就是您可以创建您自己签署的证书。然而请注意自己签置的证书不应该用于生产环境。自已签署的证书不会被用户浏览器自动接受。浏览器将提示用户接受证书并创建一个安全的连接。 

一旦您有一个自己签署的证书或一个您选择的 CA 签署的证书，您需要将它安装在您的安全服务器上。 

###### [[编辑][81]] 4.10.3.2. 证书类型

   [81]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=63 (编辑段落：4.10.3.2. 证书类型)

您需要一个钥匙和一个证书来操作您的安全服务器，这意味着您可以生成自己签署的证书或购买 CA 签署的证书。由 CA 签署的证书为您的服务器提供两个重要的功能： 

  *     *       *         * 浏览器 (通常) 会自动地识别证书并且在不提示用户的情况下允许创建一个安全连接。
        * 当一个 CA 生成一个签署过的证书，它为提供网页给浏览器的组织提供身份担保。

多数支持 SSL 的 Web 浏览器都有一个 CA 列表，它们的证书会被自动接受。如果一个浏览器遇到一个其授权 CA 并不在列表中的证书，浏览器将询问用户是否接受或拒绝连接。 

您可以为您的安全服务器生成一个自己签署的证书，但要知道自己签署的证书并不提供与 CA 签署的证书相同的功能。自己签署的证书不会被多数 Web 浏览器自动识别，而且自己签署的证书也不为任何提供网站的组织提供担保。CA 签署的证书为安全服务器提供所有这些重要的功能。从 CA 得到证书的过程相当容易。下面简要介绍一下： 

  *     *       * 创建一个私有和公共密钥对
      * 基于公钥创建一个证书请求。证书请求包含您服务器及公司信息。
      * 发送证书请求，并随之提供您的身份文档到一个 CA。我们不能告诉您选择哪个证书颁发机构。您可以基于您以往的经验或您朋友或同事的经验或纯粹基于经济因素来决定。

一旦您选定一家 CA，您需要根据他们所提供的规程来从他们那里获得证书。 

  *     *       * 当 CA 确定您确实如您所声称的那样时，他们将发给您一个数字证书。
      * 在您的安全服务器上安装该证书，并开始进行安全事务处理

无论您是从一家 CA 那儿获得证书或是生成您自己签署的证书，第一步就是生成钥匙。 

###### [[编辑][82]] 4.10.3.3. 生成一个证书签署请求 (CSR)

   [82]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=64 (编辑段落：4.10.3.3. 生成一个证书签署请求 (CSR))

要生成证书签署请求 (CSR)，您应该创建您自己的钥匙。您可以在终端提示符后运行以下命令以创建钥匙： 
    
    openssl genrsa -des3 -out server.key 1024
    
    
              
    Generating RSA private key, 1024 bit long modulus
    .....................++++++
    .................++++++
    unable to write 'random state'
    e is 65537 (0x10001)
    Enter pass phrase for server.key:
    

您现在可以输入您的 passphrase。为了最大程度的安全，它至少应该包含八个字符。当指定 -des3 时最小长度为四个字符。它应该包含数字和/或标点符号，并且不应该是字典中的单词。也请记住您的 passphrase 是大小写敏感的。 

再次输入 passphrase 核对。一旦您再次输入正确的话，服务器密钥就生成了并保存在 server.key 文件中。 

您也可以不用 passphrase 来运行您的安全 Web 服务器。这样比较方便，因为在您启动您的安全 Web 服务器时您不需要每次都输入 passphrase。但它也是相当不安全的，钥匙的风险也同样意味着服务器的风险。 

在任何情况下，您都可以选择不用 passphrase 来运行您的安全 Web 服务器，这可以通过在生成时不带 -des3 参数来实现或者通过在终端提示符后执行以下命令： 
    
    openssl rsa -in server.key -out server.key.insecure
    

一旦您运行上述命令，不安全的钥匙将被保存在 server.key.insecure 文件中。您可以使用该文件来生成没有 passphrase 的 CSR。 

要创建 CSR，可以在终端提示符后运行以下命令： 
    
    openssl req -new -key server.key -out server.csr
    

它将提示您输入 passphrase。如果您输入正确的 passphrase，它将提示您输入公司名、站点名、邮件ID等。一旦您输入了所有内容，您的 CSR 将被创建并被保存在 server.csr 文件中。您可以提交该 CSR 文件给一家 CA 去处理。CA 将使用该 CSR 并颁发证书。但是，您也可以使用该 CSR 生成自己签署的证书。 

###### [[编辑][83]] 4.10.3.4. 创建一个自己签署的证书

   [83]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=65 (编辑段落：4.10.3.4. 创建一个自己签署的证书)

要创建自己签署的证书，在终端提示符下运行以下命令： 
    
    openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
    

上述命令将提示您输入 passphrase。一旦您输入正确的 passphrase，您的证书将被创建并将保存在 server.crt 文件中。 

如果您的安全服务器被用在生产环境中，你也许需要 CA 签署的证书。并不推荐使用自己签署的证书。 

###### [[编辑][84]] 4.10.3.5. 安装证书

   [84]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=66 (编辑段落：4.10.3.5. 安装证书)

您可以安装钥匙文件 server.key 和证书文件 server.crt 或由您的 CA 颁发的证书文件，在终端提示符后运行以下命令： 
    
    sudo cp server.crt /etc/ssl/certs
    sudo cp server.key /etc/ssl/private
    

您要添加以下四行到 /etc/apache2/sites-available/default 文件或您安全虚拟主机的配置文件。您要将它们放在 VirtualHost 部分。他们应该放在 DocumentRoot 行下面： 
    
    SSLEngine on
    
    SSLOptions +FakeBasicAuth +ExportCertData +CompatEnvVars +StrictRequire
    
    SSLCertificateFile /etc/ssl/certs/server.crt
    SSLCertificateKeyFile /etc/ssl/private/server.key
    

HTTPS 在 443 端口监听。您可以将下面一行添加到 /etc/apache2/ports.conf 文件中： 
    
    Listen 443
    

###### [[编辑][85]] 4.10.3.6. 访问服务器

   [85]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=67 (编辑段落：4.10.3.6. 访问服务器)

一旦您安装了您的证书，您应该重启您的 Web 服务器。您可以在终端提示符后运行以下命令以重启您的 web 服务器： 
    
                
    sudo /etc/init.d/apache2 restart
    

您应该记住并在每次您启动您的安全 web 服务器时输入 passphrase。 

您将被提示输入 passphrase。一旦您输入正常的 passphrase，将启动安全 web 服务器。您可以通过在您的浏览器地址栏上输入 [https://your_hostname/url/][86] 来访问安全服务器的页面。 

   [86]: https://your_hostname/url/ (https://your_hostname/url/)

##### [[编辑][87]] 4.10.4. 引用

   [87]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=68 (编辑段落：4.10.4. 引用)

[Apache2 文档][88]

   [88]: http://httpd.apache.org/docs/2.0/ (http://httpd.apache.org/docs/2.0/)

[Mod SSL 文档][89]

   [89]: http://www.modssl.org/docs/ (http://www.modssl.org/docs/)

#### [[编辑][90]] Squid - 代理服务器

   [90]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=69 (编辑段落：Squid - 代理服务器)

Squid 是一个全功能的 web 代理与缓存服务器应用程序，它为超文本传输协议 (HTTP)、文件传输协议 (FTP) 以及其他流行网络协议提供代理和缓存服务。Squid 可以实现安全套接层 (SSL) 请求的缓存和代理、域名服务器 (DNS) 的缓存以及进行传输缓存。Squid 也支持大量不同的缓存协议，如 Internet 缓存协议 (ICP)、超文本缓存协议 (HTCP)、缓存阵列路由协议 (CARP) 以及 Web 缓存协同协议 (WCCP)。 

Squid 代理缓存服务器对于不同的代理和缓存服务器需求来说是一个极好的解决方案，它适用于从分支机构到企业级的网络，访问控制机制的粒度以及通过简单网络管理协议 (SNMP) 对临界参数的监视。当选择计算机系统用于 Squid 代理或缓存服务器时，请确保您的系统配置大量的物理内存以便 Squid 可以使用内存进行缓存以提高性能。 

##### [[编辑][91]] 4.11.1. 安装

   [91]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=70 (编辑段落：4.11.1. 安装)

在终端提示符后输入下列命令安装 Squid 服务器： 
    
    sudo apt-get install squid squid-common
    

##### [[编辑][92]] 4.11.2. 配置

   [92]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=71 (编辑段落：4.11.2. 配置)

Squid 可以通过编辑在 /etc/squid/squid.conf 配置文件中的语句来进行配置。下面的范例说明一些语句的修改可能对 Squid 服务器的影响。更多 Squid 的配置可以参阅参考章节。 

在编辑配置文件之前，您应该生成一份原始文件副本并对其进行写保护，以便您可以将原始文件作为参考并在必要时重用它。 

要拷贝 /etc/squid/squid.conf 文件并对其进行写保护，可以在终端提示符后使用以下命令： 
    
    sudo cp /etc/squid/squid.conf /etc/squid/squid.conf.original
    sudo chmod a-w /etc/squid/squid.conf.original
    

  *     *       *         * 要将您的 Squid 服务器监听 TCP 端口 8888 以代替缺省的 TCP 端口 3128，可以如下所示修改 http_port 语句：

`http_port 8888`

  *     *       *         * 改变 visible_hostname 语句是为了给 Squid 服务器一个特定的主机名。该主机名并必是计算机的主机名。在本范例中它被设为 weezie。

`visible_hostname weezie`

  *     *       *         * 此外，使用 Squid 访问控制，您可以通过 Squid 代理将 Internet 服务配置成仅限于有确定网际协议 (IP) 地址的用户使用。例如，我们将举例说明只让 192.168.42.0/24 子网的用户访问：

将下列语句添加到您 /etc/squid/squid.conf 文件 ACL 部分的 底部： `acl fortytwo_network src 192.168.42.0/24` 然后添加下列语句到你 /etc/squid/squid.conf 文件 http_access 部分的 顶部： `http_access allow fortytwo_network`

  *     *       *         * 使用 Squid 卓越的访问控制功能，您可以通过 Squid 代理将 Internet 服务配置成仅限于在正常商务时间使用。例如，我们将举例说明只允许来自 10.1.42.0/24 子网的商务雇员在周一到周五的上午 9:00 到 下午 5:00 时间段内访问：

将下列语句添加到您 /etc/squid/squid.conf 文件 ACL 部分的 底部： `acl biz_network src 10.1.42.0/24 acl biz_hours time M T W T F 9:00-17:00` 然后添加下列语句到你 /etc/squid/squid.conf 文件 http_access 部分的 顶部： `http_access allow biz_network biz_hours`

  
在修改 /etc/squid/squid.conf 文件后，保存该文件并重启 squid 服务器应用程序以使改动生效。可以在终端提示符后使用下列命令： 
    
    sudo /etc/init.d/squid restart
    

##### [[编辑][93]] 4.11.3. 引用

   [93]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=72 (编辑段落：4.11.3. 引用)

[Squid 网站][94]

   [94]: http://www.squid-cache.org/ (http://www.squid-cache.org/)

#### [[编辑][95]] 版本控制系统

   [95]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=73 (编辑段落：版本控制系统)

版本控制是管理改动信息的技术。它对于程序员而言一直是重要的工具，他们经常花时间对程序进行小改动之后又在第二天改回来。但版本控制软件的用途却远远超出了软件开发的界线。无论何处您可以发现人们使用计算机去管理那些经常变动的信息，那里都有使用版本控制的空间。 

##### [[编辑][96]] 4.12.1. Subversion

   [96]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=74 (编辑段落：4.12.1. Subversion)

Subversion 是一个开源的版本控制系统。使用 Subversion，您可以记录源文件和文档的历史。它管理文件和目录。文件树被放入了中心库中。库更象是一个普通的文件服务器，除了它可以记住对文件和目录的每次改变。 

###### [[编辑][97]] 4.12.1.1. 安装

   [97]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=75 (编辑段落：4.12.1.1. 安装)

要通过 HTTP 协议来访问 Subversion 库，您必须安装和配置一个 web 服务器。Apache2 被证明可以和 Subversion 一起工作。请参考 Apache2 章节的 HTTP 小节以安装和配置 Apache2。要使用 HTTPS 协议访问 Subversion 库，您必须在您的 Apache2 web 服务器上安装和配置数字证书。请参考 Apache2 章节的 HTTPS 小节以安装和配置数据证书。 

要安装 Subversion，可以在终端提示符后运行以下命令： 
    
    sudo apt-get install subversion libapache2-svn
    

###### [[编辑][98]] 4.12.1.2. 服务器配置

   [98]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=76 (编辑段落：4.12.1.2. 服务器配置)

这一步假定您已经在您的系统上安装了上面提及的包。本部分内容说明如何创建一个 Subversion 库和访问项目。 创建 Subversion 库 

Subversion 库可以在终端提示符后使用以下命令创建： 
    
    svnadmin create /path/to/repos/project
    

###### [[编辑][99]] 4.12.1.3. 访问方式

   [99]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=77 (编辑段落：4.12.1.3. 访问方式)

Subversion 库可以通过许多不同的方式如通过在本地磁盘或不同的网络协议来访问 (checked out)。然而，库的位置经常是一个 URL。下表描述了不同的URL模式如何映射相应的访问方式。 表 4-1 访问方式 

模式 
访问方式 

file:// 
直接访问库 (在本地磁盘) 

http:// 
通过 WebDAV 协议访问带有 Subversion 的 Apache2 web 服务器。 

https:// 
与 http:// 相同，但有 SSL 加密 

svn:// 
通过自身协议访问 svnserve 服务 

svn+ssh:// 
与 svn:// 一样，但使用 SSH 遂道 

在本部分，我们将看到如何为所有这些访问方式来配置 Subversion。这里，我们只介绍基本用法。更多详细、高级用法请参阅svn 书 

###### [[编辑][100]] = 4.12.1.3.1. 直接访问库 (file://) =

   [100]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=78 (编辑段落：= 4.12.1.3.1. 直接访问库 (file://) =)

这是所有访问方式中最简单的。它不要求运行任何 Subversion 服务器进程。该访问方式用于在同一台机器上访问 Subversion。在终端提示符后输入的命令如下所示： 
    
    svn co file:///path/to/repos/project
    

或 
    
    svn co file://localhost/path/to/repos/project
    

如果您没有指定主机名，则需要三个斜杠 (///) -- 其中两个是协议的 (这里是 file)，另一个是路径前的。如果您指定了主机名，那么您必须使用双个斜杠 (//)。 

库权限依赖于文件系统的权限。如果用户有读/写权限，他可以从库中检出或者提交到库。 

###### [[编辑][101]] = 4.12.1.3.2. 通过 WebDAV 协议 ([http://][102]) 访问 =

   [101]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=79 (编辑段落：= 4.12.1.3.2. 通过 WebDAV 协议 (http://) 访问 =)
   [102]: http:// (http://)

要通过 WebDAV 协议访问 Subversion，您必须配置您的 Apache2 web 服务器。您必须在您的 /etc/apache2/apache2.conf 文件中添加下面一小段： 
    
    <Location /svn>
    DAV svn
    SVNPath /path/to/repos
    AuthType Basic
    AuthName "Your repository name"
    AuthUserFile /etc/subversion/passwd
    <LimitExcept GET PROPFIND OPTIONS REPORT>
    Require valid-user
    </LimitExcept>
    </Location> 
    

接下来，您必须创建 /etc/subversion/passwd 文件。该文件包含用户认证细节。要添加一个条目，如添加一个用户，您可以在终端提示符后运行下列命令： 
    
    htpasswd2 /etc/subversion/passwd user_name
    

该命令将提示您输入密码。一旦您输入密码。该用户将被添加。现在您可以运行下列命令来访问库： 
    
    svn co http://servername/svn
    

该密码是以纯文本传输的。如果您担心密码被截取，建议您使用 SSL 加密。相关细节，请参考下一章节。 

###### [[编辑][103]] = 4.12.1.3.3. 通过带有 SSL 加密的 WebDAV 协议来访问 ([https://][104]) =

   [103]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=80 (编辑段落：= 4.12.1.3.3. 通过带有 SSL 加密的 WebDAV 协议来访问 (https://) =)
   [104]: https:// (https://)

通过带 SSL 加密的 WebDAV 协议 ([https://][105]) 访问也使用 http:// 类似，只是您必须在您的 Apache2 web 服务器中安装和配置数字证书。 

   [105]: https:// (https://)

您可以安装由证书签署机构如 Verisign 颁发的数字证书。或者，您也可以安装自己签署的证书。 

这一步假设您已经在您的 Apache2 web 服务器中安装和配置了数字证书。现在要访问 Subversion 库，请参考上一章节！除了所用协议之外访问方式完全相同。您必须使用 https:// 来访问 Subversion 库。 

###### [[编辑][106]] = 4.12.1.3.4. 通过自身协议访问 (svn://) =

   [106]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=81 (编辑段落：= 4.12.1.3.4. 通过自身协议访问 (svn://) =)

一旦 Subversion 库被创建，您就可以配置访问控制了。您可以通过编辑 /path/to/repos/project/conf/svnserve.conf 文件来配置访问控制了。例如，要设置认证，您可以在配置文件中反注释下列行： 
    
    
    在反注释上面几行之后，您可以在 passwd 文件中维护用户列表。因此编辑同一目录中的文件 passwd 并添加新用户。其语法如下：
    <pre><nowiki>
    username = password
    

更多细节，请参考该文件。 

现在要从本机或不同机器通过 svn:// 自身协议来访问 Subversion，您可以使用 svnserve 命令来运行 svnserver。其语法如下： 
    
    $ svnserve -d --foreground -r /path/to/repos
    
    For more usage details, please refer to:
    $ svnserve --help
    

一旦您运行该命令，将启动 Subversion 并在缺省端口 (3690) 监听。要访问项目库，您必须在终端提示符后运行下列命令： 
    
    svn co svn://hostname/project project --username user_name
    

根据服务器的配置，出现密码提示。一旦您认证通过，将从 Subversion 库检出代码。要让本地副本同步项目库，您可以运行 update 子命令。在终端提示符后的命令语法如下所示： 
    
    cd project_dir ; svn update
    

关于 Subversion 子命令的更多细节，您可以参考手册。如为了学到关于 co (checkout) 命令的细节，请在终端提示符后运行下列命令： 
    
    svn co help
    

###### [[编辑][107]] = 4.12.1.3.5. 通过带有 SSL 加密的自身协议 (svn+ssh://) 访问 =

   [107]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=82 (编辑段落：= 4.12.1.3.5. 通过带有 SSL 加密的自身协议 (svn+ssh://) 访问 =)

配置和服务器处理与用 svn:// 方式是相同的。详情请参考上面的章节。这一步假定您已经完成了上面的步骤并用 svnserve 命令启动了 Subversion 服务器。 

它也假定 ssh 服务器已经在该机上运行并允许连接。为了确认，请尝试使用 ssh 登录该机器。如果您可以登录，一切就绪。如果不能登录，请在继续之前解决它。 

svn+ssh:// 协议使用 SSL 加密来访问 Subversion 库。使用这种方式进行的数据传输是加密的。要访问项目库 (如 checkout)，您必须使用下面的命令语法： 
    
    svn co svn+ssh://hostname/var/svn/repos/project
    

使用这种访问方式您必须使用全路径 (/path/to/repos/project) 来访问 Subversion 库。 

根据服务器配置，它将提示输入密码。在使用 ssh 登录时您必须输入密码。一旦您被认证通过之后，它将从 Subversion 库中检出代码。 

##### [[编辑][108]] 4.12.2. CVS 服务器

   [108]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=83 (编辑段落：4.12.2. CVS 服务器)

CVS 是一个版本控制系统。您可以使用它来记录源文件的历史。 

###### [[编辑][109]] 4.12.2.1. 安装

   [109]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=84 (编辑段落：4.12.2.1. 安装)

在终端提示符后输入下列命令来安装 cvs： 
    
    sudo apt-get install cvs
    

在您安装 cvs之后，您将安装 xinetd 来启动和停用 cvs 服务器。在提示符后输入下列命令以安装 xinetd： 
    
    sudo apt-get install xinetd
    

###### [[编辑][110]] 4.12.2.2. 配置

   [110]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=85 (编辑段落：4.12.2.2. 配置)

一旦您安装 cvs，将会自动初始化库。缺省状态下，库存放在 /var/lib/cvs 目录下。您可以通过运行以下命令来改变该路径： 
    
    cvs -d /your/new/cvs/repo init
    

一旦库开始建立，您可以配置 xinetd 来启动 CVS 服务器。您可以拷贝以下行到 /etc/xinetd/cvspserver 文件。 
    
    service cvspserver
    {
    port = 2401
    socket_type = stream
    protocol = tcp
    user = root
    wait = no
    type = UNLISTED
    server = /usr/bin/cvs
    server_args = -f --allow-root /var/lib/cvs pserver
    disable = no
    }
    
    

如果你改变缺省的库目录 (/var/lib/cvs) 那么您必须要编辑库。 

一旦您配置好 xinetd ，您就可以运行以下命令来启动 cvs 服务器了： 
    
    sudo /etc/init.d/xinetd start
    

您可以执行以下命令来确定 CVS 服务器正在运行： 
    
    sudo netstat -tap | grep cvs
    

当您运行该命令时，您可以看到类似下面的行： 
    
    tcp 0 0 *:cvspserver *:* LISTEN 
    

在这里您可以继续添加用户，添加新的项目以及管理 CVS 服务器 

CVS 允许用户添加独立于 OS 安装的用户。也许最容易的方式就是让 CVS 使用 Linux 的用户，虽然它有潜在的安全隐患。详细请参考 CVS 手册。 

###### [[编辑][111]] 4.12.2.3. 添加项目

   [111]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=86 (编辑段落：4.12.2.3. 添加项目)

本部分内容说明如何在 CVS 库中添加新项目。创建目录以及该目录所需的文档和源文件。现在运行下列命令将该项目添加到 CVS 库中： 
    
    cd your/project
    cvs import -d :pserver:username@hostname.com:/var/lib/cvs -m "Importing my project to CVS repository" . new_project start
    
    

您可以使用 CVSROOT 环境变量来保存 CVS 根目录。一旦您导出 CVSROOT 环境变量，您可以在上面的 cvs 命令中避免使用 －d 选项。 

字符串 new_project 是一个 vendor 标签，start 是一个版本标签。它们在此没有任何用处，但由于 CVS 要求要有它们，所以它们必须出现。 

当您新添项目时，您所用的 CVS 用户必须对 CVS 库 (/var/lib/cvs) 有写权限。缺省状态下，src 组有对 CVS 库的写权限，因此，您可以添加用户到该组，然后就该用户就可以在 CVS 库中添加和管理项目了。 

##### [[编辑][112]] 4.12.3. 引用

   [112]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=87 (编辑段落：4.12.3. 引用)

[Subversion 主页][113]

   [113]: http://subversion.tigris.org/ (http://subversion.tigris.org/)

[Subversion 书 (使用Subversion进行版本控制)][114]

   [114]: http://svnbook.red-bean.com/ (http://svnbook.red-bean.com/)

[CVS 手册][115]

   [115]: http://ximbiot.com/cvs/manual/cvs-1.11.21/cvs_toc.html (http://ximbiot.com/cvs/manual/cvs-1.11.21/cvs_toc.html)

#### [[编辑][116]] 数据库

   [116]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=88 (编辑段落：数据库)

Ubuntu 提供两个数据库服务器。它们是： 

  *     *       *         * MySQL™
        * PostgreSQL

它们位于主软件库中。该部分内容说明如何安装和配置这些数据库服务器。 

##### [[编辑][117]] 4.13.1. MySQL

   [117]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=89 (编辑段落：4.13.1. MySQL)

MySQL 是一个快速、多线程、多用户、强大的 SQL 数据库服务器。它旨在成为能用于大型应用、高负载的生产系统以及大规模部署的软件。 

###### [[编辑][118]] 4.13.1.1. 安装

   [118]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=90 (编辑段落：4.13.1.1. 安装)

要安装 MySQL，可以在终端提示符后运行下列命令： 
    
    sudo apt-get install mysql-server mysql-client
    

一旦安装完成，MySQL 服务器应该自动启动。您可以在终端提示符后运行以下命令来检查 MySQL 服务器是否正在运行： 
    
    sudo netstat -tap | grep mysql
    

当您运行该命令时，您可以看到类似下面的行： 
    
    tcp 0 0 localhost.localdomain:mysql *:* LISTEN -
    

如果服务器不能正常运行，您可以通过下列命令启动它： 
    
    sudo /etc/init.d/mysql restart
    

###### [[编辑][119]] 4.13.1.2. 配置

   [119]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=91 (编辑段落：4.13.1.2. 配置)

缺省状态下，管理员密码是没有设置的。一旦您安装了 MySQL，您必须要做的第一件事就是配置 MySQL 的管理员密码。要做到这一点，可以运行以下命令： 
    
    sudo mysqladmin -u root password newrootsqlpassword
    sudo mysqladmin -u root -h localhost password newrootsqlpassword
    

您可以编辑 /etc/mysql/my.cnf 文件来进行基本设置 -- 日志文件、端口号等。详情请参考 /etc/mysql/my.cnf 文件。 

##### [[编辑][120]] 4.13.2. PostgreSQL

   [120]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=92 (编辑段落：4.13.2. PostgreSQL)

PostgreSQL 是一个面向对象的数据库系统，它有着传统商业数据库系统和下一代 DBMS 系统所增进的功能。 

###### [[编辑][121]] 4.13.2.1. 安装

   [121]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=93 (编辑段落：4.13.2.1. 安装)

要安装 PostgreSQL，可以在命令提示符后运行下列命令： 
    
    sudo apt-get install postgresql
    

一旦安装完成，您就要按您的需要配置 PostgreSQL 服务器，尽管缺省配置已经可以使它可以正常运行了。 

###### [[编辑][122]] 4.13.2.2. 配置

   [122]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=94 (编辑段落：4.13.2.2. 配置)

缺省状态下，通过 TCP/IP 的连接是被禁用的。PostgreSQL 支持多客户认证方式。其中 IDENT 认证方式被默认使用。请参考 PostgreSQL 管理员指南。 

接下来的讨论假定您希望启用 TCP/IP 连接并对客户认证使用 MD5 模式。PostgreSQL 配置文件被保存在 /etc/postgresql/<version>/main /etc/postgresql/7.4/main 

要配置 ident 认证，请在 /etc/postgresql/7.4/main/pg_ident.conf 文件中添加。 

要启用 TCP/IP 连接，编辑文件 /etc/postgresql/7.4/main/postgresql.conf。 

找到 #tcpip_socket == false 行并将其改为 tcpip_socket == true。如果您知道您正在做什么，那么您也可以编辑其他所有参数。详情请参考配置文件或 PostgreSQL 文档。 

缺省状态下，没有为 MD5 客户端认证设置用户证书。因此，首先必须将 PostgreSQL 服务器配置成使用 trust 客户端认证，用以连接数据库。配置密码并将配置恢复成使用 MD5 客户端认证。要启用 trust 客户端认证，可编辑文件 /etc/postgresql/7.4/main/pg_hba.conf 

注释所有使用 ident 和 MD5 客户端认证的行并添加下列行： 
    
    local all postgres trust sameuser
    

然后运行下列命令启动 PostgreSQL 服务器： 
    
    sudo /etc/init.d/postgresql start
    

一旦 PostgreSQL 服务器成功启动，在终端提示符后运行下面的命令以连接缺省的 PostgreSQL 模板数据库。 
    
    psql -U postgres -d template1
    

上面的命令是以用户 postgres 的身份连接 PostgreSQL 的 template1 数据库。一旦您连到 PostgreSQL 服务器，您将会在 SQL 提示符下。您可以在 psql 提示符中运行下列命令来为用户 postgres 配置密码。 
    
    template1=# ALTER USER postgres with encrypted password 'your_password';
    

在配置密码后，编辑文件 /etc/postgresql/7.4/main/pg_hba.conf 以使用 MD5 认证： 

注释掉最近添加的 trust 行并添加下面行： 
    
    local all postgres md5 sameuser
    

无论如何上面配置并不完整。更多的配置参数请参考 PostgreSQL 管理员指南。 

#### [[编辑][123]] 邮件服务

   [123]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=95 (编辑段落：邮件服务)

在网络或 Internet 上从一个人得到邮件给另一个人的处理过程包含许多系统的协同工作。这些系统中的每一个都必须配置正确以便可以正常工作。发送者使用一个 邮件用户代理 (MUA)，或邮件客户端通过一个或多个 邮件传输代理 (MTA) 来发送信息，最后一个将信息送到 邮件投递代理 (MDA) 以便将其投递到接受者的收件箱中。该信息将会被接受者邮件客户端检索到，通常是通过 POP3 或 IMAP 服务器。 

##### [[编辑][124]] 4.14.1. Postfix

   [124]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=96 (编辑段落：4.14.1. Postfix)

Postfix 是 Ubuntu 中缺省的邮件传输代理 (MTA)。它试图变得快捷、易于管理和安全。它与 MTA sendmail 兼容。这部分内容说明如何安装和配置 postfix。还说明如何将它设置成使用安全连接的 SMTP 服务器 (为了安全发送邮件)。 

###### [[编辑][125]] 4.14.1.1. 安装

   [125]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=97 (编辑段落：4.14.1.1. 安装)

要安装带有 SMTP-AUTH 和 传输层安全 (TLS) 的 postfix，运行下列命令： 
    
    sudo apt-get install postfix
    

当安装进程提问时简单地按回车，下面将详细说明相关配置。 

###### [[编辑][126]] 4.14.1.2. 基本配置

   [126]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=98 (编辑段落：4.14.1.2. 基本配置)

要配置 postfix，运行下列命令： 
    
    sudo dpkg-reconfigure postfix
    

用户界面将显示。在每一屏中，选择下列值： 
    
    **** Ok
    **** Internet 站点
    **** NONE
    **** mail.example.com
    **** mail.example.com, localhost.localdomain, localhost
    **** No
    **** 127.0.0.0/8
    **** Yes
    **** 0
    **** +
    **** 全部
    
    

把 mail.example.com 作为您的邮件服务器的主机名。 

###### [[编辑][127]] 4.14.1.3. SMTP 认证

   [127]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=99 (编辑段落：4.14.1.3. SMTP 认证)

下一步是为 postfix 的 SMTP AUTH 配置使用 SASL。与其直接编辑配置，不如使用 postconf 命令配置所有 postfix 参数。配置参数被保存在 /etc/postfix/main.cf 文件中。如果您希望更新配置一项参数，您可以运行命令或手工在文件中修改。 

  *     *       * 配置 Postfix 使用 SASL (saslauthd) 的 SMTP AUTH ：
    
    postconf -e 'smtpd_sasl_local_domain ='
    postconf -e 'smtpd_sasl_auth_enable = yes'
    postconf -e 'smtpd_sasl_security_options = noanonymous'
    postconf -e 'broken_sasl_auth_clients = yes'
    postconf -e 'smtpd_recipient_restrictions = permit_sasl_authenticated,permit_mynetworks,reject_unauth_destination'
    postconf -e 'inet_interfaces = all'
    echo 'pwcheck_method: saslauthd' >> /etc/postfix/sasl/smtpd.conf
    echo 'mech_list: plain login' >> /etc/postfix/sasl/smtpd.conf
    

  *     *       * 接下来为 TLS 配置数字证书。当被询问问题时，按照提示并适当回答。
    
    openssl genrsa -des3 -rand /etc/hosts -out smtpd.key 1024
    chmod 600 smtpd.key
    openssl req -new -key smtpd.key -out smtpd.csr
    openssl x509 -req -days 3650 -in smtpd.csr -signkey smtpd.key -out smtpd.crt
    openssl rsa -in smtpd.key -out smtpd.key.unencrypted
    mv -f smtpd.key.unencrypted smtpd.key
    openssl req -new -x509 -extensions v3_ca -keyout cakey.pem -out cacert.pem -days 3650
    mv smtpd.key /etc/ssl/private/
    mv smtpd.crt /etc/ssl/certs/
    mv cakey.pem /etc/ssl/private/
    mv cacert.pem /etc/ssl/certs/
    

您可以从证书颁发机构得到数字证书。或者您可以创建您自己的证书。详情参考 第4.10.3.4节 ― 创建一个自己签署的证书。 

  *     *       * 配置 Postfix 对接收或发送邮件进行 TLS 加密：
    
    postconf -e 'smtpd_tls_auth_only = no'
    postconf -e 'smtp_use_tls = yes'
    postconf -e 'smtpd_use_tls = yes'
    postconf -e 'smtp_tls_note_starttls_offer = yes'
    postconf -e 'smtpd_tls_key_file = /etc/ssl/private/smtpd.key'
    postconf -e 'smtpd_tls_cert_file = /etc/ssl/certs/smtpd.crt'
    postconf -e 'smtpd_tls_CAfile = /etc/ssl/certs/cacert.pem'
    postconf -e 'smtpd_tls_loglevel = 1'
    postconf -e 'smtpd_tls_received_header = yes'
    postconf -e 'smtpd_tls_session_cache_timeout = 3600s'
    postconf -e 'tls_random_source = dev:/dev/urandom'
    postconf -e 'myhostname = mail.example.com'
    

在您运行所有命令之后，postfix 的 SMTP AUTH 将被配置。将为 TLS 创建自己签署的证书并与 postfix 一起配置。 

现在文件 /etc/postfix/main.cf 看上去就象 这样。 

postfix 初始配置完成。运行下列命令以开始 postfix 守护程序： 
    
    sudo /etc/init.d/postfix start
    

现在 postfix 已经被安装、配置及成功运行。Postfix 支持在 RFC2554 中定义的 SMTP AUTH。它基于 SASL。无论如何在您使用 SMTP 之前必须设置 SASL 认证。 

###### [[编辑][128]] 4.14.1.4. 配置 SASL

   [128]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=100 (编辑段落：4.14.1.4. 配置 SASL)

libsasl2、sasl2-bin 和 libsasl2-modules 对于使用 SASL 的 SMTP AUTH 是必需的。如果您没有安装它们的话，您可以安装这些应用程序。 
    
    apt-get install libsasl2 sasl2-bin
    

要让其正常工作的话做一些改动是必须的。因为 Postfix 是被 chroot 在 /var/spool/postfix 中运行，SASL 需要被配置在假根目录中运行 (从 /var/run/saslauthd 到 /var/spool/postfix/var/run/saslauthd)： 
    
    mkdir -p /var/spool/postfix/var/run/saslauthd
    rm -rf /var/run/saslauthd
    

要激活 saslauthd，编辑文件 /etc/default/saslauthd，并修改或添加 START 变量。为了将 saslauthd 配置成在假根目录中运行，添加 PWDIR、PIDFILE 和 PARAMS 变量。最终，随您所好配置 MECHANISMS 变量。该文件看起来象这样： 
    
    START=yes
    
    PWDIR="/var/spool/postfix/var/run/saslauthd"
    PARAMS="-m ${PWDIR}"
    PIDFILE="${PWDIR}/saslauthd.pid"
    
    
    MECHANISMS="pam"
    
    

如果您喜欢，您可以使用 shadow 代替 pam。这将使用 MD5 哈希密码传输并更为安全。需要认证的用户名和密码将是您正在服务器上使用系统的那些用户。 

接下来更新 /var/spool/portfix/var/run/saslauthd 的 dpkg "state"。saslauthd 初始化脚本将使用该设置来创建有着适当权限和所有权的目录： 
    
    dpkg-statoverride --force --update --add root sasl 755 /var/spool/postfix/var/run/saslauthd
    

###### [[编辑][129]] 4.14.1.5. 测试

   [129]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=101 (编辑段落：4.14.1.5. 测试)

SMTP AUTH 配置完成。现在是启动并测试设置的时候了。您可以运行下列命令来启动 SASL 守护程序： 
    
    sudo /etc/init.d/saslauthd start
    

要查看 SMTP-AUTH 和 TLS 是否正常工作，运行下列命令： 
    
    telnet mail.example.com 25
    

在您建立到 postfix 邮件服务器连接之后，输入： 
    
    ehlo mail.example.com
    

如果您看到包括下列行时，那么一切工作正常。输入 quit 退出。 
    
    250-STARTTLS
    250-AUTH LOGIN PLAIN
    250-AUTH=LOGIN PLAIN
    250 8BITMIME
    

##### [[编辑][130]] 4.14.2. Exim4

   [130]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=102 (编辑段落：4.14.2. Exim4)

Exim4 是另一个由剑桥大学开发的用于连接 Internet 的 Unix 系统上的消息传输代理 (MTA)。安装Exim可以代替 sendmail，虽然 exim 的配置与 sendmail 是非常不同。 

###### [[编辑][131]] 4.14.2.1. 安装

   [131]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=103 (编辑段落：4.14.2.1. 安装)

要安装 exim4，运行下列命令： 
    
    sudo apt-get install exim4 exim4-base exim4-config
    

###### [[编辑][132]] 4.14.2.2. 配置

   [132]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=104 (编辑段落：4.14.2.2. 配置)

要配置 exim4，运行下列命令： 
    
    sudo dpkg-reconfigure exim4-config
    

用户界面将显示。该用户界面让您配置许多参数。例如，在 exim4 中配置文件被分成多个文件。如果您希望将它们放在一个文件中，您可以根据该用户界面进行配置。 

您在用户界面配置的所有参数被保存在 /etc/exim4/update-exim4.conf.conf 文件。如果您希望重新配置，您可以重新运行配置向导或用您喜欢的编辑器手工编辑该文件。一旦您配置好了，您可以运行下列命令来生成主配置文件： 
    
    sudo update-exim4.conf
    

生成主配置文件且被保存在 /var/lib/exim4/config.autogenerated. 

在任何时候，你都不要手工编辑主配置文件 /var/lib/exim4/config.autogenerated。它在每次您运行 update-exim4.conf 之后会自动更新。 

您可以运行下列命令以启动 exim4 守护程序。 
    
    sudo /etc/init.d/exim4 start
    

TODO: 该内容将覆盖 exim4 的 SMTP AUTH 配置。 

##### [[编辑][133]] 4.14.3. Dovecot 服务器

   [133]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=105 (编辑段落：4.14.3. Dovecot 服务器)

Dovecot 是一个主要出于安全考虑编写的邮件投递代理。它支持主要收件箱格式：mbox 或 Maidir。这部分说明如何将它设为一个 imap 或 pop3 服务器。 

###### [[编辑][134]] 4.14.3.1. 安装

   [134]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=106 (编辑段落：4.14.3.1. 安装)

要安装 dovecot，在命令提示符中运行下列命令： 
    
    sudo apt-get install dovecot-common dovecot-imapd dovecot-pop3d
    

###### [[编辑][135]] 4.14.3.2. 配置

   [135]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=107 (编辑段落：4.14.3.2. 配置)

要配置 dovecot，您可以编辑文件 /etc/dovecot/dovecot.conf。您可以选择您所使用的协议。它可以是 pop3、pop3 (安全 pop3)、imap 和 imaps (安全 imap)。对这些协议的说明是超出该指南范围的。更多信息请参考 wikipedia 关于 POP3 和 IMAP 的文章。 

IMAPS 和 POP3S 比简单 IMAP 和 POP3 更安全，因为他们使用 SSL 加密连接。一旦您选择了协议，修改在文件 /etc/dovecot/dovecot.conf 中的下列行： 
    
    protocols = pop3 pop3s imap imaps
    

当 dovecot 启动时协议开始生效。接下来在文件 /etc/dovecot/dovecot.conf 的 pop3 部分添加下列行： 
    
    pop3_uidl_format = %08Xu%08Xv
    

然后选择您所用的收件箱。Dovecot 支持 maildir 和 mbox 格式。大多数通常使用 mailbox 格式。它们都有自己的优点，并且在 dovecot 网站 上讨论。 

一旦您选择了您的收件箱格式后，就可以编辑文件 /etc/dovecot/dovecot.conf 并修改下列行： 
    
    default_mail_env = maildir:~/Maildir # (for maildir)
    

或 
    
    default_mail_env = mbox:~/mail:INBOX=/var/spool/mail/%u # (for mbox)
    

如果接收到的邮件类型与您已经配置不同，那么您要配置您的邮件传输代理 (MTA) 用来将该邮件传输到这种类型的收件箱中。 

一旦您已经配置好了 dovecot，启动 dovecot 守护程序以测试您的设置： 
    
    sudo /etc/init.d/dovecot start
    

如果您启用 imap 或 pop3，您也可以试着用命令 telnet localhost pop3 或 telnet localhost imap2 登录。如果您看到类似下面的东西，那么安装就成功了： 
    
    bhuvan@rainbow:~$ telnet localhost pop3
    Trying 127.0.0.1...
    Connected to localhost.localdomain.
    Escape character is '^]'.
    +OK Dovecot ready.
    

###### [[编辑][136]] 4.14.3.3. Dovecot SSL 配置

   [136]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=108 (编辑段落：4.14.3.3. Dovecot SSL 配置)

要配置 dovecot 使用 SSL，您可以编辑文件 /etc/dovecot/dovecot.conf 并修改下列行： 
    
    ssl_cert_file = /etc/ssl/certs/dovecot.pem
    ssl_key_file = /etc/ssl/private/dovecot.pem
    ssl_disable = no
    disable_plaintext_auth = no
    

当您安装 dovecot 时，会通过它自动创建 cert 和 key 文件。请注意这些钥匙没被签名并会在客户端连接时给出 "bad signature" 的错误。要避免这一点，您可以使用商业证书，甚至更好，使用您自己签署的 SSL 证书。 

###### [[编辑][137]] 4.14.3.4. 邮件服务器的防火墙配置

   [137]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=109 (编辑段落：4.14.3.4. 邮件服务器的防火墙配置)

要从另一台计算机访问您的邮件服务器，您必须配置您的防火墙以允许连接服务器必要的端口。 
    
    **** IMAP - 143
    **** IMAPS - 993
    **** POP3 - 110
    **** POP3S - 995
    

##### [[编辑][138]] 4.14.4. Mailman

   [138]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=110 (编辑段落：4.14.4. Mailman)

Mailman 是一个管理电子邮件讨论及电子通讯列表的开源程序。许多开源的邮件列表 (包括所有的 Ubuntu 邮件列表)使用 Mailman 作为他们的邮件列表软件。它是强大的且易于安装和维护。 

###### [[编辑][139]] 4.14.4.1. 安装

   [139]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=111 (编辑段落：4.14.4.1. 安装)

Mailman 为管理员和用户提供一个 web 界面。因此，它要求 apache 要有 mod_perl 的支持。Mailman 使用外部邮件服务器来发送和接收邮件。它可以与下列邮件服务器很好的工作： 

  *     *       *         * Postfix
        * Exim
        * Sendmail
        * Qmail

我们将看到如何安装 mailman、apache web 服务器和 Exim 邮件服务器。如果您希望安装 mailman 和一个不同的邮件服务器，请参考参考部分。 

###### [[编辑][140]] = 4.14.4.1.1. Apache2 =

   [140]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=112 (编辑段落：= 4.14.4.1.1. Apache2 =)

要安装 apache2 您可以参考 第4.10.1节 ― 安装。 

###### [[编辑][141]] = 4.14.4.1.2. Exim4 =

   [141]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=113 (编辑段落：= 4.14.4.1.2. Exim4 =)

要安装 Exim4 您可以在终端提示符后运行下列命令： 
    
    sudo apt-get install exim4
    sudo apt-get install exim4-base
    sudo apt-get install exim4-config
    

一旦安装好 exim4，配置文件被保存在 /etc/exim4 目录中。缺省情况下，在 ubuntu 中，exim4 配置文件被分成不同的文件。您可以通过配置 /etc/exim4/update-exim4.conf 中的下列变量来改变这一现状： 

  *     *       *         * dc_use_split_config='true'

###### [[编辑][142]] = 4.14.4.1.3. Mailman =

   [142]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=114 (编辑段落：= 4.14.4.1.3. Mailman =)

要安装 Mailman，在终端提示符后运行下列命令： 
    
    sudo apt-get install mailman
    

它复制安装文件到 /var/lib/mailman 目录，将 CGI 脚本安装到 /usr/lib/cgi-bin/mailman目录，创建 list linux 用户，创建 list linux 用户组。mailman 进程将以该用户运行。 

###### [[编辑][143]] 4.14.4.2. 配置

   [143]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=115 (编辑段落：4.14.4.2. 配置)

本节假定您已经成功安装 mailman、apache2 和exim4。现在您只需要配置它们。 

###### [[编辑][144]] = 4.14.4.2.1. Apache2 =

   [144]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=116 (编辑段落：= 4.14.4.2.1. Apache2 =)

一旦 apache2 安装之后，您可以在 /etc/apache2/apache2.conf 文件添加下列行： 
    
    Alias /images/mailman/ "/usr/share/images/mailman/"
    Alias /pipermail/ "/var/lib/mailman/archives/public/"
    

Mailman 使用 apache2 来运行它的 CGI 脚本。mailman 的 CGI 脚本被安装在 /usr/lib/cgi-bin/mailman 目录中。因此 mailman 的 url 将是 [http://hostname/cgi-bin/mailman/。如果您希望改变这一状况，您可以修改][145] /etc/apache2/apache2.conf 文件。 

   [145]: http://hostname/cgi-bin/mailman/%E3%80%82%E5%A6%82%E6%9E%9C%E6%82%A8%E5%B8%8C%E6%9C%9B%E6%94%B9%E5%8F%98%E8%BF%99%E4%B8%80%E7%8A%B6%E5%86%B5%EF%BC%8C%E6%82%A8%E5%8F%AF%E4%BB%A5%E4%BF%AE%E6%94%B9 (http://hostname/cgi-bin/mailman/。如果您希望改变这一状况，您可以修改)

###### [[编辑][146]] = 4.14.4.2.2. Exim4 =

   [146]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=117 (编辑段落：= 4.14.4.2.2. Exim4 =)

一旦 Exim4 安装之后，您可以在终端提示符后使用下列命令启动 Exim 服务器： 
    
    sudo apt-get /etc/init.d/exim4 start
    

为了使 mailman 可以和 exim4 一起工作，您需要配置 exim4。正如先前所说的那样，在缺省状态下 exim4 使用不同类型的多个配置文件。详情请参考 Exim 网站。要运行 mailman，我们可以新建一个配置文件到下列配置类型： 

  *     *       *         * 主
        * 传输
        * 路由器

Exim 按这些小的配置文件顺序创建出一个主配置文件。因此这些配置文件的顺序是非常重要的。 

###### [[编辑][147]] = 4.14.4.2.3. 主 =

   [147]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=118 (编辑段落：= 4.14.4.2.3. 主 =)

所有隶属于主类别的配置文件都被保存在 /etc/exim4/conf.d/main/ 目录中。您可以将下面的内容添加到一个名为 04_exim4-config_mailman 的新文件中： 
    
    MM_HOME=/var/lib/mailman
    MM_UID=list
    MM_GID=list
    domainlist mm_domains=hostname.com↵
    MM_WRAP=MM_HOME/mail/mailman
    MM_LISTCHK=MM_HOME/lists/${lc::$local_part}/config.pck
    
    

###### [[编辑][148]] = 4.14.4.2.4. 传输 =

   [148]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=119 (编辑段落：= 4.14.4.2.4. 传输 =)

所有隶属于传输类型的文件被保存在 /etc/exim4/conf.d/transport/ 目录中。您可以将下面的内容添加到一个名为 40_exim4-config_mailman 的新文件中： 
    
    mailman_transport:
    driver = pipe
    command = MM_WRAP \
    '${if def:local_part_suffix \
    {${sg{$local_part_suffix}{-(\\w+)(\\+.*)?}{\$1}} } \
    {post}}' \
    $local_part
    current_directory = MM_HOME
    home_directory = MM_HOME
    user = MM_UID
    group = MM_GID
    

###### [[编辑][149]] = 4.14.4.2.5. 路由器 =

   [149]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=120 (编辑段落：= 4.14.4.2.5. 路由器 =)

所有隶属于路由类的所有配置文件都被保存在 /etc/exim4/conf.d/router/ 目录中。您可以将下列内容添加到名为 101_exim4-config_mailman 的新文件中： 
    
    mailman_router:
    driver = accept
    require_files = MM_HOME/lists/$local_part/config.pck
    local_part_suffix_optional
    local_part_suffix = -bounces : -bounces+* : \
    -confirm+* : -join : -leave : \
    -owner : -request : -admin
    transport = mailman_transport
    

主类和传输类的配置文件的顺序可以随意。但路由类的配置文件的顺序必须相同。该文件必须在 200_exim4-config_primary 文件之前出现。如果两个配置文件包含相同类型的信息。第一个文件优先。详情请参阅参考部分。 

###### [[编辑][150]] = 4.14.4.2.6. Mailman =

   [150]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=121 (编辑段落：= 4.14.4.2.6. Mailman =)

安装 mailman 之后，您可以使用下列命令来运行它： 
    
    sudo /etc/init.d/mailman start
    

一旦 mailman 安装，您必须创建缺省的邮件列表。运行下列命令以创建邮件列表： 
    
    sudo /usr/sbin/newlist mailman
    
    Enter the email address of the person running the list: bhuvan at ubuntu.com
    Initial mailman password:
    To finish creating your mailing list, you must edit your /etc/aliases (or
    equivalent) file by adding the following lines, and possibly running the
    `newaliases' program:
    
    ## mailman mailing list
    mailman: "|/var/lib/mailman/mail/mailman post mailman"
    mailman-admin: "|/var/lib/mailman/mail/mailman admin mailman"
    mailman-bounces: "|/var/lib/mailman/mail/mailman bounces mailman"
    mailman-confirm: "|/var/lib/mailman/mail/mailman confirm mailman"
    mailman-join: "|/var/lib/mailman/mail/mailman join mailman"
    mailman-leave: "|/var/lib/mailman/mail/mailman leave mailman"
    mailman-owner: "|/var/lib/mailman/mail/mailman owner mailman"
    mailman-request: "|/var/lib/mailman/mail/mailman request mailman"
    mailman-subscribe: "|/var/lib/mailman/mail/mailman subscribe mailman"
    mailman-unsubscribe: "|/var/lib/mailman/mail/mailman unsubscribe mailman"
    
    Hit enter to notify mailman owner...
    
    # 
    
    

我们已经把 exim 配置成可以识别所有来自 mailman 的邮件。因此并不需要在 /etc/aliases 中添加任何新的条目。如果您对配置文件做了改动，请确保您在进入下一章节之前重启了这些服务。 

###### [[编辑][151]] 4.14.4.3. 管理

   [151]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=122 (编辑段落：4.14.4.3. 管理)

我们假定您已经进行了缺省安装。mailman 的 cgi 脚本应该在 /usr/lib/cgi-bin/mailman/ 目录中。Mailman 提供了一个基于 web 管理工具。可以在您的浏览器输入下列 url 访问该页： 
    
    http://hostname/cgi-bin/mailman/admin
    

缺省的邮件列表 mailman 将出现在屏幕上。如果您点击邮件列表名，它将询问您的认证密码。如果您输入了正确的密码，您将可以改变该邮件列表的管理设置。您可以使用命令行工具 (/usr/sbin/newlist) 创建一个新的邮件列表，您也可以使用 web 界面来创建新的邮件列表。 

###### [[编辑][152]] 4.14.4.4. 用户

   [152]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=123 (编辑段落：4.14.4.4. 用户)

Mailman 为用户提供了一个 web 界面，可以在您的浏览器中输入下列 url 来访问该页： 
    
    http://hostname/cgi-bin/mailman/listinfo
    

缺省邮件列表 mailman 将出现在屏幕上。如果您点击邮件列表名，它将显示订阅表单。您可以输入您的邮件地址、姓名 (可选)及密码来订阅。一个邀请邮件将发送给您。您可以根据该邮件的指示完成订阅。 

###### [[编辑][153]] 4.14.4.5. 引用

   [153]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=124 (编辑段落：4.14.4.5. 引用)

[GNU Mailman - 安装手册][154]

   [154]: http://www.list.org/mailman-install/index.html (http://www.list.org/mailman-install/index.html)

  
[指南 - 一起使用 Exim 4 和 Mailman 2.1][155]

   [155]: http://www.exim.org/howto/mailman21.html (http://www.exim.org/howto/mailman21.html)

### [[编辑][156]] Windows 联网

   [156]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=125 (编辑段落：Windows 联网)

计算机网络通常包含不同的系统，虽然使用全由 Ubuntu 桌面计算机和服务器计算机构成的网络是有趣的，但一些网络环境还是需要 Ubuntu 和 Microsoft® Windows® 这两个系统协同工作。Ubuntu 服务器指南中的这部分内容介绍配置你 Ubuntu 服务器的原理及所用工具，以便同 Windows 计算机共享网络资源。 

#### [[编辑][157]] 介绍

   [157]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=126 (编辑段落：介绍)

将你的 Ubuntu 系统与 Windows 客户机成功连网包括为 Windows 环境提供和整合常用服务。这些服务有助于网络中计算机和用户的数据和信息共享，可以将它们按功能划分为以下三大类： 

  *     *       *         * 文件和打印共享服务。Server Message Block (SMB) 协议使得在网络上共享文件、文件夹、卷和打印机变得容易。
        * 目录服务。通过Lightweight Directory Access Protocol (LDAP) 和 Microsoft Active Directory®技术来共享网络计算机和用户的重要信息。
        * 认证和权限。建立网络计算机和用户身份并通过使用文件权限、组策略和Kerberos认证服务等原理和技术来确定计算机或用户信息。

幸运的是，你的 Ubuntu 系统可以给 Windows 客户机提供上述所有的服务并且在它们之间共享网络资源。你的 Ubuntu 系统中用来和 Windows 网络互连的一个基本软件是包含 SMB 服务器应用程序及其工具。Ubuntu 服务器指南的这部分将简要介绍一下 SAMBA 套件中服务器应用程序及工具的安装和简单配置。此外，SAMBA的详细文档和信息已超出了本文档的编制范围，您可以在 SAMBA 网站 上找到。 

#### [[编辑][158]] 安装 SAMBA

   [158]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=127 (编辑段落：安装 SAMBA)

欲安装 SAMBA 服务器应用程序，请在提示符里输入如下命令： 
    
    sudo apt-get install samba
    

#### [[编辑][159]] 配置 SAMBA

   [159]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=128 (编辑段落：配置 SAMBA)

您可以通过编辑 /etc/samba/smb.conf 文件来配置 SAMBA 服务：用以改变缺省设置或新增设置。可用设置项的更多信息可以查看 /etc/samba/smb.conf 文件中的注释，或在终端提示符后输入以下命令查看 /etc/samba/smb.conf 手册来获得： 
    
    man smb.conf
    

在编辑配置文件之前，您应该保留一份原文件的副本，不对其作修改，以便今后必要时可参考和重用这份原始配置。 

备份 /etc/samba/smb.conf 文件： 
    
    sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.original
    

然后编辑 /etc/samba/smb.conf 文件，进行相应的修改。 

##### [[编辑][160]] 5.3.1. 服务器

   [160]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=129 (编辑段落：5.3.1. 服务器)

除了文件及打印共享服务器应用程序 SAMBA 之外，Ubuntu 还包括了其他强大的服务器应用程序，它们设计成象真正的 Windows 服务器一样，可以向 Windows 客户端提供相同的网络附加服务功能。例如，Ubuntu 可以提供统一的网络资源，如使用目录服务的计算机和用户，方便的身份识别，以及通过认证服务来获得授权的计算机和用户。 

下一节将讨论 SAMBA 及其支持技术，如 Lightweight Directory Access Protocol (LDAP) 服务器和 Kerberos 认证服务器的更多细节。你也可以学到SAMBA配置文件中的一些有用的指令，以方便同 Windows 客户机和服务器集成。 

###### [[编辑][161]] 5.3.1.1. Active Directory

   [161]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=130 (编辑段落：5.3.1.1. Active Directory)

Active Directory 是 Microsoft 专用的目录服务实现，用来提供网络资源和用户的共享信息。除了提供这些信息的统一源之外，Active Directory 也担任网络统一身份安全认证的角色。Active Directory 融合了通常独立、专用的目录系统中简单集成，管理和网络资源安全等功能。SAMBA 软件包可以被配置成使用由 Windows 域控制器提供的 Active Directory 服务。 

###### [[编辑][162]] = 5.3.1.1.1. LDAP =

   [162]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=131 (编辑段落：= 5.3.1.1.1. LDAP =)

LDAP 服务器应用程序给 Windows 计算机提供了与 Microsoft Active Directory 服务非常相似的目录服务功能。这些服务包括管理加入网络的计算机、用户、计算机组和用户组的身份和关系，为描述、定位和管理这些资源提供一个统一的方法。在你的 Ubuntu 系统中所使用的是 LDAP 的一个自由有效的实现，名为OpenLDAP，这个名为slapd 和 slurpd的服务守护程序负责处理 OpenLDAP 目录请求和 LDAP 服务器与另一个 Ubuntu 服务器之间的目录数据传送。只要 SAMBA编译时加上LDAP 支持，OpenLDAP 与 SAMBA 一起就可以和Windows域服务器一样以相同的方式提供文件、打印和目录服务。 

###### [[编辑][163]] = 5.3.1.1.2. Kerberos =

   [163]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=132 (编辑段落：= 5.3.1.1.2. Kerberos =)

Kerberos 安全认证系统是一个通过一台集中的服务器给计算机和用户提供认证的标准服务，它允许从其他任何使用 Kerberos 的计算机那里接受加密的授权票据。Kerberos 验证的好处包括相互验证，委派验证，互操作性和简单信任管理。在 Ubuntu 中处理 Kerberos 验证和 Kerberos 数据库管理的主要服务器守护程序是 krb5kdc 和 kadmin。相对于 Windows 域控制器SAMBA 可以使用 Kerberos 作为一个用来给计算机和用户授权的机制。为了做到这些，Ubuntu 系统必须安装 Kerberos，同时 /etc/samba/smb.conf 也需要修改以选择适当的realm 和 security 模式。举个例子，编辑/etc/samba/smb.conf文件，并将下列语句： 
    
    realm = DOMAIN_NAME
    security = ADS
    

添加到文件中并保存 

请确定上面示例中的 DOMAIN_NAME 已经被你的 Windows 域名所代替。 

你需要重启 SAMBA 进程以使改变产生作用。重启 SAMBA 进程可以在终端提示符后输入以下命令： 
    
    sudo /etc/init.d/samba restart
    

###### [[编辑][164]] 5.3.1.2. 计算机帐号

   [164]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=133 (编辑段落：5.3.1.2. 计算机帐号)

目录服务中计算机帐号唯一指定网络中的计算机系统，甚至在安全方面就象对待用户一样。计算机账号可以象用户账号一样有密码，同样也象用户账号一样访问网络资源受授权影响。举个例子，如果一个在特定网络中有着合法账号网络用户，企图给一个没有合法计算机账号的计算机上的网络资源授权时，根据网络策略，如果用户企图验证的计算机是未经授权的计算机，则用户也许会被拒绝访问资源。 

计算机帐号可以会被添加到 SAMBA 的密码文件中，首先该计算机的名字在本地密码库中做为一个合法用户帐号被添加。添加一个计算机或机器帐号到 SAMBA 的密码文件可以在终端提示符后使用 smbpasswd 命令，如下所示： 
    
    sudo smbpasswd -a -m COMPUTER_NAME
    

请确定示例中的 COMPUTER_NAME 被那台你希望添加机器账号的计算机实际名字所代替。 

  


###### [[编辑][165]] 5.3.1.3. 文件权限

   [165]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=134 (编辑段落：5.3.1.3. 文件权限)

文件权限清楚地定义计算机和用户对指定的目录、文件或一组文件的权力。这些权限可以通过编辑/etc/samba/smb.conf 来定义并明确被定义文件共享的权限。举个例子，如果你定义了一个叫 sourcedocs 的 SAMBA 共享，并希望给 planning 用户组以 read-only 只读权限，但允许叫 authors 的用户组和名为 richard 的用户可以写该共享，那么你可以编辑/etc/samba/smb.conf 文件，并将下列条目放在 [sourcedocs] 条目下： 
    
    read list = @planning
    write list = @authors, richard
    

将其保存在 /etc/samba/smb.conf中，以使其生效。 

另一个适合的权限就是对特定的共享资源声明 administrative 权限。有着administrative 权限的用户可以读、写或修改任何包含在用户已明确给予 administrative 权限的资源里的任何信息。举个例子，如果你想给用户 melissa 在示例 sourcedocs 共享上的 administrative 权限，你可以编辑/etc/samba/smb.conf 文件，并将下面一行添加到 [sourcedocs] 条目下： 
    
    admin users = melissa
    

将其保存在 /etc/samba/smb.conf中，以使其生效。 

##### [[编辑][166]] 5.3.2. 客户端

   [166]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=135 (编辑段落：5.3.2. 客户端)

Ubuntu 中包括客户端应用以及通过 SMB 协议远程访问网络资源的能力。例如，一个名为 smbclient 的工具就可以以一种类似文件传输协议（FTP）客户端的方式访问远程共享的文件系统。以使用 smbclient 来访问在名为 bill 远程 Windows 计算机中的共享文件夹资源 documents 来做示范，在提示符后输入类似下面的命令： 
    
    smbclient //bill/documents -U <username>
    

然后你将被提示输入相对于 -U 参数之后用户名的密码，当成功验证后，将出现一个提示符，在那里我们可以使用与非图形化 FTP 客户端类似的语法来操作和传输文件。smbclient 工具的详细信息可以输入以下命令来查阅该工具的手册页： 

使用 SMB 协议的远程网络资源在本地挂载也可以使用 mount 命令。例如，要挂载在一个名为 development 的Windows 服务器上的一个名为 project-code 共享文件夹，并使用 dlightman 用户名挂载到你的 Ubuntu 系统的 /mnt/pcode 挂载点上，你可以在提示符后运行以下命令： 
    
    mount -t smbfs -o username=dlightman //development/project-code /mnt/pcode
    

然后你将被提示输入用户密码，在成功验证之后，共享资源的内容就可以通过 mount 命令中的最后一个参数所指定的本地挂载点来访问了。要卸载该共享资源，就象其他被挂载的文件系统一样只需简单的使用 umount 命令即可。例如： 
    
    umount /mnt/pcode
    

###### [[编辑][167]] 5.3.2.1. 用户帐号

   [167]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=136 (编辑段落：5.3.2.1. 用户帐号)

用户帐号定义了有一些授权来使用指定计算机和网络资源的人。通常，在网络环境中，用户帐号被提供给每个允许访问计算机或网络的人，然后这些计算机或网络根据策略和权限来明确用户帐号拥有多大的权力。要在你的 Ubuntu 系统中定义 SAMBA 网络用户，你可能使用smbpasswd 命令。如要为你 Ubuntu 系统上名为jseinfeld 的用户添加一个 SAMBA 用户，你可以在提示符后输入命令： 
    
    smbpasswd -a jseinfeld
    

然后 smbpasswd 应用程序将提示你为该用户输入密码： 
    
    New SMB password:
    

输入你想为用户设置的密码，smbpasswd 会要求你确认密码： 
    
    Retype new SMB password:
    

确认密码之后，smbpasswd 将为用户在 SAMBA 密码文件中添加一个条目。 

###### [[编辑][168]] 5.3.2.2. 组

   [168]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=137 (编辑段落：5.3.2.2. 组)

组定义了对指定网络资源拥有共同访问级别的一组计算机或用户，并提供对这些资源的访问控制粒度级别。举个例子，如果定义一个 qa 组并包含用户 freda、danika 和 rob，再定义第二个组 support 并包含 danika、jeremy 和 vincent。那么某个网络资源被配置为允许 qa 组访问时，则其可以被 freda、danika 和 rob访问，而不是 jeremy 或 vincent。因为用户 danika 属于 qa 和 support 两个组，所以她能够访问配置为两个组访问的资源，而所有其他用户则只能访问明确允许其所属组访问的资源。 

当在 SAMBA 配置文件 /etc/samba/smb.conf 中定义组时，被认可的写法是在用“@”符作为组名的开始。例如，如果你希望在 /etc/samba/smb.conf 某段中定义一个名为 sysadmin 的组，那么你需要输入 @sysadmin 来作为组名。 

###### [[编辑][169]] 5.3.2.3. 组策略

   [169]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&action=edit&section=138 (编辑段落：5.3.2.3. 组策略)

组策略为 SAMBA 服务器定义属于域或工作组计算机帐号的 SAMBA 配置设置及其他全局设置。举个例子，如果 SAMBA 服务器属于一个名为 LEVELONE 的 Windows 计算机工作组，那么 /etc/samba/smb.conf 可以相应改成下面的值： 
    
    workgroup = LEVELONE
    

  
保存文件并重启 SAMBA 守护程序以使改变生效。 

其它重要的全局策略设置包括 server string，定义了 Ubuntu 系统向基于 Windows 网络上其它机器所宣称的 NETBIOS 服务器名称。该名称也使你的 Ubuntu 系统被 Windows 客户端及其它使用 SMB 协议浏览网络的计算机所识别。而且用户还可以通过 /etc/samba/smb.conf 文件中的 log file 指令来指定 SAMBA 服务器日志文件的名称和所在位置。 

还有一些可以管理全局组策略的附加指令包括对所有共享资源全局性能的说明。例如在 /etc/samba/smb.conf 中 [global] 标题下面的指令将影响所有共享资源，除非在某些共享资源标题下有覆盖全局设置的指令。你可以通过 browseable 指令来使网络中所有的客户机都能浏览全部共享。该指令使用一个布尔值，位于 /etc/samba/smb.conf 文件的 [global] 标题中。编辑该文件，并添加下面一行： 
    
    browseable = true
    

到 /etc/samba/smb.conf 文件的 [global] 节的下面。这样所有你 Ubuntu 系统中的共享将通过 SAMBA 被授权客户机浏览，除非该共享包含了覆盖全局指令的 browseable = false 指令。 

相同方式运作的其他例子还有 public 和 writeable 指令。public 指令接受一个布尔值并决定特定的共享资源对所有客户是否可见，而无论其被授权与否。writeable 指令也采用布尔值以定义特定共享资源是否对部分或所有网络客户可写。 

取自“[http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&variant=zh-cn][170]” 

   [170]: http://wiki.ubuntu.org.cn/index.php?title=Ubuntu%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97&variant=zh-cn

