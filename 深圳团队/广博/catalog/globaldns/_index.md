---
title: 全局DNS
---

_自v2.2.0起可用_
 
Rancher的全局DNS功能提供了一种对外部DNS提供商进行编程的方法，以将流量路由到您的Kubernetes应用程序。 由于DNS编程支持跨不同Kubernetes集群的跨应用程序，因此在全局级别配置全局DNS。 一个应用程序可以变得高度可用，因为它允许您在不同的Kubernetes集群上运行一个应用程序。 如果您的Kubernetes集群之一发生故障，该应用程序仍将可访问。
 
> **注意：** 全局DNS仅在启用[本地集群](/docs/installation/options/chart-options/#import-local-cluster)的[Kubernetes安装](/docs/installation/k8s-install/)。

### 全球DNS提供商

在添加全局DNS记录之前，您将需要配置对外部提供商的访问。

下表列出了每个提供商首次发布的Rancher版本。

| DNS提供商                                           | 可用版本         |
| -------------------------------------------------- | --------------- |
| [AWS Route53](https://aws.amazon.com/route53/)     | v2.2.0          |
| [CloudFlare](https://www.cloudflare.com/dns/)      | v2.2.0          |
| [AliDNS](https://www.alibabacloud.com/product/dns) | v2.2.0          |

### 全局DNS记录

对于要将流量路由到的每个应用程序，您将需要创建一个全局DNS记录。此项将使用来自全球DNS提供商的标准域名（也称为FQDN）来定位应用程序。这些应用程序可以解析为单个[多集群应用程序](/docs/catalog/multi-cluster-apps/)或特定项目。您必须向入口[添加特定的注释标签](#adding-annotations-in-inresses-to-program-the-external-dns)，以将流量正确路由到应用程序。没有此注释，DNS记录的编程将无法进行。

### 全局DNS提供商/记录的权限

默认情况下，只有[全局管理员](/docs/admin-settings/rbac/global-permissions/)和全局DNS提供程序或全局DNS记录的创建者有权使用，编辑和删除它们。创建提供者或记录时，创建者可以添加其他用户，以便这些用户访问和管理他们。默认情况下，这些成员将具有`所有者`角色来管理它们。

### 为应用程序设置全局DNS

### 全局DNS记录

对于要将流量路由到的每个应用程序，您将需要创建一个全局DNS记录。 此项将使用来自全球DNS提供商的标准域名（也称为FQDN）来定位应用程序。 这些应用程序可以解析为单个[多集群应用程序](/docs/catalog/multi-cluster-apps/)或特定项目。 您必须向入口[添加特定的注释标签](#adding-annotations-in-inresses-to-program-the-external-dns)，以将流量正确路由到应用程序。 没有此注释，DNS记录的编程将无法进行。

### 全局DNS提供商/记录的权限

默认情况下，只有[全局管理员](/docs/admin-settings/rbac/global-permissions/)和全局DNS提供程序或全局DNS记录的创建者有权使用，编辑和删除它们。 创建提供者或记录时，创建者可以添加其他用户，以便这些用户访问和管理他们。 默认情况下，这些成员将具有**所有者**角色来管理它们。

### 为应用程序设置全局DNS

#### 添加全局DNS提供商

1. 在**全局**视图中，选择**工具>全局DNS服务**。
1. 要添加提供程序，请从可用的提供程序选项中进行选择，并为全局DNS提供程序配置必要的凭据和可选域。
1. （可选）添加其他用户，以便他们在创建全局DNS记录以及管理全局DNS提供程序时可以使用提供程序。

 accordion id="route53" label="Route53" 

1. 输入提供商的**名称**。
1. （可选）输入AWS Route53上托管区域的**根域**。 如果未提供此选项，则Rancher的Global DNS Provider将与AWS密钥可以访问的所有托管区域一起使用。
1. 输入AWS **访问密钥**。
1. 输入AWS **Secret Key**。
1. 在**成员访问权限**下，搜索您希望能够使用此提供程序的任何用户。 通过添加此用户，他们还将能够管理全局DNS提供程序记录。
1. 单击**创建**。
    /accordion 
    accordion id="cloudflare" label="CloudFlare" 
1. 输入提供商的**名称**。
1. 输入**Root Domain**（根域），此字段是可选的，如果未提供，则Rancher的Global DNS Provider将与密钥可以访问的所有域一起使用。
1. 输入CloudFlare **API电子邮件**。
1. 输入CloudFlare **API密钥**。
1. 在**成员访问权限**下，搜索您希望能够使用此提供程序的任何用户。 通过添加此用户，他们还将能够管理全局DNS提供程序记录。
1. 单击**创建**。
    /accordion 
    accordion id="alidns" label="AliDNS" 
1. 输入提供商的**名称**。
1. 输入**Root Domain**（根域），此字段是可选的，如果未提供，则Rancher的Global DNS Provider将与密钥可以访问的所有域一起使用。
1. 输入**访问密钥**。
1. 输入**密钥**。
1. 在“成员访问权限”下，搜索您希望能够使用此提供程序的任何用户。 通过添加此用户，他们还将能够管理全局DNS提供程序记录。
1. 单击**创建**。

>> **注意：**
>
> - 阿里云SDK使用TZ数据。 它必须存在于运行[local cluster](/docs/installation/options/chart-options/#import-local-cluster)的节点的`/usr/share/zoneinfo`路径中，并安装到外部DNS容器。 如果在节点上不可用，请按照[说明](https://www.ietf.org/timezones/tzdb-2018f/tz-link.html)进行准备。
> - 不同版本的AliDNS具有不同的允许TTL范围，其中全局DNS记录的默认TTL可能无效。 在添加AliDNS记录之前，请参阅[参考](https://www.alibabacloud.com/help/doc-detail/34338.htm)。
>    /accordion 

#### 添加全局DNS记录

1. 在**全局**视图中，选择**工具>全局DNS服务**。
1. 点击**添加DNS记录**。
1. 输入要在外部DNS上编程的**FQDN**。
1. 从列表中选择一个全局DNS**提供程序**。
1. 选择此DNS记录是用于[多集群应用程序](/docs/catalog/multi-cluster-apps/)还是用于不同[项目](/docs/k8s-in-rancher/projects-and-namespaces/)。 您将需要确保[将注释添加到任何入口](#在程序外部的dns中添加注释）。
1. 以秒为单位配置**DNS TTL**值。 默认情况下为300秒。
1. 在**成员访问**下，搜索您希望能够管理此全局DNS记录的所有用户。

### 在程序外部的dns中添加注释

为了对全局DNS记录进行编程，您需要在应用程序或目标项目的入口上添加特定的批注，并且此入口需要使用特定的**主机名**和与全局DNS的FQDN匹配的注释记录。

1. 对于要作为全局DNS记录目标的任何应用程序，请找到与该应用程序关联的入口。
1. 为了对DNS进行编程，必须满足以下要求：
    - 必须将入口路由规则设置为使用与全局DNS记录的FQDN匹配的`主机名`。
    - 入口必须具有注释（`rancher.io / globalDNS.hostname`），并且此注释的值应与全局DNS记录的FQDN相匹配。
1. 一旦您的[多集群应用程序](/docs/catalog/multi-cluster-apps/)或目标项目中的入口处于`Active`状态，FQDN将针对该入口在外部DNS上进行编程IP地址。

### 编辑全局DNS提供商

[全球管理员](/docs/admin-settings/rbac/global-permissions/)，全球DNS提供程序的创建者，以及作为`成员`添加到全球DNS提供程序的任何用户，都对该所有者具有_owner_访问权限。 任何成员都可以编辑以下字段：

- 根域
- 访问密钥和秘密密钥
- 成员

1. 在**全局**视图中，选择**工具>全局DNS提供商**。

1. 对于要编辑的全局DNS提供商，单击**垂直省略号（...）>编辑**。

### 编辑全局DNS记录

[全局管理员](/docs/admin-settings/rbac/global-permissions/)，全局DNS条目的创建者，以及作为`成员`添加到全局DNS条目的任何用户，都具有对该所有者的_owner_访问权限。任何成员都可以编辑以下字段：

- FQDN
- 全球DNS提供商
- 目标项目或多集群应用
- DNS TTL
- 成员

任何可以访问**全局DNS**记录的用户都**只**可以添加他们有权访问的目标项目。但是，用户可以删除任何目标项目，因为不会检查确认该用户是否有权访问目标项目。

放宽了权限检查以删除目标项目，以支持用户在删除目标项目之前可能已更改其权限的情况。另一个用例可能是在从全局DNS条目的目标项目中删除目标项目之前，先将其从集群中删除。

1. 在**全局**视图中，选择**工具>全局DNS记录**。

1. 对于要编辑的全局DNS条目，单击**垂直省略号（...）>编辑**。
