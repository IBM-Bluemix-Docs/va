---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-06"

keywords: IBM Cloud Kubernetes Service, IBM Cloud Container Registry, security status of container images, image security, Vulnerability Advisor, security, registry, vulnerabilities, container scanner, containers, security issues, configuration issues,

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
如果在每个集群中安装了容器扫描程序，那么漏洞顾问程序也会检查正在运行的容器的状态。
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
- 扫描正在运行的容器中的问题（如果每个集群中安装了[容器扫描程序](#va_install_container_scanner)）
- 提供基于安全实践（特定于 {{site.data.keyword.containerlong_notm}}）的评估报告
- 提供建议来确保一部分应用程序类型的配置文件安全
- 提供有关如何修复其报告中所报告的[有漏洞包](#packages)或[配置问题](#app_configurations)的指示信息
- 针对 [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce) 提供判定
- 对帐户、名称空间、存储库或标记级别的报告应用豁免，以标记所标记问题不适用于您的用例
- 在 {{site.data.keyword.registrylong_notm}} 图形用户界面的**标记**视图中，提供关联容器的链接。您可以列出在安装了容器扫描程序的集群中正在运行的容器以及正在使用该映像的容器。

在“注册表”仪表板中，**策略状态**列显示存储库的状态。链接的报告可识别映像的良好云安全实践。

漏洞顾问程序仪表板提供映像安全性的概述和评估，如果安装了容器扫描程序，那么还将提供正在运行的容器的链接。如果要了解漏洞顾问程序仪表板的更多信息，请参阅[复查漏洞报告](#va_reviewing)。

**数据保护**

为了扫描帐户中的映像和容器以查找安全问题，漏洞顾问程序会收集、存储和处理以下信息：

- 自由格式字段，包括标识、描述和映像名称（注册表、名称空间、存储库名称和映像标记）
- Kubernetes 元数据，包括 Kubernetes 资源的名称，例如 pod、ReplicaSet 和部署名称
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

扫描结果中会显示包含已知漏洞问题的包。对于下表中所列的 Docker 映像类型，将使用已发布的安全通知，每天更新可能的漏洞。通常，要使有漏洞的包能够通过扫描，需要该包的更高版本，其中包含对该漏洞的修订。相同的包可能列出多个漏洞，在此情况下，对包进行一次升级可解决多个漏洞。

  |Docker 基本映像|安全通知的源|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://git.alpinelinux.org/) 和 [CIRCL CVE Search ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cve.circl.lu/)|
  |CentOS|[CentOS announce archives ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://lists.centos.org/pipermail/centos-announce/) 和 [CentOS CR announce archives ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://usn.ubuntu.com/)|
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

如果部署了容器扫描程序，那么在部署映像后，漏洞顾问程序会继续扫描容器中的安全和配置问题。您可以按照[检查容器报告](#va_reviewing_container)中描述的步骤来解决发现的任何问题。

### 使用 GUI 复查漏洞报告
{: #va_reviewing_gui}

您可以使用 GUI 复查存储在 {{site.data.keyword.registrylong_notm}} 中名称空间内的 Docker 映像的安全性。
{:shortdesc}

1. 登录到 {{site.data.keyword.Bluemix_notm}}。
2. 单击**导航菜单**图标，然后单击 **Kubernetes**。
3. 单击**注册表**，然后单击**映像**磁贴。映像列表和每个映像的安全状态将显示在**映像**表中。
4. 要查看标记为 `latest` 的映像的报告，请单击该映像所在的行。这会打开**映像详细信息**选项卡，显示该映像的数据。如果存储库中不存在 `latest` 标记，那么将使用最新的映像。
5. 如果安全状态显示任何问题，单击**按类型分组的问题**选项卡，可查找问题。这会打开**漏洞**和**配置问题**表。

   - **漏洞**：该表显示了每个问题的漏洞标识、该问题的策略状态、受影响的包以及解决问题的方法。要查看有关该问题的更多信息，请展开该行。将显示问题的摘要，其中包含该问题的供应商安全通知的链接。并列出包含已知漏洞问题的包。
  
     对于[漏洞类型](#types)中所列的 Docker 映像类型，将使用已发布的安全通知，每天更新列表。通常，要使有漏洞的包能够通过扫描，需要该包的更高版本，其中包含对该漏洞的修订。相同的包可能列出多个漏洞，在此情况下，对包进行一次升级可更正多个问题。单击安全通知代码，以查看包的更多信息以及更新包的步骤。

   - **配置问题**：该表显示了每个问题的配置问题标识、该问题的策略状态以及安全实践。要查看有关该问题的更多信息，请展开该行。将显示问题的摘要，其中包含该问题的安全通知的链接。
  
     此列表包含为提高容器安全性而采取的措施的建议，以及容器任何不安全的应用程序设置。展开相应行可查看问题的解决方法。

6. 对报告中显示的每个问题完成更正操作，然后重建该映像。

### 使用 CLI 复查漏洞报告
{: #va_registry_cli}

您可以使用 CLI 复查存储在 {{site.data.keyword.registrylong_notm}} 中名称空间内的 Docker 映像的安全性。
{:shortdesc}

1. 列出 {{site.data.keyword.Bluemix_notm}} 帐户中的映像。这将返回所有映像的列表，而无论其存储在哪个名称空间。

   ```
    ibmcloud cr image-list
    ```
   {: pre}

2. 检查**安全状态**列中的状态。
    - **没有问题**：未发现安全问题。
    - **`<X>` 个问题** 发现 `<X>` 个可能的安全问题或漏洞，其中，`<X>` 是问题的数量。
    - **正在扫描**：正在扫描映像，尚未确定最终漏洞状态。

3. 要查看状态的详细信息，请复查漏洞顾问程序报告：

   ```
    ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
   {: pre}

   在 CLI 输出中，可以查看有关配置问题的以下信息。
      - **安全实践**：发现的漏洞的描述
      - **更正操作**：有关如何修复漏洞的详细信息

## 设置组织豁免策略
{: #va_managing_policy}

如果想要管理 {{site.data.keyword.Bluemix_notm}} 组织的安全性，您可以使用策略设置以确定问题是否为豁免。您可以选择使用 Container Image Security Enforcement，确保在识别策略所免除的任何问题后只允许来自不包含任何安全问题的映像的部署。
{:shortdesc}

除非在集群中部署了 Container Image Security Enforcement，否则无论安全状态如何，都可以从任何映像部署容器。要查找如何部署 Container Image Security Enforcement，请参阅[安装强制实施安全性](/docs/services/Registry?topic=registry-security_enforce#security_enforce)。

在使用 Container Image Security Enforcement 时，漏洞顾问程序检测到的任何安全问题都将阻止从映像部署容器。要允许部署包含检测到的问题的映像，必须向策略添加豁免。

### 使用 GUI 设置组织豁免策略
{: #va_managing_policy_gui}

如果想要使用 GUI 设置策略豁免，请完成以下步骤：

1. 登录到 {{site.data.keyword.Bluemix_notm}}。您必须登录才能在 GUI 中查看漏洞顾问程序。
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

## 安装容器扫描程序
{: #va_install_container_scanner}

容器扫描程序支持漏洞顾问程序报告在运行中的容器中发现的任何问题，这些问题在容器的基本映像中不存在。如果不对容器进行运行时修改，那么将不需要运行容器扫描程序，因为映像报告将会显示相同的问题。
{:shortdesc}

要检查在集群中运行的实时容器的安全状态，可以安装容器扫描程序。为了保护您的应用程序，容器扫描程序会定期扫描正在运行的容器，以便您可以检测和纠正任何新检测到的漏洞。

您可以设置容器扫描程序，以监视分配给所有 Kubernetes 空间名称中的 pod 的容器中的漏洞。在发现漏洞时，您必须纠正映像的任何问题，然后重新部署应用程序。容器扫描程序仅支持从存储在 {{site.data.keyword.registrylong_notm}} 中的映像中创建的容器。

要使用容器扫描程序，必须设置许可权，然后设置 [Helm Chart ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.helm.sh/developing_charts) 并将其与要使用它的集群相关联。

### 为容器扫描程序设置服务许可权
{: #va_install_container_scanner_permissions}

容器扫描程序需要设置许可权，以便服务可以运行。
{:shortdesc}

要设置服务许可权，请完成以下步骤：

1. 登录到 {{site.data.keyword.Bluemix_notm}} CLI 客户机。如果您具有联合帐户，请使用 `--sso`。
2. [设定 `kubectl` CLI 的目标](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)为要使用 Helm chart 的集群。
3. 为容器扫描程序创建服务标识和 API 密钥并为其指定名称：
    1. 要创建服务标识，请运行以下命令，其中 `<scanner_serviceID>` 是针对服务标识选择的名称。记下其 **CRN**。

       ```
ibmcloud iam service-id-create <scanner_serviceID>
    	```
       {: codeblock}

    2. 创建服务 API 密钥，其中 `<scanner_serviceID>` 是在上一步中创建的服务标识，且 `<scanner_APIkey_name>` 是针对扫描程序 API 密钥选择的名称。

       ```
ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
       {: codeblock}
这将返回扫描程序 API 密钥。

       确保安全地存储扫描程序 API 密钥，因为日后无法对其进行检索。
	    还要确保安装了扫描程序的每个集群有独立的服务 API 密钥。
       {: tip}

    3. 创建用于授予 `Writer` 角色的服务策略。

       ```
       ibmcloud iam service-policy-create --resource-type scaningress --service-name container-registry --roles Writer <scanner_serviceID>
       ```
       {: codeblock}

### 配置 Helm chart
{: #va_install_container_scanner_helm}

配置 Helm chart，并将其与要使用它的集群相关联。
{:shortdesc}

要配置 Helm chart，请完成以下步骤：

1. [在 IBM Cloud Kubernetes Service 中设置 Helm](/docs/containers?topic=containers-integrations#helm)。如果使用基于角色的访问控制 (RBAC) 策略向 Tiller 授予访问权，请确保 Tiller 角色有权访问所有名称空间。向 Tiller 角色授予对所有名称空间的访问权，可确保容器扫描程序可以查看所有名称空间中的容器。

2. 向 Helm 添加 IBM 图表存储库，例如 `ibm`。

   ```
   helm repo add ibm https://icr.io/helm/ibm
   ```
   {: pre}

3. 在本地 YAML 文件中保存容器扫描程序 Helm chart 的缺省配置设置。在 Helm chart 路径中包含图表存储库，例如 `ibm`。

   ```
   helm inspect values ibm/ibmcloud-container-scanner > config.yaml
   ```
   {: pre}

4. 编辑 `config.yaml` 文件。

   ```yaml
    EmitURL: <regional_emit_URL>
    AccountID: <IBM_Cloud_account_ID>
    ClusterID: <cluster_ID>
    APIKey: <scanner_APIkey>
    ...
   ```
   {: pre}

   <table>
   <col width="22%">
   <col width="78%">
   <caption>表 2. 了解 YAML 文件的组成部分</caption>
   <thead>
   <th>字段</th>
   <th>值</th>
   </thead>
   <tbody>
   <tr>
   <td><code>EmitURL</code></td>
   <td>输入漏洞顾问程序区域端点 URL。要获取该 URL，请运行 <code>ibmcloud cr info</code> 并检索 <strong>Container Registry</strong> 地址。例如，<code>https<span comment="make the link not a link">://uk.</span>icr.io</code>。将 <code>/va</code> 添加到此地址的结尾处。例如，<code>https<span comment="make the link not a link">://uk.</span>icr.io/va</code></td>
   </tr>
   <tr>
   <td><code>AccountID</code></td>
   <td>将 <code>AccountID</code> 替换为集群所在的 {{site.data.keyword.Bluemix_notm}} 帐户标识。要获取帐户标识，请运行 <code>ibmcloud account list</code>。</td>
   </tr>
   <tr>
   <td><code>ClusterID</code></td>
   <td>将 <code>ClusterID</code> 替换为要在其中安装容器扫描程序的 Kubernetes 集群。要列出集群标识，请运行 <code>ibmcloud ks clusters</code>。<br> **提示：**使用集群的标识，而不是集群的名称。
   </td>
   </tr>
   <tr>
   <td><code>APIKey</code></td>
   <td>将 <code>APIKey</code> 替换为先前创建的扫描程序 API 密钥。</td>
   </tr>
   </tbody></table>

5. 使用更新后的 `config.yaml` 文件将 Helm chart 安装到集群。更新的属性会存储在图表的配置映射中。将 `<myscanner>` 替换为针对 Helm chart 选择的名称。在 Helm chart 路径中包含图表存储库，例如 `ibm`。

   ```
   helm install -f config.yaml --name=<myscanner> ibm/ibmcloud-container-scanner
   ```
   {: pre}

   容器扫描程序安装在 `kube-system` 名称空间中，但是会扫描所有名称空间中的容器。
    {:tip}

6. 检查图表部署状态。图表就绪时，**STATUS** 字段的值为 `DEPLOYED`。

   ```
helm status <myscanner>
    ```
   {: pre}

7. 部署图表后，验证是否使用了 `config.yaml` 文件中更新的设置。

   ```
helm get values <myscanner>
    ```
   {: pre}

现在容器扫描程序已安装，并且该代理程序已部署为集群中的 [DaemonSet ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)。容器扫描程序虽然是部署到 `kube-system` 名称空间，但是可扫描分配给所有 Kubernetes 名称空间（例如，`default`）中 pod 的所有容器。

## 从防火墙后面运行容器扫描程序
{: #va_firewall}

如果防火墙阻止出局连接，那么必须配置防火墙以允许工作程序节点访问下表中 IP 地址上的 TCP 端口 `443` 上的容器扫描程序。
{:shortdesc}

 

<p>
  <table summary=" 应该从左到右读取行，其中第一列是服务器位置，第二列是要匹配的 IP 地址。">
  <caption>表 3. 要为出局流量打开的 IP 地址</caption>
    <thead>
      <th>位置</th>
      <th>IP 地址</th>
    </thead>
    <tbody>
      <tr>
        <td>达拉斯</td>
        <td><code>169.47.103.118</code><br><code>169.48.165.6</code></td>
      </tr>
      <tr>
         <td>法兰克福</td>
         <td><code>159.8.220.182</code><br><code>158.177.74.102</code></td>
      </tr>
      <tr>
        <td>伦敦</td>
        <td><code>158.175.71.134</code><br><code>5.10.111.190</code></td>
      </tr>
      <tr>
         <td>悉尼</td>
         <td><code>168.1.40.158</code><br><code>130.198.65.182</code></td>
      </tr>
      <tr>
        <td>华盛顿</td>
         <td><code>169.60.73.158</code><br><code>169.61.84.102</code></td>
      </tr>
    </tbody>
  </table>
</p>

## 复查容器报告
{: #va_reviewing_container}

在仪表板中，可以查看容器的状态，以确定其安全性是否符合组织策略。还可以复查容器的安全报告，其中详细描述了任何有漏洞的包和不安全的容器或应用程序设置，以及容器是否符合组织策略。
{:shortdesc}

通过复查**策略状态**字段，检查正在空间中运行的容器是否依然符合组织策略。状态显示为以下某个条件：


- **符合策略**：未发现安全或配置问题。
- **不符合策略**：漏洞顾问程序发现可能的安全或配置问题，导致容器不符合策略。如果组织策略允许部署有漏洞的映像，那么可使用`小心部署`状态部署映像，并且会向部署的用户发送警告。
- **评估不完整**：扫描未完成。扫描可能仍在运行，或该容器实例的操作系统可能不兼容。

通过完成以下步骤，查看容器安全报告并对任何报告的安全或配置问题采取措施，以确保容器尽可能安全：

1. 选择要查看其报告的容器：
    1. 单击**导航菜单**图标，然后单击 **Kubernetes**。
    2. 依次单击**注册表**和**存储库**磁贴，然后展开所需存储库所在的行。
    3. 选择所需映像所在的行。
    4. 选择**关联的容器**选项卡，然后选择所需容器所在的行。这将打开安全报告。
2. 复查各部分以了解映像中每个包的潜在安全和配置问题：

    - **漏洞**：列出包含已知漏洞问题的包。对于[漏洞类型](#types)中所列的 Docker 映像类型，将使用已发布的安全通知，每天更新列表。通常，要使有漏洞的包能够通过扫描，需要该包的更高版本，其中包含对该漏洞的修订。相同的包可能列出多个漏洞，在此情况下，对包进行一次升级可更正多个问题。单击安全通知代码，以查看包的更多信息以及更新包的步骤。

    - **配置问题**：列出为提高容器安全性可采用的建议，以及容器任何不安全的应用程序设置。展开相应行可查看问题的解决方法。

   针对列出的每项都提供了更正操作或建议。

3. 复查每个安全问题的策略状态。策略状态指示此问题是否已豁免。

    - **活动**：您有未豁免的问题，该问题正在影响安全状态。
    - **豁免**：您的策略设置已豁免此问题。
    - **部分豁免**：此问题与多个安全通知相关联。并非所有安全通知都已豁免。

4. 决定如何更新容器以便您能够解决问题。

    **重要信息**：要修复容器映像问题，必须删除旧实例并重新部署，这意味着会丢失现有容器中的所有数据。请确保对容器体系结构有充分的了解，以选择相应的容器重新部署方法。

    **示例**

    - 如果容器与其计算的数据相分离，那么可以停止容器并将其删除，对映像进行必需的更改，然后重新部署，而不会丢失数据。
    - 可以使用 {{site.data.keyword.Bluemix_notm}} 服务（例如 [Delivery Pipeline](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-deliverypipeline_about#deliverypipeline_about)）来帮助更新有漏洞的容器实例。
    - 在微服务体系结构中，可在修复安全或配置问题时，将流量路由到其他容器实例，然后在红黑部署中推送新的映像。

5. 如果目前无法解决问题，那么可以在策略设置中豁免该问题，从而避免该问题阻止容器部署。要豁免问题，请单击**打开和关闭选项列表**图标，然后单击**创建豁免**，参阅[设置组织豁免策略](#va_managing_policy)。

6. 修复**安全**报告中描述的问题，然后根据您选择的方法重建映像或重新部署容器。
