---
layout: post
title: Magento2 搭建流程
description: Magento2 是一个用 PHP 语言开发的开源电商平台，本文简单介绍下 Magento2 本地搭建流程
key: TCP, HTTP
---

### 系统要求
- 操作系统: Linux x86-64, 如: RedHat Enterprise Linux (RHEL), CentOS, Ubuntu, Debian等
- 内存: 2GB 以上的运行内存(RAM)
- Web Servers:
  - Apache 2.2 or 2.4
  - nginx 1.8 (or [latest mainline version](http://nginx.org/en/linux_packages.html#mainline){:target="_blank"})
- Database
  - MySQL 5.6
  - Magento \>= 2.1.2 兼容 MySQL 5.7
  - Magento 兼容 MariaDB 和 Percona 数据库
- PHP
<table width="100%">
	<tbody>
		<tr>
			<th width="20%">7.1.x</th>
			<th width="20%">7.0.6–7.0.x</th>
			<th width="20%">7.0.5</th>
			<th width="20%">7.0.4</th>
			<th width="20%">7.0.3</th>
		</tr>
		<tr>
			<td>×</td>
			<td>√</td>
			<td>×</td>
			<td>√</td>
			<td>×</td>
		</tr>
		<tr>
            <th width="20%">7.0.2</th>
            <th width="20%">7.0.0, 7.0.1</th>
            <th width="20%">5.6.5–5.6.x</th>
            <th width="20%">5.6.0–5.6.4</th>
            <th width="20%">5.5.x</th>
        </tr>
        <tr>
            <td>√</td>
            <td>×</td>
            <td>√</td>
            <td>×</td>
            <td>×</td>
        </tr>
	</tbody>
</table>

- PHP 插件: [http://devdocs.magento.com/guides/v2.1/install-gde/system-requirements-tech.html#required-php-extensions](http://devdocs.magento.com/guides/v2.1/install-gde/system-requirements-tech.html#required-php-extensions){:target="_blank"}
- PHP OPcache(强烈推荐)

### 环境搭建
- MySQL 安装
- Nginx 安装
- PHP 及插件安装

### 获取 Magento2
- 使用Composer获取Magento2（官方推荐）
{% highlight shell %}
cd <web server docroot directory>
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition magento2
{% endhighlight %}

- 官网下载: [https://magento.com/tech-resources/download](https://magento.com/tech-resources/download){:target="_blank"}

### 修改目录属主及权限
[http://devdocs.magento.com/guides/v2.1/install-gde/install-quick-ref.html](http://devdocs.magento.com/guides/v2.1/install-gde/install-quick-ref.html){:target="_blank"}

### 安装 Magento2
官方安装文档: [http://devdocs.magento.com/guides/v2.1/install-gde/install-quick-ref.html](http://devdocs.magento.com/guides/v2.1/install-gde/install-quick-ref.html){:target="_blank"}
- Command-line安装
- Web Setup Wizard安装

网页安装带有Simple Data的Magento2时，进度经常会卡在73%，需要更改Magento2安装根目录中的nginx.conf.simple
{% highlight nginx %}
location ~ ^/setup/index.php {
    fastcgi_pass   fastcgi_backend;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
{% endhighlight %}

添加下面两行代码，增大超时的时间：
{% highlight nginx %}
location ~ ^/setup/index.php {
    fastcgi_pass   fastcgi_backend;
    fastcgi_read_timeout 600s;
    fastcgi_connect_timeout 600s;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
{% endhighlight %}

### 参考链接
- Magento2 系统要求: [http://devdocs.magento.com/guides/v2.1/install-gde/system-requirements-tech.html](http://devdocs.magento.com/guides/v2.1/install-gde/system-requirements-tech.html){:target="_blank"}
- Magento2 官方安装文档: [http://devdocs.magento.com/guides/v2.1/install-gde/install-quick-ref.html](http://devdocs.magento.com/guides/v2.1/install-gde/install-quick-ref.html){:target="_blank"}