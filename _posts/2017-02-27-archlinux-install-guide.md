---
layout: post
title: Arch Linux 安装指南
description: Arch Linux 是通用 x86-64 GNU/Linux 发行版。Arch 采用滚动升级模式，尽全力提供最新的稳定版软件，用户可以根据自己的喜好安装需要的软件并配置成符合自己理想的系统。
key: Linux, Arch Linux
---

### 简介
**Arch Linux** 是通用 x86-64 GNU/Linux 发行版。Arch 采用**滚动升级模式**，尽全力提供最新的稳定版软件。初始安装的 Arch 只是一个基本系统，随后用户可以根据自己的喜好安装需要的软件并配置成符合自己理想的系统。以下核心原则构成了我们通常所指的 Arch 之道，或者说 Arch 的哲学，或许最好的结词是 **Keep It Simple, Stupid**（对应中文为“保持简单，且一目了然”）。
- **简洁**: 避免任何不必要的添加、修改和复杂增加。它提供的软件都来自原始开发者(上游)，仅进行和发行版(下游)相关的最小修改。
- **现代**: Arch尽全力保持软件处于最新的稳定版本，只要不出现系统软件包破损，都尽量用最新版本。Arch采用滚动升级策略，安装之后可以持续升级。
- **实用**: Arch 注重实用性，避免意识形态之争。最终的设计决策都是由开发者的共识决定。开发者依赖基于事实的技术分析和讨论，避免政治因素，不会被流行观点左右。
- **以用户为中心**: 许多 Linux 发行版都试图变得更“用户友好”，Arch Linux 则一直是，永远会是“以用户为中心”。此发行版是为了满足贡献者的需求，而不是为了吸引尽可能多的用户。Arch 适用于乐于自己动手的用户，他们愿意花时间阅读文档，解决自己的问题。

### 安装准备
- 内存空间不小于 512MB 内存的x86-64兼容机
- 有效的互联网连接
- 存储空间大于 1G 的 U 盘

### 系统安装
- 镜像下载: [https://www.archlinux.org/download/](https://www.archlinux.org/download/){:target="_blank"}
- 启动盘制作: [https://wiki.archlinux.org/index.php/USB_flash_installation_media](https://wiki.archlinux.org/index.php/USB_flash_installation_media){:target="_blank"}
- 安装系统: [https://wiki.archlinux.org/index.php/Installation_guide](https://wiki.archlinux.org/index.php/Installation_guide){:target="_blank"}

### KDE 桌面安装
- KDE 桌面安装: [https://wiki.archlinux.org/index.php/KDE](https://wiki.archlinux.org/index.php/KDE){:target="_blank"}

### 常用程序
- 网络
```
pacman -S networkmanager
```

- 输入法
```
pacman -S fcitx fcitx-qt4 libidn lsb-release opencc xorg-xprop
pacman -S fcitx-sogoupinyin
```

- 通讯
  - QQ: [https://wiki.archlinux.org/index.php/Tencent_QQ_(简体中文)](https://wiki.archlinux.org/index.php/Tencent_QQ_(简体中文)){:target="_blank"}
  - WeChat: [https://github.com/geeeeeeeeek/electronic-wechat](https://github.com/geeeeeeeeek/electronic-wechat){:target="_blank"}

- 代理
```
pacman -S shadowsocks-qt5
pacman -S proxychains
```

### 常用命令
- 更新系统
```
pacman -Syu
```
- 安装程序
```
pacman -S [options] package
```
- 卸载程序
```
pacman -R [options] package
```

### 注意事项
- Arch 安装全过程需要从网络上下载多种依赖包，确保有网络连接。
- Arch 官方有大量的 Wiki 文档，当遇到问题或者新装程序均可参阅。

### 参考链接
- Arch Linux 官方介绍: [https://wiki.archlinux.org/index.php/Arch_Linux](https://wiki.archlinux.org/index.php/Arch_Linux){:target="_blank"}
- Arch Linux 官方安装文档: [https://wiki.archlinux.org/index.php/Installation_guide](https://wiki.archlinux.org/index.php/Installation_guide){:target="_blank"}
- Arch Linux 常用程序列表: [https://wiki.archlinux.org/index.php/List_of_applications](https://wiki.archlinux.org/index.php/List_of_applications){:target="_blank"}