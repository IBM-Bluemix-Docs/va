---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-21"

keywords: security status of container images, image security, Vulnerability Advisor, security, registry, vulnerabilities, containers, security issues, configuration issues, registry, container registry, kubernetes,

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
{:term: .term}
{:external: target="_blank" .external}

# Managing image security with Vulnerability Advisor
{: #va_index}

Vulnerability Advisor checks the security status of container images that are provided by {{site.data.keyword.IBM}}, third parties, or added to your organization's registry namespace.
{:shortdesc}

Vulnerability Advisor provides security management for [{{site.data.keyword.registrylong_notm}}](/docs/Registry?topic=registry-getting-started#getting-started), generating a security status report that includes suggested fixes and best practices.

When you add an image to a namespace, the image is automatically scanned by Vulnerability Advisor to detect security issues and potential vulnerabilities. If security issues are found, instructions are provided to help fix the reported vulnerability.

Any issues that are found by Vulnerability Advisor result in a verdict that indicates that it is not advisable to deploy this image. If you choose to deploy the image, any containers that are deployed from the image include known issues that might be used to attack or otherwise compromise the container. The verdict is adjusted based on any exemptions that you specified. This verdict can be used by Container Image Security Enforcement to prevent the deployment of nonsecure images in {{site.data.keyword.containerlong}}.

Fixing the security and configuration issues that are reported by Vulnerability Advisor can help you to secure your {{site.data.keyword.cloud_notm}} infrastructure.

## About Vulnerability Advisor
{: #about}

Vulnerability Advisor provides functions to help you to secure your images.
{:shortdesc}

The following functions are available:

- Scans images for issues
- Provides an evaluation report that is based on security practices that are specific to {{site.data.keyword.containerlong_notm}}
- Provides recommendations to secure configuration files for a subset of application types
- Provides instructions about how to fix a reported [vulnerable package](#packages) or [configuration issue](#app_configurations) in its reports
- Provides verdicts to [Container Image Security Enforcement](/docs/Registry?topic=registry-security_enforce#security_enforce)
- Applies exemptions to reports at an account, namespace, repository, or tag level to mark when issues that are flagged do not apply to your use case

In the {{site.data.keyword.registrylong_notm}} dashboard, the **Policy Status** column displays the status of your repositories. The linked report identifies good cloud security practices for your images.

The Vulnerability Advisor dashboard provides an overview and assessment of the security for an image. If you want to find out more about the Vulnerability Advisor dashboard, see [Reviewing a vulnerability report](#va_reviewing).

### Data protection
{: #about_data_protection}

To scan images and containers in your account for security issues, Vulnerability Advisor collects, stores, and processes the following information:

- Free-form fields, including IDs, descriptions, and image names (registry, namespace, repository name, and image tag)
- Metadata about the file modes and creation timestamps of the configuration files
- The content of system and application configuration files in images and containers
- Installed packages and libraries (including their versions)

Do not put personal information into any field or location that Vulnerability Advisor processes, as identified in the preceding list.

Scan results, aggregated at a data center level, are processed to produce anonymized metrics to operate and improve the service.

Scan results are deleted 30 days after they are generated.

## Types of vulnerabilities
{: #types}

### Vulnerable packages
{: #packages}

Vulnerability Advisor checks for vulnerable packages in images that are using supported operating systems and provides a link to any relevant security notices about the vulnerability.
{:shortdesc}

Packages that contain known vulnerability issues are displayed in the scan results. The possible vulnerabilities are updated daily by using the published security notices for the Docker image types that are listed in the following table. Typically, for a vulnerable package to pass the scan, a later version of the package is required that includes a fix for the vulnerability. The same package can list multiple vulnerabilities, and in this case, a single package update can address multiple vulnerabilities.

  |Docker base image|Source of security notices|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux](https://git.alpinelinux.org/){: external} and [CVE](https://cve.mitre.org/data/downloads/index.html){: external}.|
  |CentOS| [CentOS announce archives](https://lists.centos.org/pipermail/centos-announce/){: external} and [CentOS CR announce archives](https://lists.centos.org/pipermail/centos-cr-announce/){: external}. For more information about vulnerabilities, see [Vulnerabilities in packages on CentOS](#va_centos).|
  |Debian|[Debian security announcements](https://lists.debian.org/debian-security-announce/){: external}.|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Security Data API](https://access.redhat.com/labsinfo/securitydataapi){: external}.|
  |Ubuntu|[Ubuntu Security Notices](https://usn.ubuntu.com/){: external}.|
  {: caption="Table 1. Supported Docker base images that Vulnerability Advisor checks for vulnerable packages" caption-side="top"}

### Configuration issues
{: #app_configurations}

Configuration issues are potential security issues that are related to how an app is set up. Many of the reported problems can be fixed by updating your Dockerfile.
{:shortdesc}

Images are scanned only if they are using an operating system that is supported by Vulnerability Advisor. Vulnerability Advisor checks the configuration settings for the following types of apps:

- MySQL
- NGINX
- Apache

## Reviewing a vulnerability report
{: #va_reviewing}

Before you deploy an image, you can review its Vulnerability Advisor report for details about any vulnerable packages and nonsecure container or app settings. You can also check whether the image is compliant with organizational policies.
{:shortdesc}

If you do not address any discovered issues, those issues can impact the security of containers that are built by using that image. If Container Image Security Enforcement is not deployed, you can continue to use an image that has security and configuration issues in a container. If Container Image Security Enforcement is deployed and active for the image, all issues that are discovered must be exempt by your policy for containers to be deployable from this image.

To configure the scope of enforcement of Vulnerability Advisor issues in Container Image Security Enforcement, see [Customizing policies](/docs/Registry?topic=registry-security_enforce#customize_policies).
{:tip}

If your image does not meet the requirements that are set by your organization's policy, you must configure the image to meet those requirements before you can deploy it. For more information about how to view and change the organization policy, see [Setting organizational exemption policies](#va_managing_policy).
{:tip}

### Reviewing a vulnerability report by using the GUI
{: #va_reviewing_gui}

You can review the security of Docker images that are stored in your namespaces in {{site.data.keyword.registrylong_notm}} by using the GUI.
{:shortdesc}

1. Log in to {{site.data.keyword.cloud_notm}}.
2. Click the **Navigation Menu** icon, and then click **Kubernetes**.
3. Click **Registry**, and then click the **Images** tile. A list of your images and the security status of each image is displayed in the **Images** table.
4. To see the report for the image that is tagged `latest`, click the row for that image. The **Image Details** tab opens showing the data for that image. If no `latest` tag exists in the repository, the most recent image is used.
5. If the security status shows any issues, to find out about the issues, click the **Issues by Type** tab. The **Vulnerabilities** and **Configuration Issues** tables open.

   - **Vulnerabilities** table. Shows the Vulnerability ID for each issue, the policy status for that issue, the affected packages and how to resolve the issue. To see more information about that issue, expand the row. A summary of that issue is displayed that contains a link to the vendor security notice for that issue. Lists packages that contain known vulnerability issues.
  
     The list is updated daily by using published security notices for the Docker image types that are listed in [Types of vulnerabilities](#types). Typically, for a vulnerable package to pass the scan, a later version of the package is required that includes a fix for the vulnerability. The same package can list multiple vulnerabilities, and in this case, a single package update can correct multiple issues. Click the security notice code to view more information about the package and for steps to update the package.

   - **Configuration Issues** table. Shows the Configuration Issue ID for each issue, the policy status for that issue, and the security practice. To see more information about that issue, expand the row. A summary of that issue is displayed that contains a link to the security notice for that issue.
  
     The list contains suggestions for actions that you can take to increase the security of the container and any application settings for the container that are nonsecure. Expand the row to view how to resolve the issue.

6. Complete the corrective action for each issue shown in the report, and rebuild the image.

### Reviewing a vulnerability report by using the CLI
{: #va_registry_cli}

You can review the security of Docker images that are stored in your namespaces in {{site.data.keyword.registrylong_notm}} by using the CLI.
{:shortdesc}

1. List the images in your {{site.data.keyword.cloud_notm}} account. A list of all images is returned, independent of the namespace where they are stored.

   ```
   ibmcloud cr image-list
   ```
   {: pre}

2. Check the status in the **SECURITY STATUS** column.
    - `No Issues` No security issues were found.
    - `<X> Issues` The number of potential security issues or vulnerabilities that are found, where `<X>` is the number of issues.
    - `Scanning` The image is being scanned and the final vulnerability status is not determined.

3. To view the details for the status, review the Vulnerability Advisor report:

   ```
   ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
   ```
   {: pre}

   In the CLI output, you can view the following information about the configuration issues.
      - `Security practice` A description of the vulnerability that was found
      - `Corrective action` Details about how to fix the vulnerability

### Vulnerabilities in packages on CentOS
{: #va_centos}

If you are using CentOS, you might get false positives in your report, that is, the report might report a vulnerability when there isn't one. This situation occurs when a security notice is released by Red Hat but the security notice is not applicable to, or the fix has not yet been ported to, CentOS.
{:shortdesc}

If you receive a report that says your package has vulnerabilities, complete the following steps:

1. View the steps for updating the package by clicking the security notice code.
2. Update the package by completing the steps to update the package.
3. If the package is updated, the result was not a false positive and the required action is complete.
4. If the package does not update because no newer versions are available to install, the result was a false positive. You can add an exemption policy for this security notice, see [Setting organizational exemption policies](#va_managing_policy).

## Setting organizational exemption policies
{: #va_managing_policy}

If you want to manage the security of an {{site.data.keyword.cloud_notm}} organization, you can use your policy setting to determine whether an issue is exempt or not. You can use Container Image Security Enforcement to ensure that deployment is allowed only from images that contain no security issues after accounting for any issues that are exempted by your policy.
{:shortdesc}

You can deploy containers from any image regardless of security status unless Container Image Security Enforcement is deployed in your cluster. To find out how to deploy Container Image Security Enforcement, see [Installing security enforcement](/docs/Registry?topic=registry-security_enforce#security_enforce).

When you use Container Image Security Enforcement, any security issue that is detected by Vulnerability Advisor prevents a container from being deployed from the image. To allow an image with detected issues to be deployed, exemptions must be added to your policy.

### Setting organizational exemption policies by using the GUI
{: #va_managing_policy_gui}

If you want to set exemptions to the policy by using the GUI, complete the following steps:

1. Log in to {{site.data.keyword.cloud_notm}}. You must be logged in to see Vulnerability Advisor in the GUI.
2. Click the **Navigation Menu** icon, then click **Kubernetes**.
3. Click **Registry**, then click the **Settings** icon.
4. Click **Create exemption**.
5. Select the issue type.
6. Enter the issue ID.

   You can find this information in your [vulnerability report](#va_reviewing). The **Vulnerability ID** column contains the ID to use for CVE or security notice issues; the **Configuration Issue ID** column contains the ID to use for configuration issues.
   {: tip}

7. Select the registry namespace, repository, and tag that you want the exemption to apply to.
8. Click **Save**.

You can also edit and remove exemptions by hovering over the relevant row and clicking the **open and close list of options** icon.

### Setting organizational exemption policies by using the CLI
{: #va_managing_policy_cli}

If you want to set exemptions to the policy by using the CLI, you can run the following commands:

- To create an exemption for a security issue, run the [`ibmcloud cr exemption-add`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_add) command.
- To list your exemptions for security issues, run the [`ibmcloud cr exemption-list`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_list) command.
- To list the types of security issues that you can exempt, run the [`ibmcloud cr exemption-types`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_types) command.
- To delete an exemption for a security issue, run the [`ibmcloud cr exemption-rm`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_rm) command.

For more information about the commands, you can use the `--help` flag when you run the command.
