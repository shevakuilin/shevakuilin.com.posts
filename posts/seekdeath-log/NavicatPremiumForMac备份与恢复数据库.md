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

## 数据库备份

首先我们需要建立 Navicat Premium 与服务器数据库的连接，以此来进行服务器数据库的备份。

### 步骤一、创建安全组规则

这里以阿里云为例，如果你已经设置过服务器的安全组规则，并且开放了数据库的访问权限，那么可以**直接跳过**步骤一和步骤二。

访问**阿里云主页** -> 进入**控制台** -> 找到**云服务器 ECS** -> 在**网络与安全**分类中找到**安全组** 

