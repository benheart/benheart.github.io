---
layout: post
title: Magento2 中文化
description: Magento2 是一个用 PHP 语言开发的开源电商平台，本文旨在介绍 Magento2 语言包的制作以及中文包的安装流程
key: Magento2, Chinese, zh_CN, language-package
---

### 翻译目录
当以下条件满足时，Magento 才会翻译单词或短语:
- Magneto2 代码中有特定语言对应的翻译字典
- 语言注册过使用范围(前端)
  
Magento2 会按照每种语言自动把各个模块的 i18n 目录聚合成一个目录，中文简体(zh_Hans_CN)翻译字典在模块和主题中的目录形式如下:

```
<Magento_Checkout_module_dir>/i18n/zh_Hans_CN.csv
<Magento_Checkout_module_dir>/<theme>/i18n/zh_Hans_CN.csv
```

### 生成翻译目录
- 登录 Magento2 服务器，切换到拥有 Magento2 根目录写权限的用户
- 通过运行**翻译采集指令**在启用的组件中提取需要翻译的单词和短语
```
bin/magento i18n:collect-phrases [-o|--output="<csv file path and name>"] [-m|--magento] <path to directory to translate>
```
参数说明:

| 参数 | 值 | 必选项 |
| :----:| :----: | :----: |
| \<path to directory to translate\> | 需要翻译的文件路径， <br/> 如果使用 <code class="code">-m|--magento</code> 则不需要此参数 | Yes (dictionaries) <br/> No (packages). |
| -m\|--magento | 创建语言包需要此参数，如果使用此命令，<br/>会搜索包含 <code class="code">bin/magento</code> 的路径，<br/>把需要翻译的主题或模块按行添加到字典中 | No |
| -o\|--output="\<path\>" | 给生成的 .csv 文件声明一个绝对路径和文件名，<br/>大小写敏感。文件名必须与 locale 准确匹配 | No |

- 翻译单词和短语

### 语言包简介
Magento2 允许创建以下类型的语言包:
- 为模块和主题创建多个 .csv 文件
- 一个目录中包含完整的 .csv 字典
  
语言包路径中除了包含 .csv 文件，还其他包含元数据:
- composer.json: 语言包基本信息以及定义相关依赖, [Sample composer.json](https://github.com/magento/magento2/blob/2.1/app/i18n/Magento/de_DE/composer.json){:target="_blank"}
- language.xml: 语言包的配置文件, [Sample language.xml](https://github.com/magento/magento2/blob/2.1/app/i18n/Magento/de_DE/language.xml){:target="_blank"}

### 语言包制作
- 收集需要翻译的单词和短语(<code class="code">-m|--magento</code> 参数是必须的)
- 创建相应的目录和文件，语言包通常在 Magento2 的 app/i18n/ 目录下，包含以下内容:
  1. 必要的 license 文件
  2. composer.json: 基本信息与依赖
  3. registration.php: 注册语言包
  4. language.xml: 元数据文件
- 语言包示例: [https://github.com/magento/magento2/tree/2.0/app/i18n/magento/zh_hans_cn](https://github.com/magento/magento2/tree/2.0/app/i18n/magento/zh_hans_cn){:target="_blank"}

### 中文包项目地址
[https://github.com/benheart/magento2_zh_hans_cn](https://github.com/benheart/magento2_zh_hans_cn){:target="_blank"}

### 安装中文包
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
- Magento2 翻译概述:  
[http://devdocs.magento.com/guides/v2.1/frontend-dev-guide/translations/xlate.html](http://devdocs.magento.com/guides/v2.1/frontend-dev-guide/translations/xlate.html){:target="_blank"}
- Magento2 翻译目录生成:  
[http://devdocs.magento.com/guides/v2.1/frontend-dev-guide/translations/translate_practice.html](http://devdocs.magento.com/guides/v2.1/frontend-dev-guide/translations/translate_practice.html){:target="_blank"}
- Magento2 语言包生成:  
[http://devdocs.magento.com/guides/v2.1/config-guide/cli/config-cli-subcommands-i18n.html](http://devdocs.magento.com/guides/v2.1/config-guide/cli/config-cli-subcommands-i18n.html){:target="_blank"}