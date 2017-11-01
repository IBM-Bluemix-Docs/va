---

copyright:
  years: 2017
lastupdated: "2017-11-01"

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
-   Provides an evaluation report based on security standards, such as ISO 27002, as well as security practices specific to {{site.data.keyword.containerlong_notm}}



-   Detects file-based malware
-   Provides recommendations to secure configuration files for a subset of application types
-   Provides instructions on how to fix a reported vulnerability or configuration issue in its reports
   

    


<dl>
  <dt><strong>Vulnerable Packages</strong></dt>
  <dd>Vulnerability Advisor checks for vulnerable packages in images that are based on supported operating systems and provides a link to any relevant security notices about the vulnerability. Vulnerability Advisor updates its internal list against these security notices daily. Supported base images are described in the following table.</dd>
</dl>

  |Docker base image|Source of security notices|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://git.alpinelinux.org/) and [CIRCL CVE Search ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lists.centos.org/pipermail/centos-announce/) and [CentOS CR announce archives ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ubuntu.com/usn/)|
  {: caption="Table 1. Docker base images that Vulnerability Advisor checks for vulnerable packages" caption-side="top"}

**Recommendations**

   In the **Vulnerability Assessment** report, security recommendations are displayed in **Container Settings** for container settings, and **Application Configurations** for application configuration settings.

    Vulnerability Advisor checks for known vulnerabilities in configuration settings for the following types of application:

    Applications
    -   MySQL
    -   NGINX
    -   Apache

    The recommendations that are made by Vulnerability Advisor are never blocked by organizational policies because they might not always be required in your architecture but represent good cloud security practices for {{site.data.keyword.containerlong_notm}}.

The Vulnerability Advisor dashboard provides an overview and assessment of the security for an image or running container. You can also access a full list of the security and configuration issues that are checked by Vulnerability Advisor. Access the Vulnerability Advisor dashboard in the following ways:

-   Select an image from the {{site.data.keyword.Bluemix_notm}} catalog when you are logged in to {{site.data.keyword.Bluemix_notm}}, and click **View report**.
-   In your {{site.data.keyword.Bluemix_notm}} dashboard, select a running container. In the Vulnerability Advisor pane, click **View report**.






## Reviewing an image VA report (OLD UI)
{: #va_reviewing}

Before you deploy an image, you can review its Vulnerability Advisor report, which details any vulnerable packages, nonsecure container or application settings, and whether the image is compliant with organizational policies.
{:shortdesc}

Container images are provided by IBM, third parties, or can be added by your organization. If your image does not meet the requirements that are set by your organization's policy, you must configure the image to meet those requirements before you can deploy it. Only the organization manager can change the policy to allow a vulnerable image to be deployed. For steps to view and change the organization policy, see [Reviewing organizational policies](#va_managing_policy).

Review potential image security and configuration issues by completing the following steps:

1.  Log in to {{site.data.keyword.Bluemix_notm}}. You must be logged in to see Vulnerability Advisor in the graphical user interface.
2.  In the **Vulnerability Assessment** section, review the status of your vulnerability assessment. The status is displayed as one of the following conditions:

    -   Safe to Deploy - No security issues were found.
    -   Deploy with Caution - Security or configuration issues were found and your organizational policy permits deployment of vulnerable images. If you attempt to deploy this image, you see a warning.
    -   Deployment Blocked - Security or configuration issues were found and must be addressed before the image can be deployed because your organizational policy does not permit deployment of vulnerable images.
    -   Incomplete - The scan is not complete. The scan might still be running or the image’s operating system might not be compatible. Wait and try the scan again. If the scan still does not complete, push the image again to start a new scan. Images with incomplete scans are not blocked for deployment.
    Review the detailed report that forms the Vulnerability Advisor security assessment:

3.  Select the image and in the **Vulnerability Assessment** section, click **View report**.
4.  Review the sections to see the potential security and configuration issues for each package in the image:

    -   **Organizational Policies**

        Lists whether the image adheres to the policy that was set by the organization manager.

    -   **Risk Analysis**

        If the image has one or more vulnerabilities, this section displays the risk rating of the discovered vulnerabilities as calculated by Vulnerability Advisor. The risk is calculated by using the [CVSS ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.first.org/cvss) calculator, and [IBM X-Force® Exchange ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://exchange.xforce.ibmcloud.com/) technology.

    -   **Vulnerable Packages**

        Lists packages with known vulnerability issues, which are updated daily from published security notices for the Docker image types that are listed in [Managing image security with Vulnerability Advisor](va_index.html). Typically, for a vulnerable package to pass the scan, a later version of the package is required that includes a fix for the vulnerability. The same package might list multiple vulnerabilities, and in this case, a single package upgrade might correct multiple issues. Click the security notice code to review more information on the package and for steps to update the package.

    -   **Container Settings**

        Lists suggestions that you can take to increase the security of the image. The **Corrective Action** column describes how to improve security.

    -   **Application Configurations**

        Lists application settings for the image that are nonsecure. The **Corrective Action** column describes how to improve security.

    -   **Container Instances**

        Lists containers that use this image that are already running in your organization.

    Corrective actions or suggestions are provided for each item that is listed.

5.  Fix the problems that are described in the **Vulnerability Assessment** report, and rebuild the image. Some issues in the Dockerfile can be resolved by using the code that is provided in [Resolving problems in images](#va_report).

If vulnerabilities exist and you do not fix them, those issues can impact the usage of the image for a container. You can continue to use an image that has security and configuration issues in a container, unless a specific type of issue is blocked by your organization. Review the deployment settings for your organization to see which problems are blocked, as described in [Reviewing organizational policies](#va_managing_policy).

After you deploy your image, Vulnerability Advisor continues to scan for security and configuration issues in the container. You can resolve any problems that are found by following the steps that are described in [Reviewing a container report](#va_reviewing_container).





## Reviewing image security for Docker images that are stored in a namespace in {{site.data.keyword.registrylong_notm}} by using the CLI 
{: #va_registry_cli}

You can review the security of Docker images that are stored in a namespace to find information about potential vulnerabilities.
{:shortdesc}

When you add an image to {{site.data.keyword.registrylong_notm}}, the image is automatically scanned by Vulnerability Advisor to detect security issues and potential vulnerabilities. Containers that are deployed from vulnerable images might be attacked and compromised. Images are scanned only if they are based on an operating system that is supported by Vulnerability Advisor.

Vulnerability Advisor checks for the following vulnerabilities. If security issues are found, instructions are provided to help fix the reported vulnerability.

To check the vulnerability status of images in your {{site.data.keyword.Bluemix_notm}} account, complete the following steps.

1.  List all images in your {{site.data.keyword.Bluemix_notm}} account. The following command returns a list of all images independent of the namespace where they are stored.

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
{: #va_report}

Example fixes for common problems that are reported by Vulnerability Advisor.
{:shortdesc}

Vulnerability Advisor provides corrective actions with reported security or configuration issues. Some of the reported problems can be fixed by updating your Dockerfile. To help you resolve common problems more quickly, use the following code examples to implement the solution in your Dockerfile.

### Maximum password age, minimum password days, and minimum password length
{: #va_password}

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
{: #ssh}

Problem: The following error is returned:

```
SSH server should not be installed.
```
{: screen}

Fix: Instead of using SSH, use `docker attach` or `docker exec` to access your container. Ensure that your Dockerfile does not contain any steps for installing an SSH Server.

