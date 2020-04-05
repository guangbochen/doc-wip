---
title: 应用商店, Helm Chart与应用
---

Rancher提供了基于Helm的应用商店的功能，该功能使部署和管理相同的应用程序变得更加容易。

-**应用商店目录**是GitHub存储库或Helm Chart存储库，其中包含了可部署的应用程序。 应用程序打包在称为_Helm chart_的对象中。
-**Helm charts**是描述一组相关Kubernetes资源的文件的集合。 单个chart可能用于部署简单的内容（例如Mencached pod）或复杂的内容（例如带有HTTP服务器，数据库，缓存等的完整Web应用程序栈）。

Rancher改进了Helm目录和chart。 所有本地Helm chart都可以在Rancher中使用，但是Rancher添加了一些增强功能以改善用户体验。

本节涵盖以下主题：
- [前置条件](#前置条件)
- [应用商店范围](#应用商店范围)
- [启用内置的全局目录](#启用内置的全局目录)
- [添加自定义全局应用商店](#添加自定义全局应用商店)
  - [添加自定义Git存储库](#添加自定义Git存储库)
  - [添加自定义Helm存储库](#添加自定义Helm存储库)
  - [添加私人Git/Helm存储库](#添加私人Git/Helm存储库)
- [创建应用商店应用](#创建应用商店应用)
- [使用应用商店](#使用应用商店)
  - [Apps](#apps)
  - [全局DNS](#全局DNS)
  - [与Rancher的兼容性](#与Rancher的兼容性)

## 前置条件

当Rancher部署应用商店应用时，它会启动一个临时的service account账号关联到Helm服务，该账号只具有部署应用商店应用的权限。 因此，用户无法通过此账号或Helm服务获得其他资源的访问权限。

要启动目录应用程序或多集群应用程序，您至少应具有以下权限之一：

- 目标集群的[项目成员角色](/docs/admin-settings/rbac/cluster-project-roles/#project-roles)，使您能够创建，读取，更新和删除工作负载
- 目标集群的[集群所有者角色](/docs/admin-settings/rbac/cluster-project-roles/#cluster-roles)

## 应用商店范围

在Rancher中，您可以在三个不同的范围内管理目录。 全局目录在所有集群和项目之间共享。 在某些用例中，您可能不想跨不同集群甚至同一集群中的项目共享目录。 通过利用集群和项目范围的目录，您将能够为特定团队提供应用程序，而无需与所有集群和/或项目共享它们。

| 范围    | 描述                                            | Available As of |
| ------- | -----------------------------------------------| --------------- |
| Global  | 所有集群和所有项目都可以访问此目录中的Helm chart        | v2.0.0          |
| Cluster | 特定集群中的所有项目都可以访问此目录中的Helm chart      | v2.2.0          |
| Project | 该特定集群项目可以访问此目录中的Helm chart             | v2.2.0          |

## 添加自定义全局应用商店

在Rancher中，有一些默认目录打包为Rancher的一部分。 这些可以由管理员启用或禁用。

1. 从**全局**视图中，在导航栏中选择**工具>应用商店**。 在v2.2.0之前的版本中，您可以直接在导航栏中选择**应用商店**。

2. 将要使用的默认目录切换为**启用**设置。

   - **Library**

     ```

     ```

   - **Helm Stable**

     ```

     ```

   - **Helm Incubator**

     ```

     ```

**结果**：启用所选目录。 等待几分钟，让Rancher复制目录模板。 复制完成后，您可以通过从主导航栏中选择**工具->应用商店**在任何项目中查看它们。 在v2.2.0之前的版本中，您可以从主导航栏中选择**应用商店**。

## 添加自定义全局应用商店

添加目录就像添加目录名称，URL和分支名称一样简单。

#### 添加自定义Git存储库

Git URL必须是`git clone`[可以处理的URL](https://git-scm.com/docs/git-clone#_git_urls_a_id_urls_a)，并且必须以.git结尾。 分支名称必须是目录URL中的一个分支。 如果没有提供分支名称，则默认使用`master`分支。 每当您将目录添加到Rancher时，它将几乎立即可用。

#### 添加自定义Helm存储库

Helm Chart仓库是一个HTTP服务器，其中包含一个或多个打包的Chart。 可以提供YAML文件和tar文件并可以处理GET请求的任何HTTP服务器都可以用作应用商店仓库。

Helm带有用于开发人员测试的内置软件包服务器（`helm serve`）。 Helm团队已经测试了其他服务器，包括启用了网站模式的Google Cloud Storage，启用了网站模式的S3或使用[ChartMuseum](https://github.com/helm/chartmuseum)等开源项目托管自定义应用商店Chart的服务器 。

在Rancher中，您可以仅使用名称和Chart仓库的URL地址添加自定义Helm应用商店。

#### 添加私人Git/Helm存储库

_自v2.2.0起可用_

Rancher v2.2.0起，用户可以使用任一凭据（即`用户名`和`密码`）将私有Git或Helm chart存储库添加到Rancher中。 私有Git存储库还支持使用OAuth令牌进行身份验证。

[阅读有关添加私人Git/Helm目录的更多信息](/docs/catalog/custom/#private-repositories)

<!-- 可以将两种类型的目录添加到Rancher中。 有全局目录和项目目录。 在全局目录中，目录模板可用于*所有*项目。 在项目目录中，目录仅在添加了目录的项目中可用。

Rancher的[admin](/docs/admin-settings/#global-Permissions)可以在Rancher中全局添加或删除目录。

2.0需要修复：Rancher的任何[用户]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/configuration/accounts/#account-types)环境可以在**应用商店**->**管理**中的相应Rancher环境中添加或删除环境目录。
 -->

1. 从**全局**视图中，在导航栏中选择**工具>应用商店**。 在v2.2.0之前的版本中，您可以直接在导航栏中选择**应用商店**。
2. 单击**添加**。
3. 填写表格，然后单击**创建**。

**结果**：您的目录已添加到Rancher。

## 创建应用商店应用

启用内置目录或添加自己的自定义目录后，可以开始启动任何目录应用程序。

1. 从**全局**视图中，导航到要开始部署应用程序的项目。

2. 从主导航栏中，选择**工具->应用商店**。在v2.2.0之前的版本中，在主导航栏上选择**应用商店**。点击**启动**。

3. 找到您要启动的应用程序，然后单击**查看详细信息**。

4.在**配置选项**下输入**名称**。 默认情况下，该名称还用于为应用程序创建Kubernetes命名空间。

   - 如果您想更改**名称空间**，请单击**自定义**并更改名称空间的名称。
   - 如果要使用已经存在的其他名称空间，请单击**自定义**，然后单击**使用现有的名称空间**。从列表中选择一个名称空间。

5. 选择一个**模板版本**。

6.完成其余的**配置选项**。

    - 对于本地Helm chart（即Helm Stable或Helm Incubator目录中的chart），答案在答案部分作为键值对提供。
    - 键和值在**详细说明**中可用。
    - 输入答案时，必须使用[使用Helm：--set的格式和限制](https://helm.sh/docs/intro/using_helm/#the-format-and-limitations-of-set)，因为Rancher将其作为`--set`标志传递给Helm。

      例如，输入包含两个用逗号分隔的值的答案（即`abc,bcd`）时，请用双引号将这些值引起来（即`"abc,bcd"`）。

7. 查看**预览**部分中的文件。如果确认，请单击**启动**。

**结果**：您的应用程序已部署到所选的名称空间。您可以从项目的以下位置查看应用程序状态：

通过创建带有添加文件的自定义存储库，Rancher改进了Helm存储库和chart。 所有原生Helm chart都可以在Rancher中使用，但是Rancher添加了一些增强功能以改善用户体验。

## 使用应用商店

Rancher中有两种类型的目录。 了解有关每种类型的更多信息：

- [内置全局应用商店](/docs/catalog/built-in/)
- [自定义应用商店](/docs/catalog/custom/)

#### Apps

在Rancher中，应用程序是从目录中的模板部署的。 Rancher支持两种类型的应用程序：

- [多集群应用](/docs/catalog/multi-cluster-apps/)
- [在特定项目中部署的应用程序](/docs/catalog/apps)

#### 全局DNS

_自v2.2.0起可用_

当创建跨越多个Kubernetes集群的应用程序时，可以创建一个全局DNS记录以将流量路由到所有不同集群中的端点。 将需要对外部DNS服务器进行编程，以为您的应用程序分配完全限定的域名（也称为FQDN）。 Rancher将使用您提供的FQDN和应用程序运行所在的IP地址来对DNS进行编程。 Rancher将从运行您的应用程序的所有Kubernetes集群中收集端点，并对DNS进行编程。

有关如何使用此功能的更多信息，请参见[全局DNS](/docs/catalog/globaldns/).

#### 与Rancher的兼容性

chart现在支持[questions.yml](https://github.com/rancher/integration-test-charts/blob/master/charts/chartmuseum/v1.6.0/questions.yml)文件中的字段`rancher_min_version`和`rancher_max_version`以指定chart兼容的Rancher版本。 
使用用户界面时，将仅显示对运行的Rancher版本有效的应用程序版本。 已完成API验证，以确保无法启动不满足Rancher要求的应用程序。 如果较新的Rancher版本不符合应用程序的要求，则已经运行的应用程序不会受到Rancher升级的影响。
