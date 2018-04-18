---

copyright:
  years: 2017, 2018
lastupdated: "2018-04-18"

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

Vulnerability Advisor checks the security status of container images that are provided by IBM, third parties, or added to your organization's registry namespace.
{:shortdesc}

When you add an image to a namespace, the image is automatically scanned by Vulnerability Advisor to detect security issues and potential vulnerabilities. If security issues are found, instructions are provided to help fix the reported vulnerability. You can still deploy containers from vulnerable images, but keep in mind that those containers might be attacked or compromised.


## About Vulnerability Advisor
{: #about}

Vulnerability Advisor provides security management for {{site.data.keyword.containerlong}}. Vulnerability Advisor generates a security status report, suggests fixes and best practices, and provides management to restrict nonsecure images from running. Fixing the security and configuration issues that are reported by Vulnerability Advisor can help you secure your {{site.data.keyword.cloud_notm}} infrastructure.
{:shortdesc}

Vulnerability Advisor includes the following features:

-   Scans images for vulnerabilities
-   Provides an evaluation report that is based on security practices that are specific to {{site.data.keyword.containerlong_notm}}
-   Provides recommendations to secure configuration files for a subset of application types
-   Provides instructions about how to fix a reported [vulnerable package](#packages) or [configuration issue](#app_configurations) in its reports

In the Registry dashboard, the **SECURITY REPORT** column displays the status of your repositories. The report identifies good cloud security practices for your images. 

The Vulnerability Advisor dashboard provides an overview and assessment of the security for an image. To find out more about the Vulnerability Advisor dashboard, see [Reviewing a vulnerability report](#va_reviewing).


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
{: #va_install_livescan}

Before you begin:

1.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI client. If you have a federated account, use `--sso`.
1.  [Target your `kubectl` CLI](../../containers/cs_cli_install.html#cs_cli_configure) to the cluster where you want to use a Helm chart.
1.  Create a service ID and API key for the container scanner. and give it a name. 		    
    1.  Create a service ID. Note its **CRN**. 
        
        ```
		    bx iam service-id-create <scanner_serviceID>
		    ```
        {: pre}
    2.  Create a service API key, where `<scanner_serviceID>` is the service ID that you previously made. Make sure to store your API key safely, as it cannot be retrieved later.
    	  
        ```
    		bx iam service-api-key-create <scanner_APIkey> <scanner_serviceID>
    		```
        {: pre}
    3.  Create a service policy that grants the `Writer` role.
    		
        ```
    		bx iam service-policy-create <scanner_serviceID> -r Writer
    		```
        {: pre}

To configure the Helm chart:

1.  [Set up Helm in your cluster](../../containers/cs_integrations.html#helm). If you use an RBAC policy to grant the Helm tiller access, make sure that the tiller role has access to all namespaces so that the scanner can watch containers in all namespaces.

1.  Add the IBM chart repository to your Helm, such as `ibm-incubator`.

    ```
    helm repo add ibm-incubator https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

1.  Save the default configuration settings for the container scanner Helm chart in a local YAML file. Include the chart repository, such as `ibm-incubator`, in the Helm chart path.

    ```
    helm inspect values ibm-incubator/ibmcloud-container-scanner > config.yaml
    ```
    {: pre}

1.  Edit the `config.yaml` file.

    ```yaml
    EmitURL: <regional_emit_URL>
    IAMURL: <regional_IAM_URL>
    AccountID: <IBM_Cloud_account_ID>
    ClusterID: <cluster_ID>
    APIKey: <service_API_key>
    ServiceID: <service_ID>
    ...
    ```

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
    <td>Enter the Vulnerability Advisor regional endpoint URL. To get the URL, run <code>bx cr info</code> and retrieve the <strong>Container Registry</strong> address. Replace <code>registry</code> with <code>va</code>. For example: <code>https://va.eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>IAMURL</code></td>
    <td>Replace with <code>https://iam.bluemix.net</code>.</td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>Replace with the {{site.data.keyword.Bluemix_notm}} account ID that your cluster is in. To get the account ID, run <code>bx account list</code>.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>Replace with the Kubernetes cluster that you want to install the container scanner in. To list cluster IDs, run <code>bx cs clusters</code>.</td>
    </tr>
    <tr>
    <td><code>APIKey</code></td>
    <td>Replace with the API key that you created before you began.</td>
    </tr>
    <tr>
    <td><code>ServiceID</code></td>
    <td>Replace with the service ID **CRN** that you created before you began.</td>
    </tr>
    </tbody></table>

1.  Install the Helm chart to your cluster with the updated `config.yaml` file. The updated properties are stored in a configmap for your chart. Replace `<myscanner>` with a name for your Helm chart. Include the chart repository, such as `ibm-incubator`, in the Helm chart path.

    ```
    helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    **Note**: The container scanner is installed into the `kube-system` namespace, but scans containers from all namespaces.

1.  Check the chart deployment status. When the chart is ready, the **STATUS** field near the top of the output has a value of `DEPLOYED`.

    ```
    helm status <myscanner>
    ```
    {: pre}

1.  After the chart is deployed, verify that the updated settings in the `config.yaml` file were used.

    ```
    helm get values <myscanner>
    ```
    {: pre}


IBM Container Scanner is now installed, and the livescan agent is deployed as a [DaemonSet ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) in your cluster. Although the scanner is deployed to the `kube-system` namespace, it scans all containers that are assigned to pods in all your Kubernetes namespaces, such as `default`. 



## Reviewing a vulnerability report
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

2.  Check the status in the **VULNERABILITY STATUS** column.
    -   `OK`: No security issues were found
    -   `Vulnerable`: A potential security issue or vulnerability was found
    -   `Unknown`: The image is being scanned and the final vulnerability status is not yet determined
    -   `Unsupported OS`: The image is not supported to be scanned by Vulnerability Advisor
4.  To view the details for the status, review the Vulnerability Advisor report.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    In the CLI output, you can view the following information about the configuration issues.
      - Security practice: A description of the vulnerability that was found
      - Corrective action: Details about how to fix the vulnerability


## Resolving common problems in images
{: #va_report}

Review the example fixes for common problems that might be reported by Vulnerability Advisor. Some of the reported problems can be fixed by updating your Dockerfile.
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

### SSH vulnerability
{: #ssh}

**Problem**: The following vulnerability is returned:

```
SSH server should not be installed.
```
{: screen}

**Fix**: Instead of using SSH, use `docker attach` or `docker exec` to access your container. Ensure that your Dockerfile does not contain any steps for installing an SSH Server.
