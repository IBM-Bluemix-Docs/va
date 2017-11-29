---

copyright:
  years: 2017
lastupdated: "2017-10-26"

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

「漏洞警告器」會在部署之前檢查容器映像檔的安全狀態。
{:shortdesc}


## 關於漏洞警告器 
{: #about}

「漏洞警告器」為 {{site.data.keyword.containerlong}} 提供安全管理。「漏洞警告器」會產生安全狀態報告、建議修正程式及最佳作法，以及提供管理以限制不安全的映像檔執行。修正「漏洞警告器」所報告的安全及配置問題，有助於您保護 {{site.data.keyword.cloud_notm}} 基礎架構。
{:shortdesc}

「漏洞警告器」包含下列特性：

-   掃描映像檔是否有漏洞
-   根據安全標準（例如 ISO 27002）以及 {{site.data.keyword.containerlong_notm}} 特有的安全作法，提供評估報告
-   偵測以檔案為基礎的惡意軟體
-   提供建議以保護部分應用程式類型的配置檔
-   提供如何修正已報告之漏洞或其報告中之配置問題的指示

<dl>
  <dt><strong>有漏洞的套件</strong></dt>
  <dd>「漏洞警告器」會檢查以支援作業系統為基礎的映像檔中有漏洞的套件，並提供關於漏洞之任何相關安全注意事項的鏈結。「漏洞警告器」會每天針對這些安全注意事項更新其內部清單。下表說明支援的基礎映像檔。</dd>
</dl>

  |Docker 基礎映像檔|安全注意事項的來源|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://git.alpinelinux.org/) 及 [CIRCL CVE Search ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cve.circl.lu/)|
  |CentOS| [CentOS 公告保存 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://lists.centos.org/pipermail/centos-announce/) 及 [CentOS CR 公告保存 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian 安全公告 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ubuntu.com/usn/)|
  {: caption="表 1. 「漏洞警告器」檢查套件是否有漏洞的 Docker 基礎映像檔" caption-side="top"}










## 使用 CLI 檢閱儲存在 {{site.data.keyword.registrylong_notm}} 名稱空間之 Docker 映像檔的映像檔安全 
{: #va_registry_cli}

您可以檢閱儲存在名稱空間之 Docker 映像檔的安全，以找到可能漏洞的相關資訊。
{:shortdesc}

當您新增映像檔至 {{site.data.keyword.registrylong_notm}} 時，「漏洞警告器」會自動掃描映像檔，以偵測安全問題和可能的漏洞。從有漏洞的映像檔部署的容器可能會受到攻擊並受損。只有在映像檔是以「漏洞警告器」所支援之作業系統為基礎時才會掃描映像檔。

「漏洞警告器」會檢查下列漏洞。如果找到安全問題，會提供指示以協助修正報告的漏洞。

若要檢查您 {{site.data.keyword.Bluemix_notm}} 帳戶中的映像檔漏洞狀態，請完成下列步驟。

1.  列出 {{site.data.keyword.Bluemix_notm}} 帳戶中的所有映像檔。下列指令會傳回所有映像檔的清單，並且與它們儲存所在的名稱空間無關。

    ```
    bx cr image-list
    ```
    {: pre}

2.  檢查「漏洞狀態」直欄中的狀態。會顯示下列其中一個狀態：
    -   正常。這個狀態表示沒有找到任何安全問題。
    -   有漏洞。這個狀態表示找到可能的安全問題或漏洞。
    -   不明。當映像檔正在掃描時會顯示這個狀態，直到能判斷最後漏洞狀態為止。
    -   不受支援的 OS。如果映像檔不支援由「漏洞警告器」加以掃描，則會顯示這個狀態。
4.  若要找出狀態的相關資訊，請檢閱「漏洞警告器」報告。

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    在您的 CLI 輸出中，您可以尋找有漏洞套件的清單、已找到之漏洞的說明，以及修正方法的鏈結。


## 解決映像檔中的問題 
{: #va_report}

「漏洞警告器」報告的一般問題修正程式範例。
{:shortdesc}

「漏洞警告器」提供更正動作與報告的安全或配置問題。部分的報告問題可以藉由更新 Dockerfile 而修正。為了協助您更快速地解決一般問題，請使用下列程式碼範例在您的 Dockerfile 中實作解決方案。

### 密碼有效期上限、密碼有效期下限，及密碼長度下限
{: #va_password}

問題：您收到下列錯誤之一（或全部）：

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

修正程式：藉由將下列程式碼新增至 Dockerfile 而設定密碼相符性。

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### SSH 漏洞 
{: #ssh}

問題：傳回下列錯誤：

```
SSH server should not be installed.
```
{: screen}

修正程式：請不要使用 SSH，而是使用 `docker attach` 或 `docker exec` 來存取您的容器。請確定您的 Dockerfile 未包含任何安裝 SSH 伺服器的步驟。

