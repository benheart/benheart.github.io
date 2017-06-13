---
layout: post
title: RBAC 权限系统设计文档
description: RBAC 是一种较新且广为使用的访问控制机制。其不同于“强制访问控制”以及“自由选定访问控制”直接赋予使用者权限，而是将权限赋予角色。
key: RBAC, Role-Based
---

### 简介
**RBAC**(Role-Based Access Control，基于角色的访问控制)，一种较新且广为使用的访问控制机制。其不同于“强制访问控制”以及“自由选定访问控制”直接赋予使用者权限，而是将权限赋予角色。简而言之，每个用户拥有若干角色，每个角色拥有若干权限，构成“用户-角色-权限”的授权模型。在这种模型中，用户与角色之间，角色与权限之间，通常是多对多的关系。

### 定义

### 数据库 ER 图
<a href="/images/rbac/rbac.png" target="_blank"><img src="/images/rbac/rbac.png" class="img-center" style="width: 100%;" alt="RBAC-ER" ></a>

### 注意事项
- 角色、权限和资源的划分
- 角色权限映射关系

### 参考链接
- 维基百科 - 基于角色的访问控制: [https://en.wikipedia.org/wiki/Role-based_access_control](https://en.wikipedia.org/wiki/Role-based_access_control){:target="_blank"}
- CSDN 博客 - RBAC权限管理: [http://blog.csdn.net/painsonline/article/details/7183613/](http://blog.csdn.net/painsonline/article/details/7183613/){:target="_blank"}
- Ferraiolo, D.F. & Kuhn, D.R. (October 1992). [Role Based Access Control (PDF)](http://csrc.nist.gov/groups/SNS/rbac/documents/ferraiolo-kuhn-92.pdf){:target="_blank"}
- Sandhu, R., Coyne, E.J., Feinstein, H.L. and Youman, C.E. (August 1996). [Role-Based Access Control Models (PDF)](http://csrc.nist.gov/rbac/sandhu96.pdf){:target="_blank"}