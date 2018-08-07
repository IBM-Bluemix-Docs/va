---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-07"

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

Vulnerability Advisor checks the security status of container images that are provided by {{site.data.keyword.IBM}}, third parties, or added to your organization's registry namespace. If you've installed the Container Scanner in each cluster, Vulnerability Advisor also checks the status of containers that are running.
{:shortdesc}

When you add an image to a namespace, the image is automatically scanned by Vulnerability Advisor to detect security issues and potential vulnerabilities. If security issues are found, instructions are provided to help fix the reported vulnerability. 

Vulnerability Advisor provides security management for {{site.data.keyword.registrylong_notm}}, generating a security status report that includes suggested fixes and best practices. 

Any issues that are found result in a verdict that indicates that it is not advisable to deploy this image. If you choose to deploy the image, any containers that are deployed from the image have known issues that might be used to attack or otherwise compromise the container. Vulnerability Advisor adjusts its verdict based on any exemptions that you've specified. This verdict can be used by Container Image Security Enforcement to prevent the deployment of nonsecure images in {{site.data.keyword.containerlong_notm}}. 

Fixing the security and configuration issues that are reported by Vulnerability Advisor can help you to secure your {{site.data.keyword.cloud_notm}} infrastructure.


## About Vulnerability Advisor
{: #about}

Vulnerability Advisor provides functions to help you to secure your images.

{:shortdesc}

 The following functions are available:

-   Scans images for issues
-   Scans running containers for issues if you've [installed the Container Scanner](#va_install_container_scanner) in each cluster
-   Provides an evaluation report that is based on security practices that are specific to {{site.data.keyword.containerlong_notm}}
-   Provides recommendations to secure configuration files for a subset of application types
-   Provides instructions about how to fix a reported [vulnerable package](#packages) or [configuration issue](#app_configurations) in its reports
-   Provides verdicts to [Container Image Security Enforcement](/docs/services/Registry/registry_security_enforce.html#security_enforce)
-   Applies exemptions to reports at an account, namespace, repository, or tag level to mark when issues that are flagged do not apply to your use case
-   Provides links to associated containers from the **Tag** view of the {{site.data.keyword.registrylong_notm}} graphical user interface. You can list the containers that are running and that are using that image in a cluster that has the Container Scanner installed.


In the Registry dashboard, the **Policy Status** column displays the status of your repositories. The linked report identifies good cloud security practices for your images. 

The Vulnerability Advisor dashboard provides an overview and assessment of the security for an image and, if the Container Scanner is installed, links to running containers. If you want to find out more about the Vulnerability Advisor dashboard, see [Reviewing a vulnerability report](#va_reviewing).
	
	
**Data protection**

To scan images and containers in your account for security issues, Vulnerability Advisor collects, stores, and processes the following information:
- Free-form text fields, including IDs, descriptions, and image names (registry, namespace, repository name, and image tag)
- Kubernetes metadata including names of Kubernetes resources such as pod, replicaset, and deployment names
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

## Installing the Container Scanner
{: #va_install_container_scanner}

Before you begin:

1.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI client. If you have a federated account, use `--sso`.
2.  [Target your `kubectl` CLI](/docs/containers/cs_cli_install.html#cs_cli_configure) to the cluster where you want to use a Helm chart.
3.  Create a service ID and API key for the Container Scanner and give it a name:
    1.  Create a service ID by running the following command, replacing `<scanner_serviceID>` with a name of your choice for the service ID. Note its **CRN**.
    
        ```
    	ibmcloud iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  Create a service API key, where `<scanner_serviceID>` is the service ID that you created in the previous step and replacing  `<scanner_APIkey_name>` with a name of your choice for the scanner API key. 
    
        ```
    	ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	    The scanner API key is returned.
	
	    Ensure that you store your scanner API key safely because it cannot be retrieved later.
	    {: tip}
	
    3.  Create a service policy that grants the `Writer` role.
    		
        ```
    	ibmcloud iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

To configure the Helm chart:

1.  [Set up Helm in your cluster](/docs/containers/cs_integrations.html#helm). If you use an RBAC policy to grant the Helm tiller access, make sure that the tiller role has access to all namespaces so that the Container Scanner can watch containers in all namespaces.

2.  Add the IBM chart repository to your Helm, such as `ibm-incubator`.

    ```
    helm repo add ibm-incubator https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

3.  Save the default configuration settings for the Container Scanner Helm chart in a local YAML file. Include the chart repository, such as `ibm-incubator`, in the Helm chart path.

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
    <td>Enter the Vulnerability Advisor regional endpoint URL. To get the URL, run <code>ibmcloud cr info</code> and retrieve the <strong>Container Registry</strong> address. Replace <code>registry</code> with <code>va</code>. For example: <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>Replace with the {{site.data.keyword.Bluemix_notm}} account ID that your cluster is in. To get the account ID, run <code>ibmcloud account list</code>.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>Replace with the Kubernetes cluster that you want to install the Container Scanner in. To list cluster IDs, run <code>ibmcloud ks clusters</code>. <br> **Tip**: Use the ID of the cluster, not the name.
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
    
    The Container Scanner is installed into the `kube-system` namespace, but scans containers from all namespaces.
    {:tip}

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


The Container Scanner is now installed, and the agent is deployed as a [DaemonSet ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) in your cluster. Although the Container Scanner is deployed to the `kube-system` namespace, it scans all containers that are assigned to pods in all your Kubernetes namespaces, such as `default`.


## Running the Container Scanner from behind a firewall
{: #va_firewall}

If your firewall blocks outgoing connections, you must configure your firewall to allow worker nodes to access the Container Scanner on TCP port <code>443</code> on the IP addresses in the following table.
{:shortdesc}

 

<p>
  <table summary=" The rows should be read left to right, with the server zone in column one and IP addresses to match in column two.">
  <caption>IP addresses to open for outgoing traffic</caption>
      <thead>
      <th>Region</th>
      <th>IP address</th>
      </thead>
    <tbody>
      <tr>
         <td>AP South</td>
         <td><code>168.1.40.158</code><br><code>130.198.65.182</code></td>
      </tr>
      <tr>
         <td>EU Central</td>
         <td><code>159.8.220.182</code><br><code>158.177.74.102</code></td>
        </tr>
      <tr>
        <td>UK South</td>
        <td><code>158.175.71.134</code><br><code>5.10.111.190</code></td>
      </tr>
      <tr>
        <td>US East</td>
         <td><code>169.60.73.158</code><br><code>169.61.84.102</code></td>
      </tr>
      <tr>
        <td>US South</td>
        <td><code>169.47.103.118</code><br><code>169.48.165.6</code></td>
      </tr>
      </tbody>
    </table>
</p>


## Setting organizational exemption policies
{: #va_managing_policy}

If you want to manage the security of an {{site.data.keyword.Bluemix_notm}} organization, you can use your policy setting to determine whether a given issue is exempt or not. You can also choose to use Container Image Security Enforcement to ensure that deployment is allowed only from images that contain no security issues, after discounting those issues that are exempted by your policy.
{:shortdesc}

You can deploy containers from any image regardless of security status unless Container Image Security Enforcement is deployed in your cluster. To find out how to deploy Container Image Security Enforcement see [Installing security enforcement](/docs/services/Registry/registry_security_enforce.html#security_enforce).

When you use Container Image Security Enforcement, any security issue that is detected by Vulnerability Advisor prevents a container being deployed from the image. To allow an image with detected issues to be deployed, exemptions must be added to your policy.

### Setting organizational exemption policies by using the GUI
{: #va_managing_policy_gui}

If you want to set exemptions to the policy by using the GUI, complete the following steps:

1.  Log in to {{site.data.keyword.Bluemix_notm}}. You must be logged in to see Vulnerability Advisor in the GUI.
2.  Click  the **Menu** icon and then click **Containers**.
3.  Under **Vulnerability Advisor**, click **Policy Settings**.
4.  Click **Create Exemption**.
5.  Select the issue type.
6.  Enter the issue ID. 

    You can find this information in your [vulnerability report](#va_reviewing). The **Vulnerability ID** column contains the ID to use for CVE or security notice issues; the **Configuration Issue ID** column contains the ID to use for configuration issues.
    {: tip}


7.  Select the registry namespace, repository, and tag that you want the exemption to apply to.
8.  Click **Save**.

You can also edit and remove exemptions by hovering over the relevant row and clicking the **open and close list of options** icon.

### Setting organizational exemption policies by using the CLI
{: #va_managing_policy_cli}

If you want to set exemptions to the policy by using the CLI, you can run the following commands:

-  To create an exemption for a security issue, run the [ibmcloud cr exemption-add](/docs/services/Registry/registry_cli.html#bx_cr_exemption_add) command.
-  To list your exemptions for security issues, run the [ibmcloud cr exemption-list](/docs/services/Registry/registry_cli.html#bx_cr_exemption_list) command.
-  To list the types of security issues that you can exempt, run the [ibmcloud cr exemption-types](/docs/services/Registry/registry_cli.html#bx_cr_exemption_types) command.
-  To delete an exemption for a security issue, run the [ibmcloud cr exemption-rm](/docs/services/Registry/registry_cli.html#bx_cr_exemption_rm) command.

For more information about the commands, you can use the `--help` flag when you run the command.


## Reviewing a vulnerability report
{: #va_reviewing}

Before you deploy an image, you can review its Vulnerability Advisor report for details about any vulnerable packages and nonsecure container or app settings, and whether the image is compliant with organizational policies.
{:shortdesc}

If you do not address any discovered issues, those issues can impact the security of containers that are built with that image. If you haven't deployed Container Image Security Enforcement, you can continue to use an image that has security and configuration issues in a container. If Container Image Security Enforcement is deployed and active for the image, all issues discovered must be exempt by your policy for containers to be deployable from this image. 

To configure the scope of enforcement of Vulnerability Advisor issues in Container Image Security Enforcement, see [Customizing policies](/docs/services/Registry/registry_security_enforce.html#customize_policies).
{:tip}

If your image does not meet the requirements that are set by your organization's policy, you must configure the image to meet those requirements before you can deploy it. For information about how to view and change the organization policy, see [Setting organizational exemption policies](#va_managing_policy).
{:tip}

After you deploy your image, if you have deployed Container Scanner, Vulnerability Advisor continues to scan for security and configuration issues in the container. You can resolve any problems that are found by following the steps that are described in [Reviewing a container report](#va_reviewing_container).

### Reviewing a vulnerability report by using the GUI
{: #va_reviewing_gui}

You can review the security of Docker images that are stored in your namespaces in {{site.data.keyword.registrylong_notm}} by using the GUI.
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
9.  Perform the corrective action for each issue shown in the report, and rebuild the image.


### Reviewing a vulnerability report by using the CLI
{: #va_registry_cli}

You can review the security of Docker images that are stored in your namespaces in {{site.data.keyword.registrylong_notm}} by using the CLI.
{:shortdesc}

1.  List the images in your {{site.data.keyword.Bluemix_notm}} account. A list of all images is returned, independent of the namespace where they are stored.

    ```
    ibmcloud cr image-list
    ```
    {: pre}

2.  Check the status in the **SECURITY STATUS** column.
    -   `No Issues`: No security issues were found.
    -   `<X> Issues`: `<X>` potential security issues or vulnerabilities were found, where `<X>` is the number of issues.
    -   `Scanning`: The image is being scanned and the final vulnerability status is not yet determined.
    
3.  To view the details for the status, review the Vulnerability Advisor report.

    ```
    ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    In the CLI output, you can view the following information about the configuration issues.
      - Security practice: A description of the vulnerability that was found
      - Corrective action: Details about how to fix the vulnerability


## Reviewing a container report
{: #va_reviewing_container}

In your dashboard, you can see the status of a container to determine whether its security complies with your organization's policy. You can also review a container's Security report, which details any vulnerable packages and nonsecure container or application settings, and whether the container is compliant with organizational policies.
{:shortdesc}

Check that containers that are running in your space continue to be compliant with organizational policy by reviewing the **Policy Status** field. The status is displayed as one of the following conditions:

-   Compliant with Policy - No security or configuration issues were found.
-   Not Compliant with Policy - Vulnerability Advisor found potential security or configuration issues that caused the container to not be compliant with policy. If your organizational policy permits deployment of vulnerable images, the image might have been deployed in the state Deploy with Caution, and a warning was sent to the user that deployed it.
-   Incomplete Assessment - The scan is not complete. The scan might still be running, or the operating system for that container instance might not be compatible.

Check that your container is as secure as possible by viewing its Security report and act on any reported security or configuration issues, by completing the following steps:

1.  Select the container that you want to view a report for:
    1.  From the Catalog, select **Containers**, click **Container Registry**.
    2.  Select the **Private Repositories** tab and select the row for the repository that you want.
    3.  Select the row for the image tag that you want.
    4.  Select the **Associated Containers** tab and then select the row for the container that you want. The security report opens.
2.  Review the sections to see the potential security and configuration issues for each package in the image:

      -   **Vulnerabilities**: Lists packages with known vulnerability issues, which are updated daily from published security notices for the Docker image types that are listed in [Types of vulnerabilities](#types). Typically, for a vulnerable package to pass the scan, a later version of the package is required that includes a fix for the vulnerability. The same package might list multiple vulnerabilities, and in this case, a single package upgrade might correct multiple issues. Click the security notice code to review more information on the package and for steps to update the package.

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
    -   You can use an {{site.data.keyword.Bluemix_notm}} service to assist, such as [Delivery Pipeline](/docs/services/ContinuousDelivery/pipeline_about.html#deliverypipeline_about), with updating the vulnerable container instance.
    -   In a microservices architecture, you might route traffic to another container instance while you fix security or configuration issues, and push the new image in a red/black deployment.

5.  If it's not possible to fix the issue now, you can exempt the issue in your policy settings, which prevents the issue from blocking the deployment of the container. To exempt the issue, click the **open and close list of options** icon and click **Create Exemption**, see [Setting organizational exemption policies](#va_managing_policy).

6.  Fix the problems that are described in the **Security** report, and rebuild the image or redeploy the container according to the method you chose.
