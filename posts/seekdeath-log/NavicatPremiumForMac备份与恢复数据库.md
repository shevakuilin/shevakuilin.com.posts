# Navicat Premium For Mac 备份与恢复数据库

地址：https://shevakuilin.com/use-navicat-premium-for-mac/

创建时间：2020-01-10

## 简介

在 macOS 系统上使用 Navicat Premium 对服务器上的数据库进行本地备份，以及数据库丢失后的恢复

## 引言

对于数据库备份的重要性，无需多言，本文是一篇关于在 macOS 系统上使用 Navicat Premium 的教程。

教程中使用的环境：

- 操作系统：macOS Catalina 10.15.1
- Navicat Premium：15.0.3
- 服务器：阿里云/centOS
- 数据库：5.5.60-MariaDB

##Navicat Premium

[Navicat Premium](https://www.navicat.com.cn/) 是一套数据库开发工具，能够让你从单一应用程序中同时连接 MySQL、MariaDB、MongoDB、SQL Server、Oracle、PostgreSQL 和 SQLite 数据库。它与 Amazon RDS、Amazon Aurora、Amazon Redshift、Microsoft Azure、Oracle Cloud、MongoDB Atlas、阿里云、腾讯云和华为云等云数据库兼容。你可以快速轻松地创建、管理和维护数据库。对于操作系统来说， Winodws、macOS 以及 Linux 都可以无缝兼容。

本文主要介绍如何利用 Navicat Premium 中自带的备份工具进行备份和恢复。

## 数据库连接

首先我们需要建立 Navicat Premium 与服务器数据库的连接，以此来进行服务器数据库的备份。这里以阿里云为例，如果你已经设置过服务器的安全组规则，并且开放了数据库的访问权限，那么可以**直接跳过**步骤一和步骤二。

### 步骤一、创建安全组规则

1.访问**阿里云主页** -> 进入**控制台** -> 选中**云服务器 ECS**

<img src="https://github.com/shevakuilin/GhostPostsImages/raw/master/0CD6A5A9-3368-4F89-9A65-39478ACB5218.png" width="800" height ="603" />

2.在**网络与安全**分类中选中**安全组**

<img src="https://github.com/shevakuilin/GhostPostsImages/raw/master/05F36712-AE39-439E-9EF3-CF11C02B4E2D.png" width="317" height ="800" />

3.点击**配置规则**

<img src="https://github.com/shevakuilin/GhostPostsImages/raw/master/454BD119-0ECA-49B7-BF4D-D48BAAFC1451.png" width="800" height ="229" />

4.选择**添加安全组规则**，照搬图上的配置即可

<img src="https://github.com/shevakuilin/GhostPostsImages/raw/master/7445E51C-5C92-4A57-A613-2194875AD35D.png" width="800" height ="563" />

### 步骤二、开放数据库访问权限

在数据库中执行如下命令

```vim
$ GRANT ALL PRIVILEGES ON *.* TO 'root'@'xxx.xxx.xxx.xxx' IDENTIFIED BY 'xxxxxx' WITH grant option;
```

**ALL PRIVILEGES**：表示开放全部数据库权限，当然那你也可以只提供部分权限
**.** ：表示开放所有权限范围，这里的范围指的是数据库和表
**root** ：表示该数据库的用户名
**xxx.xxx.xxx.xxx** ：表示其公有IP地址
**BY 'xxxxxx' WITH**：表示数据库密码

```vim
$ flush privileges;
```

### 步骤三、通过 SSH 在本地连接到云服务器上的数据库

打开 Navicat Premium，点击 Connection 进行数据库连接，会出现一个下拉菜单，需要你选择所使用的的数据库类型。本文使用的是 MariaDB，它是 MySQL 的一个开源版本，所以直接选择 MySQL 即可。

<img src="https://github.com/shevakuilin/GhostPostsImages/raw/master/65C787C9-FA10-4560-8AD0-D2B1299410B3.png" width="800" height ="530" />

设置通用配置，填写连接名称、数据库用户名和密码，其他保持默认不需要修改

<img src="https://github.com/shevakuilin/GhostPostsImages/raw/master/FDFB8368-534C-4F7C-821E-178E666CFDE1.png" width="800" height ="736" />

设置 SSH 配置，填写服务器公有 IP 地址、用户名和密码，完成后点击保存，左侧的连接列表中会出现绿色图标的连接名称，连接名称就是你在通用配置中设置的名字，表示连接成功，如果连接不上，关一下服务器的防火墙即可

<img src="https://github.com/shevakuilin/GhostPostsImages/raw/master/75F7376B-BEA7-4EF8-AE73-7289629911B3.png" width="800" height ="678" />

## 数据库备份

双击上面刚创建的那个已连接的`连接名称`（如果未连接，右键选中连接名称，出现的下拉菜单中选择连接即可），选中你想要进行备份的数据库，双击展开，右键 `Backups` 出现下拉菜单，选择 `New Backup...` 进行备份

<img src="https://github.com/shevakuilin/GhostPostsImages/raw/master/0404A1F7-4439-4244-AD7A-9EB181CD3B6E.png" width="524" height ="772" />

点击`Start`，进行备份，看到提示成功即可关闭该页面，备份成功后，展开 `Backups` 后下面会出现刚刚备份完成的数据库备份文件，文件以时间戳为名，`nb3` 为后缀。备份完成的文件默认保存在 `/Users/用户名/Library/Application Support/PremiumSoft CyberTech/Navicat CC/Common/Settings/0/0/MySQL（数据库类型）/连接名称/备份的数据库名称` 路径下，你也可以在其他地方多保存几份该文件，**推荐多保存在不同的几个地方，防止炸库的问题**

<img src="https://github.com/shevakuilin/GhostPostsImages/raw/master/94FA4589-6BCF-43FD-BBBA-A616ECAE3E5F.png" width="800" height ="619" />

## 数据库恢复

假如你不幸遇到了数据库炸库或者误删除了数据库，导致整个数据库都没了，你会发现之前的备份文件连同数据库一同消失了，因为 Navicat Premium 的备份文件是默认保存在其数据库名称的路径下的，数据库消失了，其备份也跟随消失，这点比较坑，所以需要你多保存几份到其他地方，比如开个 gitlab 的私有库等等。如果你恰好偷懒没有多留几个本地备份，就只能使用 binlog 来回滚了，谁让你自己偷懒呢 🤷‍♂️。

假如勤快的你多留了几个本地备份，那么你需要在 Navicat Premium 中连接到你的服务器，并且在里面新建一个同名数据库（最好是保持同名，因为你的数据库索引可能还是之前的名字），双击进入这个新的数据库下，右键 `Backups` ，选择 `Restore Backup from...` 导入备份文件，然后双击备份文件，点击 `Start` 开始恢复数据库

<img src="https://github.com/shevakuilin/GhostPostsImages/raw/master/4344B359-3E39-4342-ABFE-DEC6CA53B48F.png" width="800" height ="619" />

如果你的数据库是存在的，那就很简单了，直接双击选择备份文件，开始恢复即可

<img src="https://github.com/shevakuilin/GhostPostsImages/raw/master/F4BC8B5F-56D4-40E7-9D2D-D5326FF79216.png" width="800" height ="619" />