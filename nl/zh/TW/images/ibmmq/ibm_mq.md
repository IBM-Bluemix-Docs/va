---

copyright:
  years: 2015, 2017

lastupdated: "2017-03-07"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# IBM MQ Bluemix Container 映像檔
{: #ibm_mq}

ibm-mq 映像檔是提供給 {{site.data.keyword.containershort}}。其中包括 {{site.data.keyword.IBM}} MQ Advanced for Developers（無保固）。 

{:shortdesc}


## 如何運作
{: #ibm_mq_how}

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers 包含您開始開發自己的傳訊解決方案及 {{site.data.keyword.IBM_notm}} MQ 應用程式所需的一切。 

若要在 {{site.data.keyword.Bluemix_notm}} 中開發傳訊解決方案及 {{site.data.keyword.IBM_notm}} MQ 應用程式，您可以在 {{site.data.keyword.containershort}} 中部署 ibm-mq Docker 映像檔。然後，您可以建立及執行佇列管理程式，並使用 Web 使用者介面或終端機來配置它們，以符合您的傳訊解決方案或應用程式需求。

**附註：**您只能使用 ibm-mq 映像檔進行開發及單元測試。您也可以使用映像檔來探索產品、透過指導教學學習，以及評估 {{site.data.keyword.IBM_notm}} MQ 對您組織的貢獻。

## 包含的內容
{: #ibm_mq_what}

此 ibm-mq 映像檔包含 {{site.data.keyword.IBM_notm}} MQ Advanced for Developers 9.0.x 版的套裝軟體，這個版本是可用於開發及單元測試的產品完整功能版本。您可以免費下載此版本，並根據授權條款自由使用。

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers 適用於 Windows 64 位元作業系統及 Linux on x86-64 作業系統。下載套件中包含所有產品必備項目。如需相關資訊，請參閱 [IBM MQ 下載 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](http://www-03.ibm.com/software/products/lv/ibm-mq-advanced-for-developers){: new_window}

如需 {{site.data.keyword.IBM_notm}} MQ Advanced for Developers 內含特性的相關資訊，請參閱 [IBM MQ ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}



## 使用限制
{: #ibm_mq_usage}

下表顯示適用於在 {{site.data.keyword.Bluemix_notm}} 中免費使用 ibm-mq 映像檔的限制：

| 環境 | 免費使用限制 |
|-------------|-------------------------|
| 開發	| 無限制免費使用 ibm-mq 映像檔來建立用於開發的單一容器。|
| 測試 | 無限制免費使用 ibm-mq 映像檔來建立用於單元測試的單一容器。如需允許哪些測試的相關資訊，請查閱[授權資訊](getstarted.html#ibm_mq_license )。|
| 正式作業 | {{site.data.keyword.IBM_notm}} MQ Advanced for Developer 授權不容許它用於正式作業。|

**附註：**ibm-mq 映像檔的定價與 {{site.data.keyword.Bluemix_notm}} 中所使用容器的定價無關。


## 授權資訊
{: #ibm_mq_license}

授權的相關資訊：

*	Dockerfiles 及相關聯的 Script 是根據 [Apache License 2.0 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.apache.org/licenses/LICENSE-2.0.html){: new_window} 進行授權。
* [{{site.data.keyword.IBM_notm}} MQ Advanced for Developers ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示") 的授權資訊](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/973FA648A5DE42948525806E004CC757?OpenDocument){: new_window}。

**附註：**授權不容許進一步配送。映像檔中 {{site.data.keyword.IBM_notm}} MQ 的條款限制開發及單元測試的使用。


## 開始使用
{: #ibm_mq_get_started}

請完成下列步驟，以在 {{site.data.keyword.Bluemix_notm}} 上執行的容器中執行及管理 {{site.data.keyword.IBM_notm}} MQ：

1. [佈建以 ibm-mq 映像檔為基礎的容器](ibm_mq.html#ibm_mq_provision)

    **附註：**容器關聯於其中「佇列管理程式」的執行中狀態。如果您使用主控台或指令行來停止「佇列管理程式」，則容器會在不久之後停止。 

2. 管理正在容器中執行的 {{site.data.keyword.IBM_notm}} MQ 資源。您可以從 IBM MQ Web 主控台中，以圖形方式來執行，或從終端機中使用指令，以程式設計方式來執行。 

    1. 選擇下列任一選項來管理容器中的 {{site.data.keyword.IBM_notm}} MQ 資源：
    
        *	啟動 {{site.data.keyword.IBM_notm}} MQ Web 主控台，並以圖形方式管理 {{site.data.keyword.IBM_notm}} MQ。如需相關資訊，請參閱[啟動 IBM MQ Web 主控台](ibm_mq.html#ibm_mq_webui)。
        *	從終端機中，登入 Docker CLI。如需相關資訊，請參閱[設定終端機以使用 Docker CLI](ibm_mq.html#ibm_mq_docker_commands)。
        * 從終端機中，使用 IBM Bluemix Container Service CLI。如需相關資訊，請參閱[設定終端機以使用 IBM Bluemix Container Service CLI](ibm_mq.html#ibm_mq_containers_cli)。
        
    2. 配置在容器中執行的「{{site.data.keyword.IBM_notm}} MQ 佇列管理程式」。 

        * 若要透過指令行管理「佇列管理程式」，請使用 *runmqsc* 程式。請執行下列指令：`runmqsc QM_Name`，其中 *QM_Name* 是「佇列管理程式」的名稱。如需相關資訊，請參閱[透過指令行管理容器](ibm_mq.html#ibm_mq_access)。
    
        * 若要以圖形方式管理「佇列管理程式」，請使用 Web 使用者介面。
        
    3. 遵循 [IBM MQ Knowledge Center ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window} 上的指示，在「佇列管理程式」上建立「訊息佇列」，然後將訊息放置到其中或從中取得訊息。
    
        例如，在 `runmqsc` 中執行下列指令，以建立新的「本端佇列」：`DEFINE QLOCAL('MY.Q')`，其中 *MY.Q* 是終端機中「本端佇列」的名稱。您可以在 Web 主控台中檢視佇列，也可以在 runmqsc 中執行下列指令，來顯示佇列及其內容：`DISPLAY QLOCAL('MY.Q')`。
    
3. [選用項目] 監視容器內的 {{site.data.keyword.IBM_notm}} MQ 日誌。如需相關資訊，請參閱[透過指令行管理容器](ibm_mq.html#ibm_mq_access)。

如需 IBM MQ 開始使用簡介的相關資訊，請參閱[IBM MQ 9.0.x 版歡迎使用頁面 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}

如需映像檔以及如何在 Docker 本端建置專屬映像檔的相關資訊，請參閱 [MQ Docker GitHub 頁面 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/mq-docker){: new_window}
    
如需 IBM MQ 的最新部落格和資訊，請參閱 [MQ DeveloperWorks 部落格 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/developerworks/community/blogs/messaging){: new_window}


### 佈建以 ibm-mq 映像檔為基礎的容器
{: #ibm_mq_provision}

請完成下列步驟，以在 {{site.data.keyword.Bluemix_notm}} 中佈建 Docker 容器，該 Docker 容器是以 {{site.data.keyword.IBM_notm}} 所提供的 ibm-mq 映像檔為基礎：

1.	登入 {{site.data.keyword.Bluemix_notm}}。請使用您的 {{site.data.keyword.Bluemix_notm}} ID 登入。
2.	從型錄中，選取**容器**，然後選擇 *ibm-mq* 映像檔。
3.	選取**單一**，以建立可用於進行開發及測試的單一實例容器。
4.	輸入容器的名稱；例如 mq。
5.	選取容器的大小。
6.	在「公用 IP 位址」欄位中，選取**要求並連結公用 IP**。
7.	在「公用埠」欄位中，指定 **1414/tcp, 9443/tcp**。
8.	展開「進階」選項，按一下**新增環境變數**，然後輸入下列環境變數：LICENSE 及 MQ_QMGR_NAME。

    * 將 **LICENSE** 的值設為 **accept**，以同意 IBM MQ Advanced for Developers 授權條款。 
    
        如果想要在接受授權之前先檢視授權，您可以將 LICENSE 設為 "view"，授權即會列印在容器日誌中。 
    
        如果英文不是您的第一語言，您可以藉由將稱為 **LANG** 的另一個環境變數設為下列其中一個值來檢視不同語言版本的授權檔：*zh_TW* 表示繁體中文、*zh* 表示簡體中文、*cs* 表示捷克文、*en* 表示英文、*fr* 表示法文、*de* 表示德文、*el* 表示希臘文、*id* 表示印尼文、*it* 表示義大利文、*ja* 表示日文、*ko* 表示韓文、*lt* 表示立陶宛文、*pl* 表示波蘭文、*pt* 表示葡萄牙文、*ru* 表示俄文、*sl* 表示斯洛維尼亞文、*es* 表示西班牙文，或 *tr* 表示土耳其文。

    * 使用值 *MYMQ* 來設定 **MQ_QMGR_NAME** 的值，其中 *MYMQ* 是所選擇的「佇列管理程式」名稱。
 
        **附註：**如果您未指定環境變數 LICENSE 及 MQ_QMGR_NAME 的值，則容器會建立，但不會啟動。 
    
    * [選用項目] 依預設，會在容器中啟動「{{site.data.keyword.IBM_notm}} MQ Web 主控台」。若要變更預設行為，請將 **MQ_DISABLE_WEB_CONSOLE** 環境變數值設為 *true*。
    
        **附註：**若要使用 Web 主控台，請不要設定 *MQ_DISABLE_WEB_CONSOLE* 環境變數。

    * [選用項目] 若要透過 {{site.data.keyword.Bluemix_notm}} 使用者介面來查看 MQ 日誌，請將環境變數 **LOG_LOCATIONS** 設為值 `/var/mqm/qmgr/QM_Name/errors/AMQERR01.LOG`，其中 *QM_Name* 是您針對環境變數 *MQ_QMGR_NAME* 設定的值。
    
    *	[選用項目] 依預設，會建立 {{site.data.keyword.IBM_notm}} MQ Developer 預設物件。若要變更預設行為，請將 **MQ_DEV** 環境變數設為 *false*，並使用自訂值來修改 IBM MQ 的預設安裝值。如需相關資訊，請參閱[修改預設安裝值](ibm_mq.html#ibm_mq_dev_default)
 
9. [選用項目] 在「進階選項」區段中，指派一個磁區。請使用 {{site.data.keyword.Bluemix_notm}} 磁區，透過容器移轉持續儲存 {{site.data.keyword.IBM_notm}} MQ 資料及配置。如果終止容器，將會失去磁區中未儲存的 {{site.data.keyword.IBM_notm}} MQ 資料。

    ibm-mq 映像檔可以與 {{site.data.keyword.Bluemix_notm}} 磁區搭配使用，不過，磁區必須裝載至 **/mnt/mqm**，{{site.data.keyword.IBM_notm}} MQ 才能偵測及使用這些磁區。

    依預設，在容器停止時，任何在沒有磁區情況下建立的容器都將失去所有 IBM MQ 資料。如果要避免此狀況，您必須使用持續傳訊及「{{site.data.keyword.Bluemix_notm}} 磁區」。

    1. 按一下**建立磁區**，輸入磁區名稱，然後選取檔案共用。
    2. 按一下**指派現有磁區**，然後選取所建立的磁區。請在路徑方框中輸入 **/mnt/mqm**。請確定未勾選**唯讀**方框。

    **附註：**在裝載磁區以用來作為 IBM MQ 資料目錄時，您必須將它裝載至位置 */mnt/mqm*。由將它裝載至 */mnt/mqm*，ibm-mq Script 將偵測磁區，並配置 IBM MQ 檔案系統來使用它。
     
10.	按一下**建立**，然後等待容器開始執行。

    在建立容器之後，即會開啟容器儀表板。請檢查容器的狀態已設為**執行中**。 

    若要檢查容器的日誌，請按一下**監視及日誌**，然後選取**記載**標籤。畫面上會顯示日誌。如需日誌的進階分析，請按一下**進階視圖**，以在 Kibana 中顯示日誌。

您已建立並啟動佇列管理程式。您現在可以使用下列連線詳細資料來連接 MQ 應用程式：

| 連線內容 | 值 |
|-------|-------|
| IP 位址 | 容器的公用 IP 位址 |
| 埠 | 1414 |
| 通道 | DEV.APP.SVRCONN |


### 透過指令行管理容器
{: #ibm_mq_access}

在執行 {{site.data.keyword.IBM_notm}} MQ 的容器部署至 {{site.data.keyword.Bluemix_notm}} 之後，您可以存取容器及容器的狀態，並驗證 {{site.data.keyword.IBM_notm}} MQ 安裝。

完成下列步驟，以驗證容器中 {{site.data.keyword.IBM_notm}} MQ 的設定及配置：

1.	設定 IBM Containers CLI。如需相關資訊，請參閱 [Bluemix CLI 頁面](https://plugins.ng.bluemix.net/ui/home.html)，並遵循指示來安裝最新的 Cloud Foundry Container 服務外掛程式。

2.	從終端機中，使用 {{site.data.keyword.Bluemix_notm}} ID 登入 {{site.data.keyword.Bluemix_notm}} 組織以及容器執行所在的空間。請執行下列指令：

    1. `bx login`
    2. `bx ic init`
    
3.	檢查容器的狀態。請執行下列指令：`bx ic ps`

4.	將 Bash 階段作業附加至容器：

    `bx ic exec -it container_name /bin/bash`
    
    其中 container_name 是容器的名稱
    
5.	執行指令來管理 MQ 設定，例如：

| 指令 | 動作 |
|---------|---------|
| dspmqver | 驗證 IBM MQ 版本及建置資訊。|
| dspmq | 檢查在容器中執行的「佇列管理程式」的狀態。| 
| runmqsc | 管理 MQ 資源。|

若要監視 MQ 日誌，請選擇下列其中一個選項：

* 如果終端機是配置為執行 {{site.data.keyword.containershort}} CLI 指令，請執行下列指令：`bx ic exec container_id tail -f /var/mqm/qmgr/QM_ Name/errors/AMQERR01.LOG`，其中 *container_id* 是容器的名稱，而 *QM_ Name* 是「佇例管理程式」的名稱。
    
* 如果終端機是配置為執行 Docker CLI 指令，請執行下列指令：`docker exec container_id tail -f /var/mqm/qmgr/QM_Name/errors/AMQERR01.LOG`，其中 *container_id* 是容器的名稱，而 *QM_Name* 是「佇列管理程式」的名稱。
    
    

### 啟動 IBM MQ Web 主控台
{: #ibm_mq_webui}

請完成下列步驟，以啟動 {{site.data.keyword.IBM_notm}} MQ Web 主控台：

1.	在 {{site.data.keyword.Bluemix_notm}} 儀表板中，選取容器，並檢查容器的狀態已設為*執行中*。
2.	驗證容器已配置公用埠 9443。
3.	檢查容器詳細資料，以尋找「公用 IP」或要求，如果尚未完成，請連結一個。
4.	從 Web 瀏覽器中，啟動下列 URL：`https://DockerContainerPublicIP:9443/ibmmq/console/`，其中 DockerContainerPublicIP 是容器的公用 IP。

    這時會開啟安全性警告，因為已產生並自行簽署 Web 伺服器在管理 MQ 主控台時使用的 TLS 憑證。請按一下**新增異常狀況**並確認。

    **附註：**在正式作業環境中，使用瀏覽器所辨識及信任的專屬憑證。此憑證應該是由憑證管理中心發出。 
    
    瀏覽器會開啟 Web 使用者介面。
    
5. 使用認證登入 {{site.data.keyword.IBM_notm}} MQ 主控台：

    1.	使用者：admin
    2.	密碼：passw0rd
    
您現在已備妥可以管理 IBM MQ。

### 設定終端機以使用 IBM Bluemix Container Service CLI
{: #ibm_mq_containers_cli}

請使用 {{site.data.keyword.containershort}} CLI，直接在容器中執行 {{site.data.keyword.IBM_notm}} MQ 管理指令。

請完成下列步驟，以設定終端機執行 bx ic 指令來管理 IBM MQ：

1.	設定 {{site.data.keyword.containershort}} CLI。如需相關資訊，請參閱 [Bluemix CLI 頁面](https://plugins.ng.bluemix.net/ui/home.html)，並遵循指示來安裝最新的 Cloud Foundry Container 服務外掛程式。

2.	從終端機中，登入 {{site.data.keyword.Bluemix_notm}}。請執行下列指令：

    1. `bx login`
    2. `bx ic init`
    
3.	將 Bash 階段作業附加至容器：

    `bx ic exec -it container_name /bin/bash`
    
    其中 container_name 是容器的名稱
    


### 設定終端機以使用 Docker CLI
{: #ibm_mq_docker_commands}

請使用 Docker CLI，直接在容器中執行 {{site.data.keyword.IBM_notm}} MQ 管理指令。

請完成下列步驟，以設定終端機執行 Docker 指令來管理 IBM MQ：

1.	設定 {{site.data.keyword.containershort}} CLI。如需相關資訊，請參閱 [Bluemix CLI 頁面](https://plugins.ng.bluemix.net/ui/home.html)，並遵循指示來安裝最新的 Cloud Foundry Container 服務外掛程式。

2.	從終端機中，登入 {{site.data.keyword.Bluemix_notm}}。請執行下列指令：

    1. `bx login`
    2. `bx ic init`
    3. 複製針對下列環境變數所提供的值：DOCKER_HOST、DOCKER_CERT_PATH 及 DOCKER_TLS_VERIFY。
    4. 設定下列變數來連接至 {{site.data.keyword.containershort}}，以置換本端 Docker 環境：
        
        `export DOCKER_HOST=<your_host_value>`
        
        `export DOCKER_CERT_PATH=<your_cert_path_value>`
        
        `export DOCKER_TLS_VERIFY=1`

3.	將 Bash 階段作業附加至容器：

    `bx ic exec -it container_name /bin/bash`
    
    其中 container_name 是容器的名稱
    
**附註：**此選項只支援部分 Docker 指令。




### 修改預設安裝值
{: #ibm_mq_dev_default}

在建立佇列管理程式時，將使用 {{site.data.keyword.IBM_notm}} MQ Developer 預設值建立該佇列管理程式。這些預設物件的設計旨在協助您快速開始開發應用程式，甚至可以試用 {{site.data.keyword.IBM_notm}} MQ。 

系統會使用下列預設物件建立「佇列管理程式」：

* 密碼為 *passw0rd* 的「MQ 管理者」使用者 **admin**。
*	沒有密碼的「MQ 應用程式」使用者 **app**。
*	配置為連結至埠 1414/TCP 的接聽器 **DEV.LISTENER.TCP**。
*	配置為處理 MQ 用戶端應用程式連線的通道 **DEV.APP.SVRCONN**。
*	配置為處理 MQ 管理連線的通道 **DEV.ADMIN.SVRCONN**。
*	您可以在其中放置訊息的 3 個佇列：**DEV.QUEUE.1**、**DEV.QUEUE.2**、**DEV.QUEUE.3**。
*	將在其中放置無法遞送之訊息的無法傳送的郵件佇列 **DEV.DEAD.LETTER.QUEUE**。
*	主題字串為 */dev* 的基本主題 **DEV.BASE.TOPIC**。
*	一筆「通道鑑別」記錄，其會封鎖所有用戶端連線，除非它們是透過通道 **DEV.APP.SVRCONN** 或 **DEV.ADMIN.SVRCONN** 進行連接。
*	一筆「通道鑑別」記錄，其會封鎖所有管理使用者連接，但透過 **DEV.ADMIN.SVRCONN** 通道進行的連接除外。
*	一筆「通道鑑別」記錄，其只容許使用者管理者連接至 **DEV.ADMIN.SVRCONN** 通道。
*	連線鑑別設為需要管理應用程式在連接時傳遞有效的認證，並採用該使用者 ID 進行權限檢查。

您可以使用這些預設物件來連接用戶端應用程式，這個用戶端應用程式可將訊息放置到佇列以及從中取得訊息，或者訂閱主題及發佈至主題。

如果想要自訂所建立的開發人員預設值，在您從 ibm-mq 映像檔建立容器時，可以新增下列環境變數。

| 環境變數 | 用途 | 預設值 |
|----------------------|---------|---------|
| MQ_ADMIN_PASSWORD | 設定此變數，可將建立的 MQ 管理者的密碼從預設值變更為您自己選擇的值。密碼長度必須至少為 8 個字元。管理使用者的使用者名稱固定為 admin。| passw0rd |
| MQ_APP_PASSWORD | 設定此變數，可設定應用程式使用者的密碼。應用程式使用者的使用者名稱固定為 app。如果您設定此環境變數，則在連接 MQ 用戶端時必須提供使用者 ID 及密碼。<br> 密碼長度必須至少為 8 個字元。|  |
| MQ_TLS_KEYSTORE | 將此環境變數設為 PKCS#12 金鑰儲存庫的位置，該金鑰儲存庫包含您要「MQ Web 主控台」及「IBM MQ 佇列管理程式」用於進行 TLS 作業的憑證。<br> 在設定此環境變數時，將使用 CipherSpec 'TLS_RSA_WITH_AES_256_GCM_SHA384'，針對 TLS 啟用建立的開發人員通道。<br> **附註：**您需要讓金鑰儲存庫檔案可在 {{site.data.keyword.Bluemix_notm}} 容器內使用。若要這麼做，您可以在建立容器時裝載包含金鑰儲存庫的磁區。|  |
| MQ_TLS_PASSPHRASE | 設定此環境變數，可設定環境變數 MQ_TLS_KEYSTORE 中指定之金鑰儲存庫的密碼。|  |
| MQ_DEV | 設定此環境變數，可控制是否建立 {{site.data.keyword.IBM_notm}} MQ Developer 預設物件。| true |
| MQ_DISABLE_WEB_CONSOLE | 如果想要保留容器的 CPU/RAM 用量，請設定此環境變數，以停用 Web 主控台的配置及啟動。|  |



