---
layout: post
title: Phabricator 安装配置
description: Phabricator 是一套完整开源的软件开发工具集，具有任务跟踪、代码审核、仓库托管、持续集成、内部讨论等强大功能。
key: TCP, HTTP
---

### 简介
**[Phabricator](https://www.phacility.com/){:target="_blank"}** 是一套完整的软件开发工具集，最初是 Facebook 的一个内部工具，主要开发者为 Evan Priestley。Evan Priestley 离开 Facebook 后，在名为 Phacility 的新公司继续 Phabricator 的开发与维护。Phabricator 主要包含以下应用:
- 代码审核与审计 - **Differential & Audit**
- 仓库托管 - **Diffusion**
- 任务和 Bug 跟踪 - **Maniphest**
- 项目管理 - **Projects**
- 团队沟通 - **Conpherence**
- Wiki 文档管理 - **Phriction**
- 员工管理 - **People**

### 安装
#### LAMP 环境安装
Phabricator 是一个基于 Web 的工具软件，使用 PHP 语言编写，为使其正常运行，需要搭建一个 LNMP（Linux，Nginx，MySQL，PHP）的 Web Server 环境。
- **Linux(Ubuntu)**
- **Nginx**
{% highlight shell %}
sudo apt-get install nginx
{% endhighlight %}
- **MySQL**
{% highlight shell %}
sudo apt-get install mysql-server
{% endhighlight %}
- **PHP**:
{% highlight shell %}
sudo apt-get install -y php5 php5-fpm php5-mysql
{% endhighlight %}

#### Phabricator 源码下载
{% highlight shell %}
$ cd somewhere/ # pick some install directory
somewhere/ $ git clone https://github.com/phacility/libphutil.git
somewhere/ $ git clone https://github.com/phacility/arcanist.git
somewhere/ $ git clone https://github.com/phacility/phabricator.git
{% endhighlight %}

### 配置
#### 基本配置

#### 登录注册

#### 邮件发送

#### 仓库托管

### 参考链接
- Phabricator 官方安装文档: [https://secure.phabricator.com/book/phabricator/article/installation_guide/](https://secure.phabricator.com/book/phabricator/article/installation_guide/){:target="_blank"}
- Phabricator 官方配置文档: [https://secure.phabricator.com/book/phabricator/article/configuration_guide/](https://secure.phabricator.com/book/phabricator/article/configuration_guide/){:target="_blank"}