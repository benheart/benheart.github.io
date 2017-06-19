---
layout: post
title: RBAC 权限系统设计文档
description: RBAC 是一种较新且广为使用的访问控制机制。其不同于“强制访问控制”以及“自由选定访问控制”直接赋予使用者权限，而是将权限赋予角色。
key: RBAC, Role-Based
---

### 简介
**RBAC**(Role-Based Access Control，基于角色的访问控制)，一种较新且广为使用的访问控制机制。其不同于“强制访问控制”以及“自由选定访问控制”直接赋予使用者权限，而是将权限赋予角色<sup>[1]</sup>。简而言之，每个用户拥有若干角色，每个角色拥有若干权限，构成“用户-角色-权限”的授权模型。在这种模型中，用户与角色之间，角色与权限之间，通常是多对多的关系<sup>[2]</sup>。

### 基本概念

- **用户(user)**: 通常指人(广义的概念)，还包括机器人、计算机、集群等<sup>[3]</sup>
- **角色(role)**: 一定数量的权限的集合，权限的载体，如: 公司的工作职位，网上论坛的版主<sup>[2]</sup>
- **权限(permission)**: 对一个或多个客体的访问或操作，如: 对功能模块的操作，对上传文件的删改，甚至页面上菜单、某个按钮的可见性控制等<sup>[2]</sup>

### 需求分析
- 支持角色、权限、资源的增删改查
- 支持单个用户与角色绑定
- 支持用户组与角色绑定
- 支持单个权限与角色绑定
- 支持权限组与角色绑定
- 不同职责的员工和不同类型的用户，对资源有不同的操作权限
- 具有良好的扩展性，满足各种规模的公司架构和用户量级

### 数据库 ER 图
- 用户相关表(绿色): *user, user_group*
- 角色表(紫色): *role*
- 权限相关表(棕色): *permission, operation, resource, permission_group*
- 中间表(灰色): *user_role, user_group_user, user_group_role, permission_role, permission_group_role, permission_group_permission*

<a href="/images/rbac/rbac.png" target="_blank"><img src="/images/rbac/rbac.png" class="img-center" alt="RBAC-ER" ></a>

### 权限验证
**用户有权访问或操作资源**: \{\{用户包含的所有角色\} ∪ \{用户所属组包含的所有角色\}\} ∩ \{\{权限对应的所有角色\} U \{权限所属组对应的所有角色\}\} ≠ \{空集\}

### 注意事项
- 角色、权限和资源的划分
- 角色权限映射关系
- 当用户数量非常多时，可以考虑引入用户组，简化用户授权的复杂性
- 当系统权限非常复杂时，可以考虑引入权限组，简化角色的权限分配

### 参考链接
1. 维基百科 - 基于角色的访问控制: [https://en.wikipedia.org/wiki/Role-based_access_control](https://en.wikipedia.org/wiki/Role-based_access_control){:target="_blank"}
2. CSDN 博客 - RBAC权限管理: [http://blog.csdn.net/painsonline/article/details/7183613/](http://blog.csdn.net/painsonline/article/details/7183613/){:target="_blank"}
3. Sandhu, R., Coyne, E.J., Feinstein, H.L. and Youman, C.E. (August 1996). [Role-Based Access Control Models (PDF)](http://csrc.nist.gov/rbac/sandhu96.pdf){:target="_blank"}