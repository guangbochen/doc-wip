---
title: 创建自定义应用商店应用
---

Charts现在支持[questions.yml](https://github.com/rancher/integration-test-charts/blob/master/charts/chartmuseum/v1.6.0/questions.yml)文件中的字段`rancher_min_version`和`rancher_max_version`以指定Charts兼容的Rancher版本。 
使用用户界面时，将仅显示对运行的Rancher版本有效的应用程序版本。 已完成API验证，以确保无法启动不满足Rancher要求的应用程序。 如果较新的Rancher版本不符合应用程序的要求，则已经运行的应用程序不会受到Rancher升级的影响。

### Charts类型

Rancher支持两种不同类型的Charts：

* **Helm Chart**

      原生Helm Charts包括一个应用程序以及运行它所需的其他软件。部署本地Helm Charts时，您将学习Charts的参数，然后使用**Answers**(它们是键值对的集合)进行配置。

      Helm Stable和Helm Incubators均为本地添加的原生Helm Charts。但是，您也可以在`定制`目录中添加本地Helm Charts(尽管我们建议使用Rancher Charts)。

* **Rancher Charts**

      尽管Rancher Charts添加了两个文件`app-readme.md`和`questions.yaml`来增强用户体验，但它们与原生Helm Chart的使用方式仍旧相同。在[Rancher Chart附加文件](#Rancher-Chart附加文件)中了解有关它们的更多信息。

      Rancher Charts的优点包括：

      - **增强的修订跟踪**

      虽然Helm支持版本化的部署，但Rancher添加了跟踪和修订历史记录，以显示Charts的不同版本之间的更改。

      - **简化的应用程序启动**

      Rancher Charts添加了简化的Charts说明和配置表单，以简化目录应用程序的部署。 Rancher用户无需阅读Helm变量的整个列表即可了解如何启动应用程序。

      - **应用程序资源管理**

      Rancher跟踪由特定应用程序创建的所有资源。用户可以轻松地浏览到页面上并进行故障排除，该页面列出了用于此应用程序的所有工作负载对象。

### Chart目录结构

下表演示了Chart的目录结构，可以在Chart目录中找到：`charts/<APPLICATION>/<APP_VERSION>/`。在为自定义目录定制Chart时，此信息很有用。带有**Rancher Specific**的文件特定于Rancher Chart，但对于Chart自定义是可选的。

``` 
charts/<APPLICATION>/<APP_VERSION>/
| --charts /            # 包含依赖性Chart的目录。
| --templates/          # 包含模板的目录，当与values.yml结合使用时，将生成Kubernetes YAML。
| --app-readme.md       # 文本显示在Rancher UI的Chart标题中。*
| --Chart.yml           # 必需的HelmChart信息文件。
| --questions.yml       # 形成在Rancher UI中显示的问题。问题显示在配置选项中。*
| --README.md           # 可选：在Rancher UI中显示的Helm自述文件。该文本显示在“详细描述”中。
| --requirements.yml    # 可选：YAML文件列出了Chart的依赖性。
| --values.yml          # Chart的默认配置值。
```

### Rancher-Chart附加文件

在创建自己的自定义目录之前，您应该对Rancher Chart与本地Helm Chart的区别有基本的了解。 Rancher Chart的目录结构与Helm Chart略有不同。 Rancher Chart包含了两个Helm Chart不包含的文件。

*`app-readme.md`

   在Chart的UI标题中提供描述性文本的文件。 下图显示了Rancher Chart(包括`app-readme.md`)和原生Helm Chart(不包括)之间的差异。

       <figcaption>带有<code>app-readme.md</code>的Rancher chart(左)与没有(右侧)的Helm Chart</figcaption>
       

![app-readme.md](/img/rancher/app-readme.png)

* `questions.yml` 

  包含Chart问题的文件。 这些形式问题简化了Chart的部署。 没有它，您必须使用键值对配置部署，这更加困难。 下图显示了Rancher Chart(包括`questions.yml`)和原生Helm Chart(不包括)之间的差异。

       <figcaption>带有<code>questions.yml</code>的Rancher chart(左)与没有(右侧)的Helm Chart</figcaption>
    

![questions.yml](/img/rancher/questions.png)

#### Questions.yml

在`questions.yml`中，大多数内容都围绕着用户关心的应用配置的问题，但是也可以在此文件中设置一些其他字段。

##### 最小/最大Rancher版本

_自v2.3.0起可用_

对于每个chart，您可以添加最小和/或最大Rancher版本，该版本确定是否可以从Rancher部署此chart。

> **注意：** 即使Rancher发行版的前缀为`v`，使用该选项时发行版的前缀也为_空_。

``` 
rancher_min_version: 2.3.0
rancher_max_version: 2.3.99
```

##### 问题变量参考

以下参考可在`questions.yml`文件中嵌套的`questions:`部分使用。

| 变量                 | 类型          | 必填      | 描述
| ------------------- | ------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| variable            | string        | true     | 定义`values.yml`文件中指定的变量名，对嵌套对象使用`foo.bar`.|
| label               | string        | true     | 定义UI标签.|
| description         | string        | false    | 指定变量的描述.|
| type                | string        | false    | 如果未指定，则默认为`string`(当前支持的类型为string，multiline，boolean，int，enum，password，storageclass，hostname，pvc和secret.|
| required            | bool          | false    | 定义变量是否为必填的(true \| false)                                                                                                    |
| default             | string        | false    | 指定默认值.|
| group               | string        | false    | 根据输入值对问题进行分组.|
| min_length          | int           | false    | 最小字符长度.|
| max_length          | int           | false    | 最大字符长度.|
| min                 | int           | false    | 最小整数长度.|
| max                 | int           | false    | 最大整数长度.|
| options             | []string      | false    | 当变量类型为`enum`时指定选项，例如：options:<br /> - "ClusterIP" <br /> - "NodePort" <br /> - "LoadBalancer"                  |
| valid_chars         | string        | false    | 用于输入字符验证的正则表达式.|
| invalid_chars       | string        | false    | 用于无效输入字符验证的正则表达式.|
| subquestions        | []subquestion | false    | 添加一个子问题数组.|
| show_if             | string        | false    | 如果条件变量为true，则显示当前变量。例如`show_if: "serviceType=Nodeport"` |
| show_subquestion_if | string        | false    | 显示子问题是否为真或等于选项之一。例如`show_subquestion_if: "true"` |

> **注意：** `subquestions[]` 不能包含subquestions或show_subquestions_if键，但是支持上表中的所有其他键

### Example Custom Chart Creation

您可以使用Helm Charts或Rancher Charts填充自定义目录，尽管我们建议使用Rancher Charts，因为它们具有增强的用户体验。

> **注意：** 有关开发chart的完整步骤，请参阅Helm chart[开发人员参考](https://helm.sh/docs/chart_template_guide/)。

1. 在您用作自定义目录的GitHub存储库中，创建一个目录结构，该目录结构参考[Chart目录结构](#Chart目录结构)中列出的结构。

   Rancher需要此目录结构，尽管`app-readme.md`和`questions.yml`是可选的。

   > **提示：**
   >
   > - 要开始自定义chart，请从[Rancher Library](https://github.com/rancher/charts)或[Helm Stable](https://github.com/kubernetes/charts/tree)复制一个/master/stable)。
   > - 有关开发chart的完整介绍，请参见上游Helm chart[开发人员参考](https://docs.helm.sh/developing_charts/)。

2. **推荐：** 创建一个`app-readme.md`文件。

   使用此文件可为Rancher UI中的chart标题创建自定义文本。您可以使用此文本来通知用户该chart是针对您的环境定制的，或者提供有关如何使用它的特殊说明。
   <br/>
   <br/>
   **例如**：

``` 
   $ cat ./app-readme.md

   # Wordpress ROCKS!
```

3.**推荐:** 添加一个`questions.yml`文件.

   该文件为用户创建一个表单，供用户在部署自定义chart时指定部署参数。 如果没有此文件，则用户**必须**使用键值对手动指定参数，这对用户不友好。
   <br/>
   <br/>
   下面的示例创建一个表单，提示用户输入持久卷大小和存储类。
   <br/>
   <br/>
   有关创建`questions.yml`文件时可以使用的变量列表，请参见[问题变量参考](#问题变量参考)

   <pre>

       categories:
       - Blog
       - CMS
       questions:
       - variable: persistence.enabled
       default: "false"
       description: "Enable persistent volume for WordPress"
       type: boolean
       required: true
       label: WordPress Persistent Volume Enabled
       show_subquestion_if: true
       group: "WordPress Settings"
       subquestions:
       - variable: persistence.size
           default: "10Gi"
           description: "WordPress Persistent Volume Size"
           type: string
           label: WordPress Volume Size
       - variable: persistence.storageClass
           default: ""
           description: "If undefined or null, uses the default StorageClass. Default to null"
           type: storageclass
           label: Default StorageClass for WordPress

   </pre>

4. 将自定义的chart添加到GitHub存储库中。

**结果：** 您的自定义chart已添加到仓库中。 您的Rancher Server将在几分钟内复制chart。
