---
title: 内置全局应用商店
---

我们默认将一些[全局级别的应用商店](/docs/catalog/#global-catalogs)打包到了Rancher中。

### 管理内置的全局应用商店

> **先决条件：** 为了管理内置目录或[管理全局目录](/docs/catalog/custom/adding/#adding-global-catalogs)，您需要具有以下权限之一：
>
> - [管理员全局权限](/docs/admin-settings/rbac/global-permissions/)
> - [自定义全局权限](/docs/admin-settings/rbac/global-permissions/#custom-global-permissions) 包含[Manage Catalogs](/docs/admin-settings/rbac/global-permissions/#global-permissions-reference) 权限.


1. 从**全局**界面中，在导航栏中选择**工具>应用商店**。 在v2.2.0之前的版本中，您可以直接在导航栏中选择**应用商店**。

2. 将要使用的默认目录切换为**启用**状态。

   - **Library**

     ```

     ```

   - **Helm Stable**

     ```

     ```

   - **Helm Incubator**

     ```

     ```

**结果**：启用所选目录。 等待几分钟，让Rancher将这些Charts拷贝到本地。 待拷贝完成后，您可以通过从主导航栏中选择**应用程序**在任何项目中查看它们。 在v2.2.0之前的版本中，可以在项目内的主导航栏中选择**应用商店**。
