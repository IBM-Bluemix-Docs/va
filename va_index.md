---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-18"

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

Vulnerability Advisor checks the security status of container images that are provided by {{site.data.keyword.IBM}}, third parties, or added to your organization's registry namespace, and, if you've installed the container scanner in each cluster, also checks the status of running containers.
{:shortdesc}

When you add an image to a namespace, the image is automatically scanned by Vulnerability Advisor to detect security issues and potential vulnerabilities. If security issues are found, instructions are provided to help fix the reported vulnerability. 

Vulnerability Advisor provides security management for {{site.data.keyword.registrylong_notm}}, generating a security status report that includes suggested fixes and best practices. 

Any issues that are found result in a verdict that indicates that it is not advisable to deploy this image. If you choose to deploy the image, any containers that are deployed from the image have known issues that might be used to attack or otherwise compromise the container. Vulnerability Advisor adjusts its verdict based on any exemptions that you've specified. This verdict can be used by image security enforcement to prevent the deployment of nonsecure images in {{site.data.keyword.containerlong_notm}}. 

Fixing the security and configuration issues that are reported by Vulnerability Advisor can help you to secure your {{site.data.keyword.cloud_notm}} infrastructure.


## About Vulnerability Advisor
{: #about}

Vulnerability Advisor provides functions to help you to secure your images.

{:shortdesc}

 The following functions are available:

-   Scans images for issues
-   Scans running containers for issues if you've [installed the container scanner](#va_install_container_scanner) in each cluster
-   Provides an evaluation report that is based on security practices that are specific to {{site.data.keyword.containerlong_notm}}
-   Provides recommendations to secure configuration files for a subset of application types
-   Provides instructions about how to fix a reported [vulnerable package](#packages) or [configuration issue](#app_configurations) in its reports
-   Provides verdicts to [image security enforcement](../Registry/registry_security_enforce.html#security_enforce)
-   Applies exemptions to reports at an account, namespace, repository, or tag level to mark when issues that are flagged do not apply to your use case
-   Provides links to associated containers from the **Tag** view of the {{site.data.keyword.registrylong_notm}} graphical user interface. You can list the containers that are running and that are using that image in a cluster that has the container scanner installed.


In the Registry dashboard, the **Policy Status** column displays the status of your repositories. The linked report identifies good cloud security practices for your images. 

The Vulnerability Advisor dashboard provides an overview and assessment of the security for an image and, if the container scanner is installed, links to running containers. If you want to find out more about the Vulnerability Advisor dashboard, see [Reviewing a vulnerability report](#va_reviewing).
	
	
**Data protection**

To scan images and containers in your account for security issues, Vulnerability Advisor collects, stores, and processes the following information:
- free-form text fields, including IDs, descriptions, and image names (registry, namespace, repository name, and image tag)
- Kubernetes metadata including names of Kubernetes resources such as pod, replicaset, and deployment names
- metadata about the file modes and creation timestamps of the configuration files
- the content of system and application configuration files in images and containers
- installed packages and libraries (including their versions)

Do not put personal information into any field or location that Vulnerability Advisor processes, as identified in the preceding list.

Scan results, aggregated at a data center level, are processed to produce anonymized metrics to operate and improve the service.

Scan results are deleted 30 days after they are generated.


## Types of vulnerabilities
{: #types}


### Vulnerable packages
{: #packages}

Vulnerability Advisor checks for vulnerable packages in images that are based on supported operating systems and provides a link to any relevant security notices about the vulnerability.
{:shortdesc}

Packages with known vulnerability issues are displayed in the scan results. The possible vulnerabilities are updated daily from published security notices for the Docker image types that are listed in the following table. Typically, for a vulnerable package to pass the scan, a later version of the package is required that includes a fix for the vulnerability. The same package might list multiple vulnerabilities, and in this case, a single package upgrade might address multiple vulnerabilities.


  |Docker base image|Source of security notices|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://git.alpinelinux.org/) and [CIRCL CVE Search ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lists.centos.org/pipermail/centos-announce/) and [CentOS CR announce archives ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://usn.ubuntu.com/)|
  {: caption="Table 1. Supported Docker base images that Vulnerability Advisor checks for vulnerable packages" caption-side="top"}


### Configuration issues
{: #app_configurations}

Configuration issues are potential security issues that are related to how an app is set up. Many of the reported problems can be fixed by updating your Dockerfile.
{:shortdesc}

Images are scanned only if they are based on an operating system that is supported by Vulnerability Advisor. Vulnerability Advisor checks the configuration settings for the following types of apps:
-   MySQL
-   NGINX
-   Apache




## Installing the container scanner
{: #va_install_container_scanner}

Before you begin:

1.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI client. If you have a federated account, use `--sso`.
2.  [Target your `kubectl` CLI](../../containers/cs_cli_install.html#cs_cli_configure) to the cluster where you want to use a Helm chart.
3.  Create a service ID and API key for the container scanner and give it a name:
    1.  Create a service ID by running the following command, replacing `<scanner_serviceID>` with a name of your choice for the service ID. Note its **CRN**.
    
        ```
    	bx iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  Create a service API key, where `<scanner_serviceID>` is the service ID that you created in the previous step and replacing  `<scanner_APIkey_name>` with a name of your choice for the scanner API key. 
    
        ```
    	bx iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	    The scanner API key is returned.
	
	    Ensure that you store your scanner API key safely because it cannot be retrieved later.
	    {: tip}
	
    3.  Create a service policy that grants the `Writer` role.
    		
        ```
    	bx iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

To configure the Helm chart:

1.  [Set up Helm in your cluster](../../containers/cs_integrations.html#helm). If you use an RBAC policy to grant the Helm tiller access, make sure that the tiller role has access to all namespaces so that the scanner can watch containers in all namespaces.

2.  Add the IBM chart repository to your Helm, such as `ibm-incubator`.

    ```
    helm repo add ibm-incubator https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

3.  Save the default configuration settings for the container scanner Helm chart in a local YAML file. Include the chart repository, such as `ibm-incubator`, in the Helm chart path.

    ```
    helm inspect values ibm-incubator/ibmcloud-container-scanner > config.yaml
    ```
    {: pre}

4.  Edit the `config.yaml` file.

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
    <caption>Understanding the YAML file components</caption>
    <thead>
    <th>Field</th>
    <th>Value</th>
    </thead>
    <tbody>
    <tr>
    <td><code>EmitURL</code></td>
    <td>Enter the Vulnerability Advisor regional endpoint URL. To get the URL, run <code>bx cr info</code> and retrieve the <strong>Container Registry</strong> address. Replace <code>registry</code> with <code>va</code>. For example: <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>Replace with the {{site.data.keyword.Bluemix_notm}} account ID that your cluster is in. To get the account ID, run <code>bx account list</code>.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>Replace with the Kubernetes cluster that you want to install the container scanner in. To list cluster IDs, run <code>bx cs clusters</code>.
    </td>
    </tr>
    <tr>
    <td><code>APIKey</code></td>
    <td>Replace with the scanner API key that you created earlier.</td>
    </tr>
    </tbody></table>

5.  Install the Helm chart to your cluster with the updated `config.yaml` file. The updated properties are stored in a configmap for your chart. Replace `<myscanner>` with a name of your choice for your Helm chart. Include the chart repository, such as `ibm-incubator`, in the Helm chart path.

    ```
    helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    **Note**: The container scanner is installed into the `kube-system` namespace, but scans containers from all namespaces.

6.  Check the chart deployment status. When the chart is ready, the **STATUS** field near the top of the output has a value of `DEPLOYED`.

    ```
    helm status <myscanner>
    ```
    {: pre}

7.  After the chart is deployed, verify that the updated settings in the `config.yaml` file were used.

    ```
    helm get values <myscanner>
    ```
    {: pre}


IBM Container Scanner is now installed, and the agent is deployed as a [DaemonSet ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) in your cluster. Although the scanner is deployed to the `kube-system` namespace, it scans all containers that are assigned to pods in all your Kubernetes namespaces, such as `default`. 


## Reviewing a vulnerability report by using the GUI
{: #va_reviewing}

Before you deploy an image, you can review its Vulnerability Advisor report for details about any vulnerable packages and nonsecure app settings.
{:shortdesc}



1.  Log in to {{site.data.keyword.Bluemix_notm}}.
2.  Click **Catalog**.
3.  Under **Infrastructure**, click **Containers**.
4.  Click the **Container Registry** tile.
5.  Expand **Vulnerability Advisor** and click **Scanned Repositories**.
6.  To see the report for the image that is tagged `latest`, click the row for that repository. The report shows the total number of issues and whether they are vulnerable packages or configuration issues. If no `latest` tag exists in the repository, the most recent image is used.
7.  To view information about each vulnerable package for the image you selected, in the **Vulnerable Packages Found** table, click the link in the **VULNERABILITIES** column to open the report.
    1.  To see more information, expand the summary.
    2.  If an operating system distributor's notice is provided, click the link in the **OFFICIAL NOTICE** column.
8.  To view information about each configuration issue, in the **Configuration Issues Found** table, click the row for the issue.
9.  Perform the corrective action for each issue shown in the report, and rebuild the image. Some issues in the Dockerfile can be resolved by using the code that is provided in [Resolving problems in images](#va_report).

If vulnerabilities exist and you do not fix them, those issues can impact the security of containers that are built with that image. However, you can continue to use an image that has security and configuration issues in a container.

 


## Reviewing a vulnerability report by using the CLI
{: #va_registry_cli}

You can review the security of Docker images that are stored in your namespaces in {{site.data.keyword.registrylong_notm}} by using the CLI.
{:shortdesc}

1.  List the images in your {{site.data.keyword.Bluemix_notm}} account. A list of all images is returned, independent of the namespace where they are stored.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Check the status in the **SECURITY STATUS** column.
    -   `No Issues`: No security issues were found.
    -   `X Issues`: Potential security issues or vulnerabilities were found.
    -   `Scanning`: The image is being scanned and the final vulnerability status is not yet determined.
4.  To view the details for the status, review the Vulnerability Advisor report.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    In the CLI output, you can view the following information about the configuration issues.
      - Security practice: A description of the vulnerability that was found
      - Corrective action: Details about how to fix the vulnerability


## Reviewing a container report
{: #va_reviewing_container}

In your dashboard, you can see the status of a container to determine whether its security complies with your organization's policy. You can also review a container's Security report, which details any vulnerable packages and nonsecure container or application settings, and whether the container is compliant with organizational policies.
{:shortdesc}

Check that your container is as secure as possible by viewing its Security report and act on any reported security or configuration issues, by completing the following steps:

1.  Select the container that you want to view a report for:
    1.  From the Catalog, select **Containers**, click **Container Registry**.
    2.  Select the **Private Repositories** tab and select the row for the repository that you want.
    3.  Select the row for the image tag that you want.
    4.  Select the **Associated Containers** tab and then select the row for the container that you want. The security report opens.
2.  Review the sections to see the potential security and configuration issues for each package in the image:

      -   **Vulnerabilities**: Lists packages with known vulnerability issues, which are updated daily from published security notices for the Docker image types that are listed in [Managing image security with Vulnerability Advisor](va_index.html). Typically, for a vulnerable package to pass the scan, a later version of the package is required that includes a fix for the vulnerability. The same package might list multiple vulnerabilities, and in this case, a single package upgrade might correct multiple issues. Click the security notice code to review more information on the package and for steps to update the package.

    -   **Configuration Issues**: Lists suggestions that you can take to increase the security of the container and any application settings for the container that are nonsecure. Expand the row to view how to resolve the issue.

   Corrective actions or suggestions are provided for each item that is listed.
   
3.  Review the policy status for each security issue. The policy status indicates whether this issue is exempt.

    -  **Active**: You have an issue that is not exempted and the issue is affecting your security status.
    -  **Exempt**: This issue has been exempted by your policy settings.
    -  **Partially exempt**: This issue is associated with more than one security notice. The security notices are not all exempt.

4.  Decide how to update the container so that you can resolve the problems.

    **Important:** To fix problems with the container image, you must delete the old instance and redeploy, which means losing any data within the existing container. Ensure that you have a good understanding of your container architecture to choose the appropriate method of redeploying the container.

    For example:

    -   If your container is decoupled from data that it computes, you can stop the container and delete it, make the required changes to the image, and redeploy, with no data loss.
    -   You can use an {{site.data.keyword.Bluemix_notm}} service to assist, such as [Delivery Pipeline](../ContinuousDelivery/pipeline_about.html), with updating the vulnerable container instance.
    -   In a microservices architecture, you might route traffic to another container instance while you fix security or configuration issues, and push the new image in a red/black deployment.

5.  If it's not possible to fix the issue now, you can exempt the issue in your policy settings, which prevents the issue from blocking the deployment of the container. To exempt the issue, click the **Dots** menu and click **Create Exemption**.

6.  Fix the problems that are described in the **Security** report, and rebuild the image or redeploy the container according to the method you chose. Some issues in the Dockerfile can be resolved by using the code that is provided in [Resolving problems in images](/docs/services/va/va_index.html#va_report).


## Resolving common problems in images
{: #va_report}

Review the example fixes for common problems that might be reported by Vulnerability Advisor. Some problems can be fixed by updating your Dockerfile.
{:shortdesc}


### Maximum password age, minimum password days, and minimum password length
{: #va_password}

**Problem**: You receive one or more of the following vulnerabilities:

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
{: codeblock}


### SSH vulnerability
{: #ssh}

**Problem**: The following vulnerability is returned:

```
SSH server should not be installed.
```
{: screen}

**Fix**: Instead of using SSH, use `docker attach` or `docker exec` to access your container. Ensure that your Dockerfile does not contain any steps for installing an SSH Server.
