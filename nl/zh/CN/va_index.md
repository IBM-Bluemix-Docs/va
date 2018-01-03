---

copyright:
  years: 2017
lastupdated: "2017-12-05"

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

漏洞顾问程序会在容器映像部署前检查其安全状态。
{:shortdesc}


## 关于漏洞顾问程序 
{: #about}

漏洞顾问程序为 {{site.data.keyword.containerlong}} 提供安全管理。漏洞顾问程序会生成安全状态报告、建议修订和最佳实践，并提供管理以限制非安全映像运行。修复漏洞顾问程序所报告的安全和配置问题可帮助您确保 {{site.data.keyword.cloud_notm}} 基础架构的安全。
{:shortdesc}

漏洞顾问程序包含以下功能：

-   扫描映像以查看是否存在漏洞
-   基于安全标准（如 ISO 27002）、[互联网安全中心 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.cisecurity.org/) 和特定于 {{site.data.keyword.containerlong_notm}} 的安全实践，提供评估报告
-   检测基于文件的恶意软件
-   提供建议，用于确保一部分应用程序类型的配置文件安全
-   提供如何修复报告中所报告漏洞或配置问题的指示信息
   

    


**有漏洞的包**

漏洞顾问程序会在基于受支持操作系统的映像中检查是否存在有漏洞的包，如有，将提供任何漏洞相关安全通知的链接。 

显示具有已知漏洞问题的包。对于下表中所列的 Docker 映像类型，会通过已发布的安全通知，每天更新可能的漏洞。通常，要使有漏洞的包能够通过扫描，需要该包的更高版本，其中包含对该漏洞的修订。相同的包可能列出多个漏洞，在此情况下，单一包升级即可能解决多个漏洞。**CORRECTIVE ACTION** 列中的信息描述如何提高安全性。

下表中描述了支持的基本映像：

  |Docker 基本映像|安全通知的源|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://git.alpinelinux.org/) 和 [CIRCL CVE Search ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://lists.centos.org/pipermail/centos-announce/) 和 [CentOS CR announce archives ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ubuntu.com/usn/)|
  {: caption="表 1. 漏洞顾问程序检查其是否存在有漏洞包的 Docker 基本映像" caption-side="top"}
  



**应用程序配置**

列出不安全的映像的应用程序设置。**CORRECTIVE ACTION** 列中的信息描述如何提高安全性




**安全报告**

在“注册表”仪表板中，**SECURITY REPORT** 列显示存储库的状态。

漏洞顾问程序针对以下类型的应用程序，检查配置设置中的已知漏洞：
-   MySQL
-   NGINX
-   Apache

报告可识别映像的良好云安全实践。您可以访问漏洞顾问程序所检查出的安全和配置问题的完整列表。

漏洞顾问程序仪表板提供映像安全性的概述和评估。 

要了解漏洞顾问程序仪表板的更多信息，请参阅[复查漏洞报告](#va_reviewing)。





## 复查映像的漏洞报告
{: #va_reviewing}

部署映像之前，您可以复查其漏洞顾问程序报告，该报告向您提供任何有漏洞的包和不安全的应用程序设置的详细信息。
{:shortdesc}

容器映像由 IBM、第三方提供，也可以由您的组织进行添加。

完成以下步骤，以复查潜在的映像安全和配置问题：

1.  登录到 {{site.data.keyword.Bluemix_notm}}。您必须登录才能在图形用户界面中查看漏洞顾问程序。
2.  单击**目录**。
3.  在**基础架构**下，单击**容器**。 
4.  单击**容器注册表**磁贴。
5.  展开**漏洞顾问程序**并单击**扫描的存储库**。 
6.  要查看标记为 `latest` 的映像的报告，请单击该存储库的行。报告显示问题总数以及它们是有漏洞的包还是配置问题。如果存储库中不存在 `latest` 标记，那么将使用最新的映像。
7.  要查看每个有漏洞的包的信息，请在**发现的有漏洞的包**表中，单击 **VULNERABILITIES** 列中的链接以打开报告。
    1.  要查看更多信息，请展开摘要。
    2.  要查看操作系统分发者的通知，请单击 **OFFICIAL NOTICE** 列中的链接。
8.  要查看每个配置问题的信息，请在**发现的配置问题**表中，单击问题的行。 
9.  为报告中显示的每个问题执行纠正行动并重建映像。使用[解决映像中的问题](#va_report)中提供的代码，可以解决 Dockerfile 中的一些问题。

如果存在漏洞且您未修正它们，那么这些问题可能会影响容器映像的使用。您可以继续在容器中使用具有安全和配置问题的映像。

 




## 使用 CLI 对存储在名称空间内的 Docker 映像复查映像安全性 
{: #va_registry_cli}

您可以使用 CLI 复查存储在 {{site.data.keyword.registrylong_notm}} 中名称空间内的 Docker 映像的安全性，以查找潜在漏洞的相关信息。
{:shortdesc}

将映像添加到注册表时，漏洞顾问程序会自动对该映像进行扫描，以检测安全问题和潜在漏洞。基于有漏洞的映像部署的容器可能会遭到攻击和破坏。仅当映像基于漏洞顾问程序支持的操作系统时，才会扫描这些映像。

如果发现安全问题，系统会提供指示信息，以帮助修复所报告的漏洞。

要检查 {{site.data.keyword.Bluemix_notm}} 帐户中映像的漏洞状态，请完成以下步骤。

1.  列出 {{site.data.keyword.Bluemix_notm}} 帐户中的所有映像。以下命令会返回所有映像的列表，而无论它们存储在哪个名称空间。

    ```
    bx cr image-list
    ```
    {: pre}

2.  检查 **VULNERABILITY STATUS** 列中的状态。此列显示以下其中一种状态：
    -   `OK`：此状态表示未发现安全问题。
    -   `Vulnerable`：此状态表示发现潜在安全问题或漏洞。
    -   `Unknown`：此状态在扫描映像期间显示，直到确定最终漏洞状态为止。
    -   `Unsupported OS`：如果不支持漏洞顾问程序扫描映像，会显示此状态。
4.  要了解状态的更多信息，请查看相应的漏洞顾问程序报告。

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    在 CLI 输出中，可以找到有漏洞包的列表、已发现漏洞的描述，以及指向如何修复漏洞的指示信息的链接。


## 解决映像中的常见问题 
{: #va_report}

漏洞顾问程序报告的常见问题的修复方法示例。
{:shortdesc}

漏洞顾问程序为报告的安全问题和配置问题提供了更正操作。通过更新 Dockerfile，可以修复某些报告的问题。为了帮助您更快解决常见问题，请使用以下代码示例在 Dockerfile 中实现解决方案。

### 最长密码寿命、最少密码天数和最短密码长度
{: #va_password}

**问题**：您收到下列其中一个错误或全部错误：

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

### SSH 漏洞 
{: #ssh}

**问题**：返回以下错误：

```
不应安装 SSH 服务器。
```
{: screen}

**修复**：不要使用 SSH，请改为使用 `docker attach` 或 `docker exec` 来访问容器。确保 Dockerfile 中不包含安装 SSH 服务器的任何步骤。

