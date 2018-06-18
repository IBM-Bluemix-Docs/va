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


# 使用漏洞警告器管理映像檔安全
{: #va_index}

「漏洞警告器」會針對由 IBM、協力廠商所提供的容器映像檔，或者新增至您組織登錄名稱空間的容器映像檔，檢查其安全狀態。
{:shortdesc}


當您將映像檔新增至名稱空間時，「漏洞警告器」會自動掃描映像檔，以偵測安全問題及潛在漏洞。如果找到安全問題，會提供指示以協助修正報告的漏洞。您仍然可以從有漏洞的映像檔中部署容器，但是請記住，那些容器可能會遭受攻擊或洩漏。


## 關於漏洞警告器
{: #about}

「漏洞警告器」為 {{site.data.keyword.containerlong}} 提供安全管理。「漏洞警告器」會產生安全狀態報告、建議修正程式及最佳作法，以及提供管理以限制不安全的映像檔執行。修正「漏洞警告器」所報告的安全及配置問題，有助於您保護 {{site.data.keyword.cloud_notm}} 基礎架構。
{:shortdesc}

「漏洞警告器」包含下列特性：

-   掃描映像檔是否有漏洞
-   提供評估報告，該報告根據 {{site.data.keyword.containerlong_notm}} 特定的安全作法。
-   提供建議以保護部分應用程式類型的配置檔
-   提供指示，告知如何修正已報告的[有漏洞套件](#packages)，或其報告中的[配置問題](#app_configurations)

在「登錄」儀表板中，**安全報告**直欄會顯示儲存庫的狀態。此報告會識別映像檔的良好雲端安全作法。 

「漏洞警告器」儀表板提供映像檔的安全概觀與評量。若要找出「漏洞警告器」儀表板的相關資訊，請參閱[檢閱漏洞報告](#va_reviewing)。
	
	
**資料保護**

為了掃描您帳戶中映像檔與容器的安全問題，「漏洞警告器」會收集、儲存及處理下列資訊：
- 開放式文字欄位，包括 ID、說明及映像檔名稱（登錄、名稱空間、儲存庫名稱及映像檔標籤）
- Kubernetes meta 資料，包括 Kubernetes 資源的名稱，例如，Pod、抄本集與部署名稱
- 有關配置檔的檔案模式及建立時間戳記的 meta 資料
- 映像檔及容器中的系統內容及應用程式配置檔
- 已安裝套件及媒體庫（包括其版本）

請勿將個人資訊放到「漏洞警告器」會處理的任何欄位或位置中，如之前清單中所示。

掃描結果（聚集在資料中心層次）會進行處理以產生匿名度量，以便運作及改進服務。

掃描結果會在產生 30 天之後刪除。



## 漏洞類型
{: #types}

### 有漏洞的套件
{: #packages}

「漏洞警告器」會檢查以支援作業系統為基礎的映像檔中有漏洞的套件，並提供關於漏洞之任何相關安全注意事項的鏈結。{:shortdesc}

掃描結果中會顯示具有已知漏洞問題的套件。潛在漏洞會根據下表所列 Docker 映像檔類型的已發佈安全注意事項每天進行更新。一般而言，為了讓有漏洞的套件通過掃描，需要有包含漏洞修正程式的更新版本套件。相同的套件可能會列出多個漏洞，而在此情況下，單一套件升級可能會解決多個漏洞。


  |Docker 基礎映像檔|安全注意事項的來源|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://git.alpinelinux.org/) 及 [CIRCL CVE Search ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.circl.lu/)|
  |CentOS|[CentOS 公告保存 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://lists.centos.org/pipermail/centos-announce/) 及 [CentOS CR 公告保存 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian 安全公告 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://usn.ubuntu.com/)|
  {: caption="表 1. 「漏洞警告器」檢查套件是否有漏洞的受支援 Docker 基礎映像檔" caption-side="top"}




### 配置問題
{: #app_configurations}

配置問題是指與應用程式設定方式有關的潛在安全問題。許多已報告的問題可以藉由更新 Dockerfile 而修正。{:shortdesc}

只有在映像檔是以「漏洞警告器」所支援之作業系統為基礎時才會掃描映像檔。「漏洞警告器」會檢查下列應用程式類型的配置設定：
-   MySQL
-   NGINX
-   Apache



## 安裝容器掃描器
{: #va_install_livescan}

開始之前：

1.  登入 {{site.data.keyword.Bluemix_notm}} CLI 用戶端。如果您有聯合帳戶，請使用 `--sso`。
2.  [將您 `kubectl` CLI](../../containers/cs_cli_install.html#cs_cli_configure) 的目標設為要在其中使用 Helm 圖表的叢集。
3.  建立容器掃描器的服務 ID 及 API 金鑰，並進行命名：
    1.  執行下列指令以建立服務 ID，將 `<scanner_serviceID>` 取代為您所選的服務 ID 名稱。請注意其 **CRN**。
    
        ```
    	bx iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  建立服務 API 金鑰，其中 `<scanner_serviceID>` 是您在前一個步驟中所建立的服務 ID，並將 `<scanner_APIkey_name>` 取代為您所選的掃描器 API 金鑰名稱。 
    
        ```
    	bx iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	    此時會傳回掃描器 API 金鑰。
	
	    請確定安全儲存您的掃描器 API 金鑰，因為稍後無法再擷取該金鑰。
	    {: tip}
	
    3.  建立授予 `Writer` 角色的服務原則。
    		
        ```
    	bx iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

若要配置 Helm 圖表，請執行下列動作：

1.  [在您的叢集中設定 Helm](../../containers/cs_integrations.html#helm)。如果您使用 RBAC 原則來授予 Helm tiller 存取權，請確定 tiller 角色擁有所有名稱空間的存取權，以便掃描器可以監看所有名稱空間中的容器。

2.  將 IBM 圖表儲存庫新增至您的 Helm，例如，`ibm-incubator`。

    ```
    helm repo add ibm-incubator https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

3.  將容器掃描器 Helm 圖表的預設配置設定儲存在本端 YAML 檔案中。將圖表儲存庫（例如，`ibm-incubator`）併入 Helm 圖表路徑。

    ```
    helm inspect values ibm-incubator/ibmcloud-container-scanner > config.yaml
    ```
    {: pre}

4.  編輯 `config.yaml` 檔案。

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
    <caption>瞭解 YAML 檔案元件</caption>
    <thead>
    <th>欄位</th>
    <th>值 </th>
    </thead>
    <tbody>
    <tr>
    <td><code>EmitURL</code></td>
    <td>輸入「漏洞警告器」區域端點 URL。若要取得 URL，請執行 <code>bx cr info</code>，並擷取 <strong>Container Registry</strong> 位址。將 <code>registry</code> 取代為 <code>va</code>。例如：<code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>取代為您叢集所在的 {{site.data.keyword.Bluemix_notm}} 帳戶 ID。若要取得帳戶 ID，請執行 <code>bx account list</code>。</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>取代為您要在其中安裝容器掃描器的 Kubernetes 叢集。若要列出叢集 ID，請執行 <code>bx cs clusters</code>。</td>
    </tr>
    <tr>
    <td><code>APIKey</code></td>
    <td>取代為您稍早建立的掃描器 API 金鑰。</td>
    </tr>
    </tbody></table>

5.  以更新的 `config.yaml` 檔案，將 Helm 圖表安裝至您的叢集。更新的內容會儲存在您圖表的 configmap 中。將 `<myscanner>` 取代為您所選的 Helm 圖表名稱。將圖表儲存庫（例如，`ibm-incubator`）併入 Helm 圖表路徑。

    ```
    helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    **附註**：容器掃描器已安裝至 `kube-system` 名稱空間，但會掃描所有名稱空間中的容器。

6.  檢查圖表部署狀態。圖表已備妥時，位於輸出頂端附近的**狀態**欄位，會有 `DEPLOYED` 值。

    ```
    helm status <myscanner>
    ```
    {: pre}

7.  部署圖表之後，請驗證已使用 `config.yaml` 檔案中的已更新設定。

    ```
    helm get values <myscanner>
    ```
    {: pre}


現在，已安裝 IBM Container Scanner，且 livescan 代理程式已部署為您叢集中的 [DaemonSet ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)。雖然掃描器已部署至 `kube-system` 名稱空間，但是它仍會掃描指派至所有 Kubernetes 名稱空間中 Pod 的所有容器，例如，`default`。 



## 檢閱漏洞報告
{: #va_reviewing}

部署映像檔之前，您可以檢閱其「漏洞警告器」報告中，有關所有有漏洞套件及不安全應用程式設定的詳細資料。
{:shortdesc}



1.  登入 {{site.data.keyword.Bluemix_notm}}。
2.  按一下**型錄**。
3.  在**基礎架構**下，按一下**容器**。
4.  按一下 **Container Registry** 磚。
5.  展開**漏洞警告器**，然後按一下**掃描的儲存庫**。
6.  若要查看以 `latest` 標記之映像檔的報告，請按一下該儲存庫的列。此報告會顯示問題總數，以及它們是有漏洞的套件還是配置問題。如果儲存庫中沒有任何 `latest` 標籤存在，則會使用最新的映像檔。
7.  若要針對您所選取的映像檔，檢視每個有漏洞套件的相關資訊，請在**找到的有漏洞的套件**表格中，按一下 **漏洞**直欄中的鏈結以開啟報告。
    1.  若要查看相關資訊，請展開摘要。
    2.  如果有提供作業系統經銷商的注意事項，請按一下**官方公告**直欄中的鏈結。
8.  若要檢視每個配置問題的相關資訊，請在**找到的配置問題**表格中，按一下問題列。
9.  針對報告中顯示的每個問題執行更正動作，然後重建映像檔。藉由使用[解決映像檔中的問題](#va_report)中所提供的程式碼，可以解決 Dockerfile 中的一些問題。

如果漏洞存在，而且您未修正它們，那些問題會影響與該映像檔建置在一起之容器的安全。然而，您可以繼續使用容器中有安全及配置問題的映像檔。

 



## 使用 CLI 檢閱漏洞報告
{: #va_registry_cli}

您可以使用 CLI 來檢閱儲存在 {{site.data.keyword.registrylong_notm}} 名稱空間之 Docker 映像檔的安全。
{:shortdesc}

1.  列出 {{site.data.keyword.Bluemix_notm}} 帳戶中的映像檔。系統會傳回所有映像檔的清單，與其儲存所在的名稱空間無關。

    ```
        bx cr image-list
    ```
    {: pre}

2.  檢查**安全狀態**直欄中的狀態。
    -   `沒有問題`：找不到任何安全問題。
    -   `X 個問題`：找到潛在的安全問題或漏洞。
    -   `掃描中`：正在掃描映像檔，尚未判斷最後的漏洞狀態。
4.  若要檢視狀態的詳細資料，請檢閱「漏洞警告器」報告。

    ```
        bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    在 CLI 輸出中，您可以檢閱下列有關配置問題的資訊。
      - 安全作法：所找到之漏洞的說明
      - 更正動作：有關如何修正漏洞的詳細資料


## 解決映像檔中的一般問題
{: #va_report}

檢閱「漏洞警告器」可能會報告之一般問題的修正範例。部分問題可以藉由更新 Dockerfile 而修正。{:shortdesc}

### 密碼有效期上限、密碼有效期下限，及密碼長度下限
{: #va_password}

**問題**：您收到下列一個以上漏洞：

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

**修正程式**：藉由將下列程式碼新增至 Dockerfile 而設定密碼相符性。

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```
{: codeblock}

### SSH 漏洞
{: #ssh}

**問題**：傳回下列漏洞：

```
SSH server should not be installed.
```
{: screen}

**修正程式**：請不要使用 SSH，而是使用 `docker attach` 或 `docker exec` 來存取您的容器。請確定您的 Dockerfile 未包含任何安裝 SSH 伺服器的步驟。
