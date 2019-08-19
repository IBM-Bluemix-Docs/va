---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-05"

keywords: IBM Cloud Kubernetes Service, IBM Cloud Container Registry, security status of container images, image security, Vulnerability Advisor, security, registry, vulnerabilities, containers, security issues, configuration issues,

subcollection: va

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}


# 使用漏洞顾问程序管理映像安全性
{: #va_index}

漏洞顾问程序用于检查 {{site.data.keyword.IBM}} 或第三方提供的容器映像的安全状态，或者检查添加到组织的注册表名称空间的容器映像的安全状态。
{:shortdesc}

将映像添加到名称空间时，漏洞顾问程序会自动对该映像进行扫描，以检测安全问题和潜在漏洞。如果发现安全问题，系统会提供指示信息，以帮助修复所报告的漏洞。

漏洞顾问程序为 [{{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started) 提供安全管理，可生成包含建议修复和最佳实践的安全状态报告。

漏洞顾问程序发现的任何问题都会生成判定，指示不建议部署此映像。如果选择部署该映像，那么基于该映像部署的任何容器包含已知问题，可能被用于攻击或以其他方式破坏容器。该判定根据您指定的任何豁免进行调整。Container Image Security Enforcement 可以使用此判定来阻止在 {{site.data.keyword.containerlong_notm}} 中部署非安全映像。

修复漏洞顾问程序所报告的安全和配置问题可帮助您确保 {{site.data.keyword.cloud_notm}} 基础架构的安全。


## 关于漏洞顾问程序
{: #about}

漏洞顾问程序提供的功能可帮助您保护映像。
{:shortdesc}

提供了以下功能：

- 扫描映像以查看是否存在问题
- 提供基于安全实践（特定于 {{site.data.keyword.containerlong_notm}}）的评估报告
- 提供建议来确保一部分应用程序类型的配置文件安全
- 提供有关如何修复其报告中所报告的[有漏洞包](#packages)或[配置问题](#app_configurations)的指示信息
- 针对 [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce) 提供判定
- 对帐户、名称空间、存储库或标记级别的报告应用豁免，以标记所标记问题不适用于您的用例

在“注册表”仪表板中，**策略状态**列显示存储库的状态。链接的报告可识别映像的良好云安全实践。

漏洞顾问程序仪表板提供映像安全性的概述和评估。如果要了解漏洞顾问程序仪表板的更多信息，请参阅[复查漏洞报告](#va_reviewing)。

### 数据保护
{: #about_data_protection}

为了扫描帐户中的映像和容器以查找安全问题，漏洞顾问程序会收集、存储和处理以下信息：

- 自由格式字段，包括标识、描述和映像名称（注册表、名称空间、存储库名称和映像标记）
- 有关配置文件的文件方式和创建时间戳记的元数据
- 映像和容器中系统和应用程序配置文件的内容
- 已安装的包和库（包括其版本）

不要将个人信息放入漏洞顾问程序处理的任何字段或位置中，如先前列表中所述。

在数据中心级别聚集的扫描结果经处理后可生成匿名度量值，从而运营和改进服务。

扫描结果将在生成后 30 天删除。

## 漏洞类型
{: #types}

### 有漏洞的包
{: #packages}

漏洞顾问程序会在使用受支持操作系统的映像中检查是否存在有漏洞的包，如有，将提供任何漏洞相关安全通知的链接。
{:shortdesc}

扫描结果中会显示包含已知漏洞问题的包。对于下表中所列的 Docker 映像类型，将使用已发布的安全通知，每天更新可能的漏洞。通常，要使有漏洞的包能够通过扫描，需要该包的更高版本，其中包含对该漏洞的修订。相同的包可能列出多个漏洞，在此情况下，对包进行一次更新可解决多个漏洞。

  |Docker 基本映像|安全通知的源|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://git.alpinelinux.org/) 和 [CIRCL CVE Search ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cve.circl.lu/)。|
  |CentOS| [CentOS announce archives ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://lists.centos.org/pipermail/centos-announce/) 和 [CentOS CR announce archives ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://lists.centos.org/pipermail/centos-cr-announce/)。有关漏洞的更多信息，请参阅 [CentOS 包中的漏洞](#va_centos)。|
  |Debian|[Debian security announcements ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://lists.debian.org/debian-security-announce/)。|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Security Data API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://access.redhat.com/labsinfo/securitydataapi)。|
  |Ubuntu|[Ubuntu Security Notices ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://usn.ubuntu.com/)。|
  {: caption="表 1. 支持漏洞顾问程序检查其是否存在有漏洞包的 Docker 基本映像" caption-side="top"}

### 配置问题
{: #app_configurations}

配置问题是指与如何设置应用程序相关的潜在安全问题。通过更新 Dockerfile，可以修复许多报告的问题。
{:shortdesc}

仅当映像使用漏洞顾问程序支持的操作系统时，才会扫描这些映像。漏洞顾问程序针对以下类型的应用程序检查配置设置：

- MySQL
- NGINX
- Apache

## 复查漏洞报告
{: #va_reviewing}

部署映像之前，可复查其漏洞顾问程序报告，以获取有关任何有漏洞包和不安全容器或应用程序设置的详细信息。您还可以检查映像是否符合组织策略。
{:shortdesc}

如果未解决任何已发现的问题，那么这些问题可能会影响使用此映像所构建容器的安全性。如果未部署 Container Image Security Enforcement，您仍可以继续在容器中使用具有安全和配置问题的映像。如果部署 Container Image Security Enforcement 并且针对映像激活，那么策略必须豁免所发现的所有问题才能从此映像部署容器。

要在 Container Image Security Enforcement 中配置实施漏洞顾问程序的范围，请参阅[定制策略](/docs/services/Registry?topic=registry-security_enforce#customize_policies)。
{:tip}

如果映像不满足组织策略所设置的需求，那么必须配置映像以满足这些需求，然后才能进行部署。有关如何查看和更改组织策略的更多信息，请参阅[设置组织豁免策略](#va_managing_policy)。
{:tip}

### 使用 GUI 复查漏洞报告
{: #va_reviewing_gui}

您可以使用 GUI 复查存储在 {{site.data.keyword.registrylong_notm}} 中名称空间内的 Docker 映像的安全性。
{:shortdesc}

1. 登录到 {{site.data.keyword.cloud_notm}}。
2. 单击**导航菜单**图标，然后单击 **Kubernetes**。
3. 单击**注册表**，然后单击**映像**磁贴。映像列表和每个映像的安全状态将显示在**映像**表中。
4. 要查看标记为 `latest` 的映像的报告，请单击该映像所在的行。这会打开**映像详细信息**选项卡，显示该映像的数据。如果存储库中不存在 `latest` 标记，那么将使用最新的映像。
5. 如果安全状态显示任何问题，单击**按类型分组的问题**选项卡，可查找问题。这会打开**漏洞**和**配置问题**表。

   - **漏洞**表。显示了每个问题的漏洞标识、该问题的策略状态、受影响的包以及解决问题的方法。要查看有关该问题的更多信息，请展开该行。将显示问题的摘要，其中包含该问题的供应商安全通知的链接。并列出包含已知漏洞问题的包。
  
     对于[漏洞类型](#types)中所列的 Docker 映像类型，将使用已发布的安全通知，每天更新列表。通常，要使有漏洞的包能够通过扫描，需要该包的更高版本，其中包含对该漏洞的修订。相同的包可能列出多个漏洞，在此情况下，对包进行一次更新可更正多个问题。单击安全通知代码，以查看包的更多信息以及更新包的步骤。

   - **配置问题**表。显示了每个问题的配置问题标识、该问题的策略状态以及安全实践。要查看有关该问题的更多信息，请展开该行。将显示问题的摘要，其中包含该问题的安全通知的链接。
  
     此列表包含为提高容器安全性而采取的措施的建议，以及容器任何不安全的应用程序设置。展开相应行可查看问题的解决方法。

6. 对报告中显示的每个问题完成更正操作，然后重建该映像。

### 使用 CLI 复查漏洞报告
{: #va_registry_cli}

您可以使用 CLI 复查存储在 {{site.data.keyword.registrylong_notm}} 中名称空间内的 Docker 映像的安全性。
{:shortdesc}

1. 列出 {{site.data.keyword.cloud_notm}} 帐户中的映像。这将返回所有映像的列表，而无论其存储在哪个名称空间。

   ```
    ibmcloud cr image-list
    ```
   {: pre}

2. 检查**安全状态**列中的状态。
    - `没有问题`：未发现安全问题。
    - `<X> 个问题`：发现的潜在安全问题或漏洞的数量，其中 `<X>` 是问题的数量。
    - `正在扫描`：正在扫描映像，未确定最终漏洞状态。

3. 要查看状态的详细信息，请复查漏洞顾问程序报告：

   ```
    ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
   {: pre}

   在 CLI 输出中，可以查看有关配置问题的以下信息。
      - `安全实践`：发现的漏洞的描述
      - `更正操作`：有关如何修复漏洞的详细信息

### CentOS 包中的漏洞
{: #va_centos}

如果您正在使用 CentOS，报告中可能会出现误报，也就是说，即使没有漏洞，报告也可能会报告漏洞。当 Red Hat 发布了安全通知，但该安全通知不适用于 CentOS 或修复程序尚未移植到 CentOS 时，就会出现此情况。
{:shortdesc}

如果您收到报告说您的包有漏洞，请完成以下步骤：

1. 通过单击安全通知代码来查看更新包的步骤。
2. 通过完成更新包的步骤来更新包。
3. 如果更新了包，那么结果不是误报，并且所需操作已完成。
4. 如果由于没有更新的版本可供安装而无法更新包，那么结果是误报。您可以为此安全通知添加豁免策略，请参阅[设置组织豁免策略](#va_managing_policy)。

## 设置组织豁免策略
{: #va_managing_policy}

如果想要管理 {{site.data.keyword.cloud_notm}} 组织的安全性，您可以使用策略设置以确定问题是否为豁免。您可以使用 Container Image Security Enforcement，确保在考虑策略所免除的任何问题后，只允许来自不包含任何安全问题的映像的部署。
{:shortdesc}

除非在集群中部署了 Container Image Security Enforcement，否则无论安全状态如何，都可以从任何映像部署容器。要查找如何部署 Container Image Security Enforcement，请参阅[安装强制实施安全性](/docs/services/Registry?topic=registry-security_enforce#security_enforce)。

在使用 Container Image Security Enforcement 时，漏洞顾问程序检测到的任何安全问题都将阻止从映像部署容器。要允许部署包含检测到的问题的映像，必须向策略添加豁免。

### 使用 GUI 设置组织豁免策略
{: #va_managing_policy_gui}

如果想要使用 GUI 设置策略豁免，请完成以下步骤：

1. 登录到 {{site.data.keyword.cloud_notm}}。您必须登录才能在 GUI 中查看漏洞顾问程序。
2. 单击**导航菜单**图标，然后单击 **Kubernetes**。
3. 在**漏洞顾问程序**下，单击**政策设置**。
4. 单击**创建豁免**。
5. 选择问题类型。
6. 输入问题标识。

   您可以在[漏洞报告](#va_reviewing)中找到此信息。**漏洞标识**列包含要用于 CVE 或安全声明问题的标识；**配置问题标识**列包含用于配置问题的标识。
    {: tip}

7. 选择要将豁免应用于哪个注册表名称空间、存储库和标记。
8. 单击**保存**。

您还可以通过在相关行上悬停鼠标并单击**打开和关闭选项列表**图标，编辑和除去豁免。

### 使用 CLI 设置组织豁免策略
{: #va_managing_policy_cli}

如果想要使用 CLI 设置策略豁免，您可以运行以下命令：

- 要创建安全问题豁免，请运行 [`ibmcloud cr exemption-add`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_add) 命令。
- 要列出安全问题的豁免，请运行 [`ibmcloud cr exemption-list`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_list) 命令。
- 要列出可豁免的安全问题的类型，请运行 [`ibmcloud cr exemption-types`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_types) 命令。
- 要删除安全问题的豁免，请运行 [`ibmcloud cr exemption-rm`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_rm) 命令。

有关命令的更多信息，可在运行命令时使用 `--help` 标志。
