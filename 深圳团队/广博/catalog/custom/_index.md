---
title: 自定义应用Custom Catalogs
---

任何用户都可以[添加自定义应用商店](/docs/catalog/custom/creating/)到Rancher中。 除了应用商店的内容外，用户还必须确保能够将商店添加到Rancher中。

### 商店仓库类型

Rancher支持不同类型的应用商店仓库:

-自定义的Github仓库
-自定义Helm Chart仓库

#### 自定义Git仓库

Git URL必须是`git clone`[可以处理的URL](https://git-scm.com/docs/git-clone#_git_urls_a_id_urls_a)，并且必须以.git结尾。 分支名称必须是目录URL中的一个分支。 如果没有提供分支名称，则默认使用`master`分支。 每当您将目录添加到Rancher时，它将几乎立即可用。

#### 自定义Helm Chart仓库

Helm Chart仓库是一个HTTP服务器，其中包含一个或多个打包的Chart。 可以提供YAML文件和tar文件并可以处理GET请求的任何HTTP服务器都可以用作应用商店仓库。

Helm带有用于开发人员测试的内置软件包服务器（`helm serve`）。 Helm团队已经测试了其他服务器，包括启用了网站模式的Google Cloud Storage，启用了网站模式的S3或使用[ChartMuseum](https://github.com/helm/chartmuseum)等开源项目托管自定义应用商店Chart的服务器 。

在Rancher中，您可以仅使用名称和Chart仓库的URL地址添加自定义Helm应用商店。

### 配置参数

当[添加应用商店](/docs/catalog/custom/adding/)到Rancher时, 用户必须提供下列信息:

| 参数                 | 描述                                                                                                                                                                       |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 名称                | 自定义名称以区分Rancher中添加的应用商店
| 商店URL地址          | 自定义商店存储库的URL
| 使用私有应用商店      | 如果使用的是需要身份验证的私有存储库，则选择
| 用户名 (可选)         | [Username](#using-username-and-password) 或 [OAuth Token](#using-an-oauth-token)                                                                                                  |
| 密码 (可选) | 如果您正在使用[用户名](#using-username-and-password)进行身份验证，则为关联的密码。如果您使用的是[OAuth Token](#using-an-oauth-token)请使用`x-oauth-basic`. |
| 分支              | Git仓库的分支名称，默认值为：`master`。对于Helm Chartc存储库，该字段将被忽略。

### 私有存储库

_自v2.2.0起可用_

可以使用任一凭据（即`用户名`和`密码`）将私有Git或Helm chart存储库添加到Rancher中。 私有Git存储库还支持使用OAuth令牌进行身份验证。

#### 使用用户名与密码

1. [添加目录](/docs/catalog/custom/adding/)时，选中**使用私有应用商店**复选框。

2. 为您的Git或Helm存储库提供`用户名`和`密码`。

#### 使用OAuth token

阅读[using Git over HTTPS and OAuth](https://github.blog/2012-09-21-easier-builds-and-deployments-using-git-over-https-and-oauth/)了解更多有关如何使用OAuth身份验证。

1.创建一个[OAuth令牌](https://github.com/settings/tokens)
  并选择`repo`权限，然后点击`生成令牌`。

2. [添加目录](/docs/catalog/custom/adding/)时，选中`使用私有应用商店`复选框。

3.对于`用户名`，提供Git生成的OAuth令牌。 对于`密码`，输入`x-oauth-basic`。
