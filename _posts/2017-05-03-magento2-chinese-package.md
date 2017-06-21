---
layout: post
title: Magento2 中文化
description: Magento2 是一个用 PHP 语言开发的开源电商平台，本文旨在介绍 Magento2 国际化原理以及中文包制作流程
key: TCP, HTTP
---

### 安装语言包
**Composer安装**
```
cd <magento2 path>
composer require benheart/magento2_zh_hans_cn:dev-master
php bin/magento cache:clean && php bin/magento setup:static-content:deploy zh_Hans_CN
```
**手动安装**
- [下载 Magento2 中文包](https://github.com/benheart/magento2_zh_hans_cn/archive/master.zip)
- 解压并上传文件到指定目录：\<magento2 path\>/app/i18n/benheart/magento2_zh_hans_cn
- 在Magento2根目录执行命令：
```
php bin/magento cache:clean && php bin/magento setup:static-content:deploy zh_Hans_CN
```
- 登录Magento2管理后台，选择中文语言包：Stores -> Configuration -> General > General -> Locale options -> Chinese (China)

### 参考链接
- Magento2 翻译文档:  
[http://devdocs.magento.com/guides/v2.1/frontend-dev-guide/translations/xlate.html](http://devdocs.magento.com/guides/v2.1/frontend-dev-guide/translations/xlate.html){:target="_blank"}
- Magento2 中文包项目地址:  
[https://github.com/benheart/magento2_zh_hans_cn](https://github.com/benheart/magento2_zh_hans_cn){:target="_blank"}