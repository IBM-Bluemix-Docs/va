---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-31"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# 使用漏洞顾问程序管理映像安全性
{: #va_index}

漏洞顾问程序用于检查 IBM 或第三方提供的容器映像的安全状态，或者检查添加到组织的注册表名称空间的容器映像的安全状态。
{:shortdesc}


将映像添加到名称空间时，漏洞顾问程序会自动对该映像进行扫描，以检测安全问题和潜在漏洞。如果发现安全问题，系统会提供指示信息，以帮助修复所报告的漏洞。您仍可以基于有漏洞的映像部署容器，但是请记住这些容器可能受到攻击或遭到破坏。


## 关于漏洞顾问程序
{: #about}

漏洞顾问程序为 {{site.data.keyword.containerlong}} 提供安全管理。漏洞顾问程序会生成安全状态报告、建议修订和最佳实践，并提供管理以限制非安全映像运行。修复漏洞顾问程序所报告的安全和配置问题可帮助您确保 {{site.data.keyword.cloud_notm}} 基础架构的安全。
{:shortdesc}

漏洞顾问程序包含以下功能：

-   扫描映像以查看是否存在漏洞
-   提供基于安全实践（特定于 {{site.data.keyword.containerlong_notm}}）的评估报告
-   提供建议，用于确保一部分应用程序类型的配置文件安全
-   提供有关如何修复其报告中所报告的[有漏洞包](#packages)或[配置问题](#app_configurations)的指示信息

在“注册表”仪表板中，**SECURITY REPORT** 列显示存储库的状态。报告可识别映像的良好云安全实践。 

漏洞顾问程序仪表板提供映像安全性的概述和评估。要了解漏洞顾问程序仪表板的更多信息，请参阅[复查漏洞报告](#va_reviewing)。
	
	
**数据保护**

为了扫描帐户中的映像和容器以查找安全问题，漏洞顾问程序会收集、存储和处理以下信息：
- 自由格式文本字段，包括标识、描述和映像名称（注册表、名称空间、存储库名称和映像标记）
- Kubernetes 元数据，包括 Kubernetes 资源的名称，例如 pod、副本集和部署名称
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

漏洞顾问程序会在基于受支持操作系统的映像中检查是否存在有漏洞的包，如有，将提供任何漏洞相关安全通知的链接。
{:shortdesc}

扫描结果中会显示具有已知漏洞问题的包。对于下表中所列的 Docker 映像类型，会通过已发布的安全通知，每天更新可能的漏洞。通常，要使有漏洞的包能够通过扫描，需要该包的更高版本，其中包含对该漏洞的修订。相同的包可能列出多个漏洞，在此情况下，单一包升级即可能解决多个漏洞。


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

仅当映像基于漏洞顾问程序支持的操作系统时，才会扫描这些映像。漏洞顾问程序针对以下类型的应用程序检查配置设置：
-   MySQL
-   NGINX
-   Apache



## 安装容器扫描程序
{: #va_install_livescan}

开始之前：

1.  登录到 {{site.data.keyword.Bluemix_notm}} CLI 客户机。如果您具有联合帐户，请使用 `--sso`。
2.  [设定 `kubectl` CLI 的目标](../../containers/cs_cli_install.html#cs_cli_configure)为要使用 Helm 图表的集群。
3.  为容器扫描程序创建服务标识和 API 密钥并为其指定名称：
    1.  通过运行以下命令创建服务标识，将 `<scanner_serviceID>` 替换为针对服务标识选择的名称。记下其 **CRN**。
    
        ```
    	bx iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  创建服务 API 密钥，其中 `<scanner_serviceID>` 是在上一步中创建的服务标识，并将 `<scanner_APIkey_name>` 替换为针对扫描程序 API 密钥选择的名称。 
    
        ```
    	bx iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	    这将返回扫描程序 API 密钥。
	
	    确保安全地存储扫描程序 API 密钥，因为日后无法对其进行检索。
	    {: tip}
	
    3.  创建用于授予 `Writer` 角色的服务策略。
    		
        ```
    	bx iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

要配置 Helm 图表，请执行以下操作：

1.  [在集群中设置 Helm](../../containers/cs_integrations.html#helm)。如果使用 RBAC 策略来授予 Helm Tiller 访问权，请确保 Tiller 角色有权访问所有名称空间，以便扫描程序可以监控所有名称空间中的容器。

2.  向 Helm 添加 IBM 图表存储库，例如 `ibm-incubator`。

    ```
    helm repo add ibm-incubator https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

3.  在本地 YAML 文件中保存容器扫描程序 Helm 图表的缺省配置设置。在 Helm 图表路径中包含图表存储库，例如 `ibm-incubator`。

    ```
    helm inspect values ibm-incubator/ibmcloud-container-scanner > config.yaml
    ```
    {: pre}

4.  编辑 `config.yaml` 文件。

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
    <caption>了解 YAML 文件组成部分</caption>
    <thead>
    <th>字段</th>
    <th>值</th>
    </thead>
    <tbody>
    <tr>
    <td><code>EmitURL</code></td>
    <td>输入漏洞顾问程序区域端点 URL。要获取该 URL，请运行 <code>bx cr info</code> 并检索 <strong>Container Registry</strong> 地址。将 <code>registry</code> 替换为 <code>va</code>。例如：<code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>替换为集群所在的 {{site.data.keyword.Bluemix_notm}} 帐户标识。要获取帐户标识，请运行 <code>bx account list</code>。</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>替换为要在其中安装容器扫描程序的 Kubernetes 集群。要列出集群标识，请运行 <code>bx cs clusters</code>。</td>
    </tr>
    <tr>
    <td><code>APIKey</code></td>
    <td>替换为先前创建的扫描程序 API 密钥。</td>
    </tr>
    </tbody></table>

5.  使用更新后的 `config.yaml` 文件将 Helm 图表安装到集群。更新的属性会存储在图表的配置映射中。将 `<myscanner>` 替换为针对 Helm 图表选择的名称。在 Helm 图表路径中包含图表存储库，例如 `ibm-incubator`。

    ```
    helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    **注**：容器扫描程序是安装到 `kube-system` 名称空间中，但是会扫描所有名称空间中的容器。

6.  检查图表部署状态。图表就绪时，输出顶部附近的 **STATUS** 字段的值为 `DEPLOYED`。

    ```
    helm status <myscanner>
    ```
    {: pre}

7.  部署图表后，验证是否使用了 `config.yaml` 文件中更新的设置。

    ```
    helm get values <myscanner>
    ```
    {: pre}


现在 IBM Container Scanner 已安装，并且该实时扫描代理程序已部署为集群中的 [DaemonSet ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)。该扫描程序虽然是部署到 `kube-system` 名称空间，但是可扫描分配给所有 Kubernetes 名称空间（例如，`default`）中 pod 的所有容器。 



## 复查漏洞报告
{: #va_reviewing}

部署映像之前，可复查其漏洞顾问程序报告，以获取有关任何有漏洞包和不安全应用程序设置的详细信息。
{:shortdesc}



1.  登录到 {{site.data.keyword.Bluemix_notm}}。
2.  单击**目录**。
3.  在**基础架构**下，单击**容器**。
4.  单击 **Container Registry** 磁贴。
5.  展开**漏洞顾问程序**并单击**扫描的存储库**。
6.  要查看标记为 `latest` 的映像的报告，请单击该存储库的行。报告显示问题总数以及它们是有漏洞的包还是配置问题。如果存储库中不存在 `latest` 标记，那么将使用最新的映像。
7.  要查看所选映像的每个有漏洞包的信息，请在**发现的有漏洞包**表中，单击**漏洞**列中的链接以打开报告。
    1.  要查看更多信息，请展开摘要。
    2.  如果提供了操作系统分发者的声明，请单击**官方声明**列中的链接。
8.  要查看每个配置问题的信息，请在**发现的配置问题**表中，单击问题的行。
9.  为报告中显示的每个问题执行纠正行动并重建映像。使用[解决映像中的问题](#va_report)中提供的代码，可以解决 Dockerfile 中的一些问题。

如果存在漏洞但并未进行修复，那么这些问题可能会影响使用此映像所构建容器的安全性。但是，您仍可以继续在容器中使用具有安全和配置问题的映像。

 



## 使用 CLI 复查漏洞报告
{: #va_registry_cli}

您可以使用 CLI 复查存储在 {{site.data.keyword.registrylong_notm}} 中名称空间内的 Docker 映像的安全性。
{:shortdesc}

1.  列出 {{site.data.keyword.Bluemix_notm}} 帐户中的映像。这将返回所有映像的列表，而无论其存储在哪个名称空间。

    ```
        bx cr image-list
    ```
    {: pre}

2.  检查 **SECURITY STATUS** 列中的状态。
    -   `No Issues`：未发现安全问题。
    -   `X Issues`：发现潜在安全问题或漏洞。
    -   `Scanning`：正在扫描映像且尚未确定最终漏洞状态。
4.  要查看状态的详细信息，请复查漏洞顾问程序报告。

    ```
        bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    在 CLI 输出中，可以查看有关配置问题的以下信息。
      - 安全做法：发现的漏洞的描述
      - 更正操作：有关如何修复漏洞的详细信息


## 解决映像中的常见问题
{: #va_report}

查看漏洞顾问程序可能报告的常见问题的示例修订。通过更新 Dockerfile，可以修复一些问题。
{:shortdesc}

### 最长密码寿命、最少密码天数和最短密码长度
{: #va_password}

**问题**：您收到下列其中一个或多个漏洞：

```
最长密码寿命必须设置为 90 天。
```
{: screen}

```
最短密码长度必须为 8。
```
{: screen}

```
用户发起的两次密码更改的间隔天数最少应为 1 天。
```
{: screen}

**修复**：将以下代码添加到 Dockerfile 以设置密码合规性。

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```
{: codeblock}

### SSH 漏洞
{: #ssh}

**问题**：返回了以下漏洞：

```
不应安装 SSH 服务器。
```
{: screen}

**修复**：不要使用 SSH，请改为使用 `docker attach` 或 `docker exec` 来访问容器。确保 Dockerfile 中不包含安装 SSH 服务器的任何步骤。
