---
title: 添加自定义的应用商店
---

[自定义应用商店](/docs/catalog/custom/)可以被添加到Rancher[不同的范围](/docs/catalog/#catalog-scope).

### 添加全局应用商店
> **先决条件：** 为了管理内置目录或[管理全局目录](/docs/catalog/custom/adding/#adding-global-catalogs)，您需要具有以下权限之一：
>
> - [管理员全局权限](/docs/admin-settings/rbac/global-permissions/)
> - [自定义全局权限](/docs/admin-settings/rbac/global-permissions/#custom-global-permissions) 包含[Manage Catalogs](/docs/admin-settings/rbac/global-permissions/#global-permissions-reference) 权限.

1. 从**全局**界面中，在导航栏中选择**工具>应用商店**。 在v2.2.0之前的版本中，您可以直接在导航栏中选择**应用商店**。

2. 单击**添加目录**。

3. 填写表格，然后单击**创建**。

**结果**：您的自定义全局应用商店目录已添加到Rancher。 处于**Active**状态后，它已完成同步，您将能够开始部署[多集群应用](/docs/catalog/multi-cluster-apps/)或[任何项目中的应用](/docs/catalog/apps/)。

### 添加集群级别应用商店

_自v2.1.0起可用_

> **先决条件：** 为了管理集群范围的应用商店，您需要具有以下权限之一：
>
> - [管理员全局权限](/docs/admin-settings/rbac/global-permissions/)
> - [集群所有者权限](/docs/admin-settings/rbac/cluster-project-roles/＃cluster-roles)
> - [自定义集群权限](/docs/admin-settings/rbac/cluster-project-roles/＃cluster-roles)包含[Manage Cluster Catalogs](/docs/admin-settings/rbac/cluster-project-roles/＃cluster-role-reference)权限。

1. 从**全局**界面，导航到要开始添加自定义应用商店的集群。
2. 在导航栏中选择**工具>商店设置**。
3. 单击**添加应用商店**。
4. 填写表格。 默认情况下，该表格将提供选择应用商店的**范围**。 当您从**集群**范围添加应用商店时，默认为`cluster`。
5. 单击**创建**。

**结果**：您的自定义集群目录已添加到Rancher。 进入**Active**状态后，它就完成了同步，您将可以从此应用商店开始部署[该集群中的应用](/docs/catalog/apps/)。

### 添加项目级别应用商店

_自v2.1.0起可用_

> **先决条件：** 为了管理项目范围的应用商店，您需要具有以下权限之一：
>
> - [管理员全局权限](/docs/admin-settings/rbac/global-permissions/)
> - [集群所有者权限](/docs/admin-settings/rbac/cluster-project-roles/#cluster-roles)
> - [项目所有者权限](/docs/admin-settings/rbac/cluster-project-roles/#project-roles)
> - [自定义项目权限](/docs/admin-settings/rbac/cluster-project-roles/#cluster-roles)包含[Manage Project Catalogs](/docs/admin-settings/rbac/cluster-project-roles/#project-role-reference)的权限.

1. 从**全局**界面，导航到要开始添加自定义应用商店的集群。
2. 在导航栏中选择**工具>商店设置**。
3. 单击**添加应用商店**。
4. 填写表格。 默认情况下，该表格将提供选择应用商店的**范围**。 当您从**项目**范围添加应用商店时，默认为`project`。
5. 单击**创建**。

**结果**：您的自定义集群目录已添加到Rancher。 进入**Active**状态后，它就完成了同步，您将可以从此应用商店开始部署[该项目中的应用](/docs/catalog/apps/)。
