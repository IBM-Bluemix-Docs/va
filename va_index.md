---

copyright:
  years: 2017
lastupdated: "2017-02-21"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip} 
{:download: .download}

# Managing image security with Vulnerability Advisor 
{: #va_index}

Vulnerability Advisor checks the security status of container images before deployment.
{:shortdesc}


## About Vulnerability Advisor 
{: #about}

Vulnerability Advisor provides security management for {{site.data.keyword.containerlong}}. Vulnerability Advisor generates a security status report, suggests fixes and best practices, and provides management to restrict nonsecure images from running. Fixing the security and configuration issues that are reported by Vulnerability Advisor can help you secure your {{site.data.keyword.cloud_notm}} infrastructure.
{:shortdesc}

Vulnerability Advisor includes the following features:

-   Scans images for vulnerabilities
-   Provides an evaluation report based on security standards, such as ISO 27002, [Center of Internet Security ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.cisecurity.org/), and security practices specific to {{site.data.keyword.containerlong_notm}}
-   Detects file-based malware
-   Provides recommendations to secure configuration files for a subset of application types
-   Provides instructions on how to fix a reported [vulnerabile package](#packages) or [configuration issue](#app_configurations) in its reports



In the Registry dashboard, the **SECURITY REPORT** column displays the status of your repositories.

The report identifies good cloud security practices for your images. You can access a full list of the security and configuration issues that are checked by Vulnerability Advisor.

The Vulnerability Advisor dashboard provides an overview and assessment of the security for an image. To find out more about the Vulnerability Advisor dashboard, see [Reviewing a vulnerability report ](#va_reviewing).

## Types of vulnerabilities 
{: #types}

### Vulnerable packages
{: #packages}

Vulnerability Advisor checks for vulnerable packages in images that are based on supported operating systems and provides a link to any relevant security notices about the vulnerability. 
{:shortdesc}

Packages with known vulnerability issues are displayed. The possible vulnerabilities are updated daily from published security notices for the Docker image types that are listed in the following table. Typically, for a vulnerable package to pass the scan, a later version of the package is required that includes a fix for the vulnerability. The same package might list multiple vulnerabilities, and in this case, a single package upgrade might address multiple vulnerabilities. The information in the **CORRECTIVE ACTION** column describes how to improve security.

Supported base images are described in the following table.

  |Docker base image|Source of security notices|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://git.alpinelinux.org/) and [CIRCL CVE Search ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lists.centos.org/pipermail/centos-announce/) and [CentOS CR announce archives ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ubuntu.com/usn/)|
  {: caption="Table 1. Docker base images that Vulnerability Advisor checks for vulnerable packages" caption-side="top"}
  



### Application configurations
{: #app_configurations}

Lists application settings for the image that are nonsecure. The information in the **CORRECTIVE ACTION** column describes how to improve security.
{:shortdesc}

Vulnerability Advisor checks for known vulnerabilities in configuration settings for the following types of applications:
-   MySQL
-   NGINX
-   Apache








## Reviewing a vulnerability report for your image
{: #va_reviewing}

Before you deploy an image, you can review its Vulnerability Advisor report, which gives you details about any vulnerable packages and nonsecure application settings.
{:shortdesc}

Container images are provided by IBM, third parties, or can be added by your organization.

Review potential image security and configuration issues by completing the following steps:

1.  Log in to {{site.data.keyword.Bluemix_notm}}. You must be logged in to see Vulnerability Advisor in the graphical user interface.
2.  Click **Catalog**.
3.  Under **Infrastructure**, click **Containers**. 
4.  Click the **Container Registry** tile.
5.  Expand **Vulnerability Advisor** and click **Scanned Repositories**. 
6.  To see the report for the image that is tagged `latest`, click the row for that repository. The report shows the total number of issues and whether they are vulnerable packages or configuration issues. If no `latest` tag exists in the repository, the most recent image is used.
7.  To view information about each vulnerable package, in the **Vulnerable Packages Found** table, click the link in the **VULNERABILITIES** column to open the report.
    1.  To see more information, expand the summary.
    2.  To see the operating system distributor's notice, click the link in the **OFFICIAL NOTICE** column.
8.  To view information about each configuration issue, in the **Configuration Issues Found** table, click the row for the issue. 
9.  Perform the corrective action for each issue shown in the report, and rebuild the image. Some issues in the Dockerfile can be resolved by using the code that is provided in [Resolving problems in images](#va_report).

If vulnerabilities exist and you do not fix them, those issues can impact the use of the image for a container. You can continue to use an image that has security and configuration issues in a container.

 




## Reviewing image security for Docker images that are stored in a namespace by using the CLI 
{: #va_registry_cli}

You can review the security of Docker images that are stored in your namespaces in {{site.data.keyword.registrylong_notm}} by using the CLI to find information about potential vulnerabilities.
{:shortdesc}

When you add an image to the registry, the image is automatically scanned by Vulnerability Advisor to detect security issues and potential vulnerabilities. Containers that are deployed from vulnerable images might be attacked and compromised. Images are scanned only if they are based on an operating system that is supported by Vulnerability Advisor.

If security issues are found, instructions are provided to help fix the reported vulnerability.

To check the vulnerability status of images in your {{site.data.keyword.Bluemix_notm}} account, complete the following steps.

1.  List all images in your {{site.data.keyword.Bluemix_notm}} account. The following command returns a list of all images independent of the namespace where they are stored.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Check the status in the **VULNERABILITY STATUS** column. One of the following statuses is displayed:
    -   `OK` This status means that no security issues were found.
    -   `Vulnerable` This status means that a potential security issue or vulnerability was found.
    -   `Unknown` This status is displayed while the image is being scanned until the final vulnerability status can be determined.
    -   `Unsupported OS` This status is displayed if the image is not supported to be scanned by Vulnerability Advisor.
4.  To find out more about the status, review the Vulnerability Advisor report.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    In your CLI output, you can find the list of vulnerable packages, a description of the vulnerability that was found, and a link to instructions for how to fix it.


## Resolving common problems in images 
{: #va_report}

Example fixes for common problems that are reported by Vulnerability Advisor.
{:shortdesc}

Vulnerability Advisor provides corrective actions with reported security or configuration issues. Some of the reported problems can be fixed by updating your Dockerfile. To help you resolve common problems more quickly, use the following code examples to implement the solution in your Dockerfile.

### Maximum password age, minimum password days, and minimum password length
{: #va_password}

**Problem**: You receive one, or all of the following errors:

```
Maximum password age must be set to 90 days.
```
{: screen}

```
Minimum password length must be 8.
```
{: screen}

```
Minimum days that must elapse between user-initiated password changes should be 1.
```
{: screen}

**Fix**: Set password compliance by adding the following code to your Dockerfile.

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### SSH vulnerability 
{: #ssh}

**Problem**: The following error is returned:

```
SSH server should not be installed.
```
{: screen}

**Fix**: Instead of using SSH, use `docker attach` or `docker exec` to access your container. Ensure that your Dockerfile does not contain any steps for installing an SSH Server.

