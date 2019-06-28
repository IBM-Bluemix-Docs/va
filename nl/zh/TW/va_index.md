---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-11"

keywords: IBM Cloud Kubernetes Service, IBM Cloud Container Registry, security status of container images, image security, Vulnerability Advisor, security, registry, vulnerabilities, container scanner, containers, security issues, configuration issues,

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


# 使用 Vulnerability Advisor 管理映像檔安全
{: #va_index}

Vulnerability Advisor 會針對由 {{site.data.keyword.IBM}}、協力廠商所提供的容器映像檔，或者新增至您組織登錄名稱空間的容器映像檔，檢查其安全狀態。如果您已在每個叢集安裝容器掃描器（已淘汰），則 Vulnerability Advisor 也會檢查執行中容器的狀態。
{:shortdesc}

當您將映像檔新增至名稱空間時，Vulnerability Advisor 會自動掃描映像檔，以偵測安全問題及潛在漏洞。如果找到安全問題，會提供指示以協助修正報告的漏洞。

Vulnerability Advisor 提供 [{{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started) 的安全管理，並產生包含建議修正程式與最佳作法的安全狀態報告。

Vulnerability Advisor 所發現的任何問題都會導致裁決，指出不建議部署此映像檔。如果您選擇部署映像檔，從該映像檔部署的任何容器都包含已知問題，這些已知問題可能會被用來攻擊或以其他方式洩漏容器機密。裁決會根據您指定的任何豁免而調整。這項裁決可以由 Container Image Security Enforcement 用來避免在 {{site.data.keyword.containerlong_notm}} 部署未受保護的映像檔。

修正 Vulnerability Advisor 所報告的安全及配置問題，有助於您保護 {{site.data.keyword.cloud_notm}} 基礎架構。


## 關於 Vulnerability Advisor
{: #about}

Vulnerability Advisor 提供功能來協助保護映像檔。
{:shortdesc}

可用的功能如下：

- 掃描映像檔是否有問題
- 掃描執行中的容器是否有問題，如果已在每個叢集安裝[容器掃描器](#va_install_container_scanner)的話（已淘汰）
- 提供評估報告，該報告根據 {{site.data.keyword.containerlong_notm}} 特定的安全作法。
- 提供建議以保護部分應用程式類型的配置檔
- 提供指示，告知如何修正已報告的[有漏洞套件](#packages)，或其報告中的[配置問題](#app_configurations)
- 提供裁決給 [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce)
- 在帳戶、名稱空間、儲存庫或標籤層次套用報告的豁免，以註記所標示的問題在何時不適用於您的使用案例
- 在 {{site.data.keyword.registrylong_notm}} 圖形使用者介面的**標籤**視圖提供相關聯容器的鏈結。您可以列出正在執行並且在已安裝容器掃描器（已淘汰）的叢集內使用該映像檔的容器。

在「登錄」儀表板中，**原則狀態**直欄會顯示儲存庫的狀態。鏈結的報告會識別映像檔的良好雲端安全作法。

Vulnerability Advisor 儀表板提供映像檔的安全概觀與評量，以及執行中容器的鏈結，如果已安裝容器掃描器（已淘汰）的話。如果您想要找出 Vulnerability Advisor 儀表板的相關資訊，請參閱[檢閱漏洞報告](#va_reviewing)。

**資料保護**

為了掃描您帳戶中映像檔與容器的安全問題，Vulnerability Advisor 會收集、儲存及處理下列資訊：

- 開放式欄位，包括 ID、說明及映像檔名稱（登錄、名稱空間、儲存庫名稱及映像檔標籤）
- Kubernetes meta 資料，包括 Kubernetes 資源的名稱，例如，Pod、抄本集與部署名稱
- 有關配置檔的檔案模式及建立時間戳記的 meta 資料
- 映像檔及容器中的系統內容及應用程式配置檔
- 已安裝套件及媒體庫（包括其版本）

請勿將個人資訊放到 Vulnerability Advisor 會處理的任何欄位或位置中，如之前清單中所示。

掃描結果（聚集在資料中心層次）會進行處理以產生匿名度量，以便運作及改進服務。

掃描結果會在產生 30 天之後刪除。

## 漏洞類型
{: #types}

### 有漏洞的套件
{: #packages}

Vulnerability Advisor 會檢查使用支援作業系統的映像檔中是否有具有漏洞的套件，並提供關於漏洞之任何相關安全注意事項的鏈結。
{:shortdesc}

掃描結果中會顯示包含已知漏洞問題的套件。潛在漏洞會使用下表所列 Docker 映像檔類型的已發佈安全注意事項每天進行更新。一般而言，為了讓有漏洞的套件通過掃描，需要有包含漏洞修正程式的更新版本套件。相同的套件可能會列出多個漏洞，而在此情況下，單一套件更新可能會解決多個漏洞。

  |Docker 基礎映像檔|安全注意事項的來源|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://git.alpinelinux.org/) 及 [CIRCL CVE Search ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.circl.lu/)。|
  |CentOS| [CentOS announce archives ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://lists.centos.org/pipermail/centos-announce/) 及 [CentOS CR announce archives ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://lists.centos.org/pipermail/centos-cr-announce/)。如需漏洞的相關資訊，請參閱 [CentOS 上的套件漏洞](#va_centos)。|
  |Debian|[Debian security announcements ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://lists.debian.org/debian-security-announce/)。|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Security Data API ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://access.redhat.com/labsinfo/securitydataapi)。|
  |Ubuntu|[Ubuntu Security Notices ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://usn.ubuntu.com/)。|
  {: caption="表 1. Vulnerability Advisor 檢查套件是否有漏洞的受支援 Docker 基礎映像檔" caption-side="top"}

### 配置問題
{: #app_configurations}

配置問題是指與應用程式設定方式有關的潛在安全問題。許多已報告的問題可以藉由更新 Dockerfile 而修正。
{:shortdesc}

只有在映像檔使用 Vulnerability Advisor 所支援的作業系統時才會掃描映像檔。Vulnerability Advisor 會檢查下列應用程式類型的配置設定：

- MySQL
- NGINX
- Apache

## 檢閱漏洞報告
{: #va_reviewing}

部署映像檔之前，您可以檢閱其 Vulnerability Advisor 報告中，有關所有有漏洞套件及不安全容器或應用程式設定的詳細資料。您也可以檢查映像檔是否與組織原則相容。
{:shortdesc}

如果您未解決任何已發現的問題，那些問題可能會影響使用該映像檔建置之容器的安全。如果未部署 Container Image Security Enforcement，可以繼續使用容器中有安全及配置問題的映像檔。如果已部署 Container Image Security Enforcement 並且針對映像檔在作用中，所有已發現的問題都必須由您的原則豁免，才能從這個映像檔部署容器。

若要在 Container Image Security Enforcement 配置 Vulnerability Advisor 問題的範圍，請參閱[自訂原則](/docs/services/Registry?topic=registry-security_enforce#customize_policies)。
{:tip}

如果您的映像檔不符合組織原則所設定的需求，您必須配置映像檔以符合那些需求，然後才能部署它。如需如何檢視及變更組織原則的相關資訊，請參閱[設定組織豁免原則](#va_managing_policy)。
{:tip}

部署映像檔之後，如果已部署容器掃描器（已淘汰），Vulnerability Advisor 會繼續掃描容器中是否有安全與配置問題。您可以藉由遵循[檢閱容器報告](#va_reviewing_container)中所述的步驟來解決找到的任何問題。

### 使用 GUI 檢閱漏洞報告
{: #va_reviewing_gui}

您可以使用 GUI 來檢閱儲存在 {{site.data.keyword.registrylong_notm}} 名稱空間之 Docker 映像檔的安全。
{:shortdesc}

1. 登入 {{site.data.keyword.cloud_notm}}。
2. 按一下**導覽功能表**圖示，然後按一下 **Kubernetes**。
3. 按一下**登錄**，然後按一下**映像檔**磚。您的映像檔清單和每個映像檔的安全狀態會顯示在**映像檔**表格中。
4. 若要查看以 `latest` 標記之映像檔的報告，請按一下該映像檔的列。**映像檔詳細資料**標籤會開啟，顯示該映像檔的資料。如果儲存庫中沒有任何 `latest` 標籤存在，則會使用最新的映像檔。
5. 如果安全狀態有顯示任何問題，若要進一步了解問題，請按一下**依類型排列的問題**標籤。隨即開啟**漏洞**及**配置問題**表格。

   - **漏洞**表格。顯示每個問題的「漏洞 ID」、該問題的原則狀態、受影響的套件，以及如何解決問題。若要查看該問題的相關資訊，請展開該列。隨即顯示該問題的摘要，其中包含該問題之供應商安全注意事項的鏈結。會列出包含已知漏洞問題的套件。
  
     清單會使用[漏洞類型](#types)所列 Docker 映像檔類型的已發佈安全注意事項每天進行更新。一般而言，為了讓有漏洞的套件通過掃描，需要有包含漏洞修正程式的更新版本套件。相同的套件可能會列出多個漏洞，而在此情況下，單一套件更新可能會更正多個問題。按一下安全注意事項代碼，以檢視套件的相關資訊，以及更新套件的步驟。

   - **配置問題**表格。顯示每個問題的「配置問題 ID」、該問題的原則狀態，以及安全作法。若要查看該問題的相關資訊，請展開該列。隨即顯示該問題的摘要，其中包含該問題之安全注意事項的鏈結。
  
     清單會包含動作的建議，以便您可以採取動作來提高容器安全和任何未受保護之應用程式設定的安全。展開某一列即可檢視如何解決問題。

6. 針對報告中顯示的每個問題完成更正動作，然後重建映像檔。

### 使用 CLI 檢閱漏洞報告
{: #va_registry_cli}

您可以使用 CLI 來檢閱儲存在 {{site.data.keyword.registrylong_notm}} 名稱空間之 Docker 映像檔的安全。
{:shortdesc}

1. 列出 {{site.data.keyword.cloud_notm}} 帳戶中的映像檔。系統會傳回所有映像檔的清單，與其儲存所在的名稱空間無關。

   ```
    ibmcloud cr image-list
    ```
   {: pre}

2. 檢查**安全狀態**直欄中的狀態。
    - `沒有問題`：找不到任何安全問題。
    - `<X> 個問題` 找到的潛在安全問題或漏洞數，其中 `<X>` 是問題的數目。
    - `掃描中`：正在掃描映像檔，尚未判斷最後的漏洞狀態。

3. 若要檢視狀態的詳細資料，請檢閱 Vulnerability Advisor 報告：

   ```
    ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
   {: pre}

   在 CLI 輸出中，您可以檢閱下列有關配置問題的資訊。
      - `安全作法`：所找到之漏洞的說明
      - `更正動作`：有關如何修正漏洞的詳細資料

### CentOS 上的套件漏洞
{: #va_centos}

如果您使用 CentOS，可能會在報告中得到誤肯定，亦即，報告可能會在沒有漏洞時回報有漏洞。這個狀況發生在 Red Hat 發表安全注意事項，但安全注意事項不適用於 CentOS，或是修正程式尚未移植到 CentOS 時。
{:shortdesc}

如果您收到的報告指出您的套件有漏洞，請完成下列步驟：

1. 按一下安全注意事項代碼，以檢視更新套件的步驟。
2. 藉由完成套件更新步驟來更新套件。
3. 如果已更新套件，則結果不是誤肯定，且必要的動作已完成。
4. 如果套件因為沒有較新的版本可供安裝而未更新，則結果是誤肯定。您可以為這個安全注意事項新增豁免原則，請參閱[設定組織豁免原則](#va_managing_policy)。

## 設定組織豁免原則
{: #va_managing_policy}

如果您想要管理 {{site.data.keyword.cloud_notm}} 組織的安全，可以使用您的原則設定來決定是否豁免問題。您可以使用 Container Image Security Enforcement，確保在為您的原則豁免的問題負責之後，只允許來自不包含任何安全問題之映像檔的部署。
{:shortdesc}

您可以從任何映像檔部署容器，而不論安全狀態為何，除非已在您的叢集部署 Container Image Security Enforcement。若要找出如何部署 Container Image Security Enforcement，請參閱[安裝安全強制執行](/docs/services/Registry?topic=registry-security_enforce#security_enforce)。

當您使用 Container Image Security Enforcement 時，Vulnerability Advisor 所偵測到的任何安全問題都會導致無法從映像檔部署容器。若要容許部署具有所偵測到問題的映像檔，必須在您的原則新增豁免。

### 使用 GUI 設定組織豁免原則
{: #va_managing_policy_gui}

如果您想要使用 GUI 設定原則的豁免，請完成下列步驟：

1. 登入 {{site.data.keyword.cloud_notm}}。您必須登入才能在 GUI 看到 Vulnerability Advisor。
2. 按一下**導覽功能表**圖示，然後按一下 **Kubernetes**。
3. 在 **Vulnerability Advisor** 下，按一下**原則設定**。
4. 按一下**建立豁免**。
5. 選取問題類型。
6. 輸入問題 ID。

   您可以在您的[漏洞報告](#va_reviewing)找到本資訊。**漏洞 ID** 直欄包含要用於 CVE 或安全注意事項問題的 ID；**配置問題 ID** 直欄包含要用於配置問題的 ID。
    {: tip}

7. 選取登錄名稱空間、儲存庫及您想要套用豁免的標籤。
8. 按一下**儲存**。

您也可以編輯與移除豁免，方法是將滑鼠移至相關列，然後按一下**開啟及關閉選項清單**圖示。

### 使用 CLI 設定組織豁免原則
{: #va_managing_policy_cli}

如果您想要使用 CLI 設定原則的豁免，可以執行下列指令：

- 若要建立安全問題的豁免，請執行 [`ibmcloud cr exemption-add`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_add) 指令。
- 若要列出安全問題的豁免，請執行 [`ibmcloud cr exemption-list`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_list) 指令。
- 若要列出您可以豁免的安全問題類型，請執行 [`ibmcloud cr exemption-types`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_types) 指令。
- 若要刪除安全問題的豁免，請執行 [`ibmcloud cr exemption-rm`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_rm) 指令。

如需指令的相關資訊，您可以在執行指令時使用 `--help` 旗標。

## 安裝容器掃描器（已淘汰）
{: #va_install_container_scanner}

容器掃描器已淘汰。
{: deprecated}

容器掃描器讓 Vulnerability Advisor 能回報執行中容器裡，所發現且不存在於容器基礎映像檔的任何問題。如果您沒有對容器進行執行時的修改，則不需要容器掃描器，因為映像檔報告會顯示相同的問題。
{:shortdesc}

若要檢查叢集裡正在執行的即時容器安全狀態，您可以安裝容器掃描器。為了保護您的應用程式，容器掃描器會定期掃描執行中的容器，以便您可以偵測並更正任何新偵測到的漏洞。

您可以設定容器掃描器，針對已指派給所有 Kubernetes 名稱空間之 Pod 的容器，監視其中的漏洞。找到漏洞時，您必須更正映像檔的任何問題，然後重新部署應用程式。容器掃描器只支援從 {{site.data.keyword.registrylong_notm}} 中所儲存映像檔建立的容器。

若要使用容器掃描器，您必須設定許可權，然後設定 [Helm 圖表 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.helm.sh/developing_charts)，並將它與您要用於的叢集相關聯。

### 設定容器掃描器（已淘汰）的服務許可權
{: #va_install_container_scanner_permissions}

容器掃描器已淘汰。
{: deprecated}

容器掃描器需要設定許可權，服務才能運作。
{:shortdesc}

若要設定服務許可權，請完成下列步驟：

1. 登入 {{site.data.keyword.cloud_notm}} CLI 用戶端。如果您有聯合帳戶，請使用 `--sso`。
2. [將您 `kubectl` CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) 的目標設為要在其中使用 Helm 圖表的叢集。
3. 建立容器掃描器的服務 ID 及 API 金鑰，並進行命名：
    1. 若要建立服務 ID，請執行下列指令，其中 `<scanner_serviceID>` 是您所選的服務 ID 名稱。請注意其 **CRN**。

       ```
       ibmcloud iam service-id-create <scanner_serviceID>
       ```
       {: codeblock}

    2. 建立服務 API 金鑰，其中 `<scanner_serviceID>` 是您在前一個步驟中所建立的服務 ID，而 `<scanner_APIkey_name>` 是您所選的掃描器 API 金鑰名稱。

       ```
       ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
       ```
       {: codeblock}
       此時會傳回掃描器 API 金鑰。

       請確定安全儲存您的掃描器 API 金鑰，因為稍後無法再擷取該金鑰。同時也請確定您針對安裝掃描器的每個叢集，都有個別的服務 API 金鑰。
       {: important}

    3. 建立授與 `Writer` 角色的服務原則。

       ```
       ibmcloud iam service-policy-create --resource-type scaningress --service-name container-registry --roles Writer <scanner_serviceID>
       ```
       {: codeblock}

### 配置 Helm 圖表（已淘汰）
{: #va_install_container_scanner_helm}

容器掃描器已淘汰。
{: deprecated}

配置 Helm 圖表，並將它與您要用於的叢集相關聯。
{:shortdesc}

若要配置 Helm 圖表，請完成下列步驟：

1. [在 IBM Cloud Kubernetes Service 設定 Helm](/docs/containers?topic=containers-helm#helm)。如果您使用角色型存取控制 (RBAC) 原則來授與 Tiller 存取權，請確定 Tiller 角色擁有所有名稱空間的存取權。提供 Tiller 角色對所有名稱空間的存取權，可確保容器掃描器可以監看所有名稱空間中的容器。

2. 將 IBM 圖表儲存庫新增至您的 Helm，例如，`ibm`。

   ```
   helm repo add ibm https://icr.io/helm/ibm
   ```
   {: pre}

3. 將容器掃描器 Helm 圖表的預設配置設定儲存在本端 YAML 檔案中。將圖表儲存庫（例如，`ibm`）併入 Helm 圖表路徑。

   ```
   helm inspect values ibm/ibmcloud-container-scanner > config.yaml
   ```
   {: pre}

4. 編輯 `config.yaml` 檔案。

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
   <caption>表 2. 瞭解 YAML 檔案元件</caption>
   <thead>
   <th>欄位</th>
   <th>值 </th>
   </thead>
   <tbody>
   <tr>
   <td><code>EmitURL</code></td>
   <td>將 <code>&lt;regional_emit_URL&gt;</code> 取代為 Vulnerability Advisor 地區端點 URL。若要取得 URL，請執行 <code>ibmcloud cr info</code>，並擷取 <strong>Container Registry</strong> 位址。例如，<code>https<span comment="make the link not a link">://us.</span>icr.io</code>。請在此位址結尾加上 <code>/va</code>。例如，<code>https<span comment="make the link not a link">://us.</span>icr.io/va</code>。如需地區的相關資訊，請參閱[本端地區](/docs/services/Registry?topic=registry-registry_overview#registry_regions_local)。</td>
   </tr>
   <tr>
   <td><code>AccountID</code></td>
   <td>將 <code>&lt;IBM_Cloud_account_ID&gt;</code> 取代為您叢集所在的 {{site.data.keyword.cloud_notm}} 帳戶 ID。若要取得帳戶 ID，請執行 <code>ibmcloud account list</code>。</td>
   </tr>
   <tr>
   <td><code>ClusterID</code></td>
   <td>將 <code>&lt;cluster_ID&gt;</code> 取代為您要在其中安裝容器掃描器的 Kubernetes 叢集。若要列出叢集 ID，請執行 <code>ibmcloud ks clusters</code>。<br> **提示**：請使用叢集的 ID，而不要使用名稱。
   </td>
   </tr>
   <tr>
   <td><code>APIKey</code></td>
   <td>將 <code>&lt;scanner_APIkey&gt;</code> 取代為您稍早建立的掃描器 API 金鑰。</td>
   </tr>
   </tbody></table>

5. 使用已更新的 `config.yaml` 檔案，將 Helm 圖表安裝至叢集。更新的內容會儲存在您圖表的 ConfigMap 中。將 `<myscanner>` 取代為您所選的 Helm 圖表名稱。將圖表儲存庫（例如，`ibm`）併入 Helm 圖表路徑。

   ```
   helm install -f config.yaml --name=<myscanner> ibm/ibmcloud-container-scanner
   ```
   {: pre}

   容器掃描器已安裝至 `kube-system` 名稱空間，但會掃描所有名稱空間中的容器。
    {:tip}

6. 檢查圖表部署狀態。圖表已備妥時，**狀態**欄位會有 `DEPLOYED` 值。

   ```
    helm status <myscanner>
    ```
   {: pre}

7. 部署圖表之後，請驗證已使用 `config.yaml` 檔案中的已更新設定。

   ```
    helm get values <myscanner>
    ```
   {: pre}

現在，已安裝容器掃描器，且代理程式已部署為您叢集中的 [DaemonSet ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)。雖然容器掃描器已部署至 `kube-system` 名稱空間，但是它仍會掃描指派至所有 Kubernetes 名稱空間中 Pod 的所有容器，例如，`default`。

## 從防火牆背後執行容器掃描器（已淘汰）
{: #va_firewall}

容器掃描器已淘汰。
{: deprecated}

如果您的防火牆會封鎖送出的連線，您必須配置防火牆。
{:shortdesc}

若要配置防火牆，以容許工作者節點存取 IP 位址的 TCP 埠 `443` 上的容器掃描器，請參閱[容許叢集透過公用防火牆存取基礎架構資源和其他服務](/docs/containers?topic=containers-firewall#firewall_outbound)中的步驟 3。

## 檢閱容器報告（已淘汰）
{: #va_reviewing_container}

容器掃描器已淘汰。
{: deprecated}

在儀表板中，您可以查看容器的狀態，來判斷其安全是否符合組織的原則。您也可以檢閱容器的安全報告，其中會詳述任何有漏洞的套件及未受保護的容器或應用程式設定，以及容器是否遵循組織原則。
{:shortdesc}

確認在您空間中執行的容器能繼續遵守組織原則，方法是檢閱**原則狀態**欄位。即會以下列其中一種狀況顯示狀態：

- `遵循原則`：找不到任何安全或配置問題。
- `未遵循原則`：Vulnerability Advisor 找到潛在的安全或配置問題，這些問題導致容器未遵循原則。如果您的組織原則允許部署有漏洞的映像檔，映像檔可能會以 `Deploy with Caution` 狀態部署，並且傳送警告給部署它的使用者。
- `不完整的評量`：掃描未完成。掃描可能仍在執行中，或該容器實例的作業系統可能不相容。

確認您的容器盡可能安全，方法是完成下列步驟以檢視其安全報告，並處理任何已報告的安全或配置問題：

1. 選取您要檢視其報告的容器：
    1. 按一下**導覽功能表**圖示，然後按一下 **Kubernetes**。
    2. 按一下**登錄**，然後按一下**儲存庫**磚，然後展開您要的儲存庫列。
    3. 選取您要的映像檔列。
    4. 選取**相關聯的容器**標籤，然後選取您要的容器列。安全報告會開啟。
2. 檢閱各區段以查看映像檔中每個套件的可能安全及配置問題：

    - **漏洞**：列出包含已知漏洞問題的套件。清單會使用[漏洞類型](#types)所列 Docker 映像檔類型的已發佈安全注意事項每天進行更新。一般而言，為了讓有漏洞的套件通過掃描，需要有包含漏洞修正程式的更新版本套件。相同的套件可能會列出多個漏洞，而在此情況下，單一套件更新可能會更正多個問題。按一下安全注意事項代碼，以檢視套件的相關資訊，以及更新套件的步驟。

    - **配置問題**：列出建議，以便您可以採用以提高容器安全和任何未受保護之應用程式設定的安全。展開某一列即可檢視如何解決問題。

   會針對每一個列出的項目，提供更正動作或建議。

3. 檢閱每個安全問題的原則狀態。原則狀態指出此問題是否得到豁免。

    - `作用中`：您的問題未得到豁免，問題會影響您的安全狀態。
    - `豁免`：此問題已由原則設定豁免。
    - `局部豁免`：此問題與多個安全注意事項相關聯。安全注意事項未全部得到豁免。

4. 決定如何更新容器，以解決問題。

    若要修正容器映像檔的問題，您必須刪除舊實例然後重新部署，這表示會遺失現有容器內的任何資料。請確定您已充份瞭解容器架構，以選擇適當的容器重新部署方法。
    {: important}

    **範例**

    - 如果您的容器與它運算的資料分開，您可以停止容器並刪除它、對映像檔進行必要的變更，然後重新部署，而不會流失任何資料。
    - 您可以使用 {{site.data.keyword.cloud_notm}} 服務，例如 [Delivery Pipeline](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-deliverypipeline_about#deliverypipeline_about)，以便協助更新有漏洞的容器實例。
    - 在微服務架構中，您可以在修正安全或配置問題時將資料流量遞送到另一個容器實例，然後以紅黑部署方式推送新的映像檔。

5. 如果您現在無法修正問題，可以在原則設定中豁免該問題，如此可避免問題阻擋容器的部署。若要豁免問題，請按一下**開啟及關閉選項清單**圖示，然後按一下**建立豁免**。請參閱[設定組織豁免原則](#va_managing_policy)。

6. 修正**安全**報告中說明的問題，然後根據您選擇的方法重建映像檔或重新部署容器。
