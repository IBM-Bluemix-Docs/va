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

# 脆弱性アドバイザーを使用したイメージ・セキュリティーの管理 
{: #va_index}

脆弱性アドバイザーは、デプロイメントの前にコンテナー・イメージのセキュリティー状況を検査します。
{:shortdesc}


## 脆弱性アドバイザーの概要 
{: #about}

脆弱性アドバイザーは、{{site.data.keyword.containerlong}} のセキュリティー管理機能を提供します。脆弱性アドバイザーは、セキュリティー状況レポートを生成し、修正やベスト・プラクティスを提案し、非セキュアなイメージが実行されるのを制限するための管理機能を提供します。脆弱性アドバイザーによって報告されたセキュリティー問題や構成問題を修正することで、{{site.data.keyword.cloud_notm}} インフラストラクチャーをセキュアに保つことができます。
{:shortdesc}

脆弱性アドバイザーには、以下の機能があります。

-   イメージをスキャンして脆弱性がないか調べる
-   セキュリティー標準 (国際標準化機構 (ISO) 27002 など) および {{site.data.keyword.containerlong_notm}} に固有のセキュリティー・プラクティスに基づいて、評価レポートを提供する
-   ファイル・ベースのマルウェアを検出する
-   アプリケーション・タイプのサブセット用の構成ファイルをセキュアにするために推奨情報を提供する
-   レポートで報告された脆弱性や構成問題の修正方法に関する指示を提供する

<dl>
  <dt><strong>脆弱なパッケージ</strong></dt>
  <dd>脆弱性アドバイザーは、サポートするオペレーティング・システムに基づいたイメージ内の脆弱なパッケージを検査し、その脆弱性に関連したセキュリティー上の注意事項へのリンクを提供します。脆弱性アドバイザーは、こうしたセキュリティー上の注意事項を毎日確認し、内部リストを更新します。サポートされている基本イメージを以下の表に記載します。</dd>
</dl>

  |Docker 基本イメージ|セキュリティー上の注意事項のソース|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://git.alpinelinux.org/) および [CIRCL CVE Search ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://lists.centos.org/pipermail/centos-announce/) および [CentOS CR announce archives ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ubuntu.com/usn/)|
  {: caption="表 1. 脆弱性アドバイザーが脆弱なパッケージについて検査する Docker 基本イメージ" caption-side="top"}










## CLI を使用した {{site.data.keyword.registrylong_notm}} の名前空間に保管されている Docker イメージのイメージ・セキュリティーの確認 
{: #va_registry_cli}

名前空間に保管されている Docker イメージのセキュリティーを検討し、潜在的な脆弱性に関する情報を見つけることができます。
{:shortdesc}

{{site.data.keyword.registrylong_notm}} にイメージを追加すると、イメージは自動的に脆弱性アドバイザーによってスキャンされ、セキュリティー問題や潜在的な脆弱性が検出されます。脆弱なイメージからデプロイされたコンテナーは、アタックされたり、暗号漏えいが生じたりするおそれがあります。イメージがスキャンされるのは、そのイメージが脆弱性アドバイザーによってサポートされるオペレーティング・システムに基づいている場合のみです。

脆弱性アドバイザーは、以下の脆弱性を検査します。セキュリティー問題が検出された場合、報告された脆弱性を修正するための指示が出されます。

{{site.data.keyword.Bluemix_notm}} アカウント内のイメージの脆弱性の状況を検査するには、以下の手順を実行します。

1.  {{site.data.keyword.Bluemix_notm}} アカウント内のすべてのイメージをリストします。次のコマンドは、保管されている名前空間に関係なくすべてのイメージのリストを返します。

    ```
    bx cr image-list
    ```
    {: pre}

2.  「脆弱性の状況 (VULNERABILITY STATUS)」列の状況を確認します。以下のいずれかの状況が表示されます。
    -   OK。この状況は、セキュリティー問題が検出されなかったことを示します。
    -   脆弱 (Vulnerable)。この状況は、潜在的なセキュリティー問題または脆弱性が検出されたことを示します。
    -   不明 (Unknown)。この状況は、イメージのスキャン中、最終的な脆弱性の状況を判断できるまで表示されます。
    -   サポートされない OS (Unsupported OS)。この状況は、イメージが脆弱性アドバイザーによるスキャンの対象としてサポートされていない場合に表示されます。
4.  状況の詳細を検索するには、脆弱性アドバイザーのレポートを確認します。

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    CLI 出力に、脆弱なパッケージのリスト、検出された脆弱性の説明、および問題の修正方法に関する指示へのリンクが示されます。


## イメージ内の問題の解決 
{: #va_report}

脆弱性アドバイザーによって報告される一般的な問題の修正例を示します。
{:shortdesc}

脆弱性アドバイザーは、報告されたセキュリティー問題や構成問題とともに、修正処置を示します。報告された問題の中には、Dockerfile を更新することで修正できるものがあります。一般的な問題を迅速に解決するために、以下のコード例を使用して、Dockerfile にソリューションを実装してください。

### パスワードの最大使用日数、パスワードの最小使用日数、およびパスワードの最小長
{: #va_password}

問題: 以下のエラーのいずれか、またはすべてを受け取る。

```
パスワードの最大使用日数は 90 日に設定する必要があります。
```
{: screen}

```
パスワードの最小長は 8 でなければなりません。
```
{: screen}

```
ユーザーが開始したパスワード変更の相互間に経過する必要がある最小日数は 1  でなければなりません。
```
{: screen}

修正: 以下のコードを Dockerfile に追加して、パスワード・コンプライアンスを設定します。

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### SSH 脆弱性 
{: #ssh}

問題: 以下のエラーが返される。

```
SSH サーバーがインストールされていてはなりません。
```
{: screen}

修正: SSH を使用する代わりに、`docker attach` または `docker exec` を使用して、コンテナーにアクセスします。Dockerfile には SSH サーバーをインストールするためのステップが含まれていないことを確認してください。

