---

copyright:
  years: 2017
lastupdated: "2017-10-06"

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
{#va_index}

Vulnerability Advisor checks the security status of container images before deployment.
{:shortdesc}


## About Vulnerability Advisor 
{#about}

Vulnerability Advisor provides security management for {{site.data.keyword.containerlong}}. Vulnerability Advisor generates a security status report, suggests fixes and best practices, and provides management to restrict nonsecure images from running. Fixing the security and configuration issues that are reported by Vulnerability Advisor can help you secure your {{site.data.keyword.Bluemix_notm}} cloud computing infrastructure.
{:shortdesc}

Vulnerability Advisor includes the following features:

-   Scans images for vulnerabilities
-   Provides an evaluation report based on security standards, such as ISO 27002, as well as security practices specific to {{site.data.keyword.containerlong_notm}}
-   Detects file-based malware
-   Provides recommendations to secure configuration files for a subset of application types
-   Provides instructions on how to fix a reported vulnerability or configuration issue in its reports


**Vulnerable Packages**
   Vulnerability Advisor checks for vulnerable packages in images that are based on supported operating systems and provides a link to any relevant security notices about the vulnerability. Vulnerability Advisor updates its internal list against these security notices daily. Supported base images are described in the following table.

  |Docker base image|Source of security notices|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux](https://git.alpinelinux.org/) and [CIRCL CVE Search](https://cve.circl.lu/)|
  |CentOS|[CentOS announce archives](https://lists.centos.org/pipermail/centos-announce/) and [CentOS CR announce archives](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat RHSA announce archives](https://www.redhat.com/archives/rhsa-announce)|
  |Ubuntu|[Ubuntu Security Notices](https://www.ubuntu.com/usn/)|
  {: caption="Table. " caption-side="top"}










## Reviewing image security for Docker images that are stored in a namespace in {{site.data.keyword.registrylong_notm}} by using the CLI 
{#va_registry_cli}

You can review the security of Docker images that are stored in a namespace to find information about potential vulnerabilities.
{:shortdesc}

When you add an image to {{site.data.keyword.registrylong_notm}}, the image is automatically scanned by Vulnerability Advisor to detect security issues and potential vulnerabilities. Containers that are deployed from vulnerable images might be attacked and compromised. Images are scanned only if they are based on an operating system that is supported by Vulnerability Advisor.

Vulnerability Advisor checks for the following vulnerabilities. If security issues are found, instructions are provided to help fix the reported vulnerability.

To check the vulnerability status of images in your account, complete the following steps.

1.  List all images in your account. The following command returns a list of all images independent of the namespace where they are stored.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Check the status in the VULNERABILITY STATUS column. One of the following statuses is displayed:
    -   OK. This status means that no security issues were found.
    -   Vulnerable. This status means that a potential security issue or vulnerability was found.
    -   Unknown. This status is displayed while the image is being scanned until the final vulnerability status can be determined.
    -   Unsupported OS. This status is displayed if the image is not supported to be scanned by Vulnerability Advisor.
4.  To find out more about the status, review the Vulnerability Advisor report.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    In your CLI output, you can find the list of vulnerable packages, a description of the vulnerability that was found, and a link to instructions for how to fix it.


## Resolving problems in images 
{#va_report}

Example fixes for common problems that are reported by Vulnerability Advisor.
{:shortdesc}

Vulnerability Advisor provides corrective actions with reported security or configuration issues. Some of the reported problems can be fixed by updating your Dockerfile. To help you resolve common problems more quickly, use the following code examples to implement the solution in your Dockerfile.

### Maximum password age, minimum password days, and minimum password length

Problem: You receive one, or all of the following errors:

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

Fix: Set password compliance by adding the following code to your Dockerfile.

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### SSH vulnerability 

Problem: The following error is returned:

```
SSH server should not be installed.
```
{: screen}

Fix: Instead of using SSH, use `docker attach` or `docker exec` to access your container. Ensure that your Dockerfile does not contain any steps for installing an SSH Server.

