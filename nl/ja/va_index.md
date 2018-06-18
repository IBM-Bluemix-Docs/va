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


# 脆弱性アドバイザーを使用したイメージ・セキュリティーの管理
{: #va_index}

脆弱性アドバイザーは、IBM やサード・パーティーによって提供されるか、組織のレジストリー名前空間に追加された、コンテナー・イメージのセキュリティー状況を検査します。
{:shortdesc}


イメージを名前空間に追加すると、脆弱性アドバイザーによってイメージが自動的にスキャンされ、セキュリティー問題や潜在的な脆弱性が検出されます。セキュリティー問題が検出された場合、報告された脆弱性を修正するための指示が出されます。引き続き脆弱なイメージからコンテナーをデプロイできますが、これらのコンテナーが攻撃または侵害される可能性があることに注意してください。


## 脆弱性アドバイザーの概要
{: #about}

脆弱性アドバイザーは、{{site.data.keyword.containerlong}} のセキュリティー管理機能を提供します。 脆弱性アドバイザーは、セキュリティー状況レポートを生成し、修正やベスト・プラクティスを提案し、非セキュアなイメージが実行されるのを制限するための管理機能を提供します。 脆弱性アドバイザーによって報告されたセキュリティー問題や構成問題を修正することで、{{site.data.keyword.cloud_notm}} インフラストラクチャーをセキュアに保つことができます。
{:shortdesc}

脆弱性アドバイザーには、以下の機能があります。

-   イメージをスキャンして脆弱性がないか調べる
-   {{site.data.keyword.containerlong_notm}} に固有のセキュリティー・プラクティスに基づく評価レポートを提供する
-   アプリケーション・タイプのサブセット用の構成ファイルをセキュアにするために推奨情報を提供する
-   レポートで報告された[脆弱なパッケージ](#packages)または[構成の問題](#app_configurations)を修正する方法について説明する

レジストリー・ダッシュボードの **SECURITY REPORT** 列にリポジトリーの状況が表示されます。レポートでは、イメージ用の有効なクラウド・セキュリティー・プラクティスが示されます。 

脆弱性アドバイザーのダッシュボードには、イメージのセキュリティーの概要と評価が示されます。脆弱性アドバイザーのダッシュボードについて詳しくは、[脆弱性レポートの検討](#va_reviewing)を参照してください。
	
	
**データ保護**

アカウント内のイメージとコンテナーをスキャンしてセキュリティー問題を探すために、脆弱性アドバイザーは以下の情報を収集、保管、および処理します。
- ID、説明、およびイメージ名 (レジストリー、名前空間、リポジトリー名、およびイメージ・タグ) が含まれたフリー・フォームのテキスト・フィールド
- Kubernetes メタデータ。Kubernetes リソースの名前 (ポッド名、レプリカセット名、デプロイメント名) など
- 構成ファイルのファイル・モードと作成タイム・スタンプに関するメタデータ
- イメージおよびコンテナー内のシステムとアプリケーションの構成ファイルの内容
- インストールされているパッケージとライブラリー (バージョンを含む)

上記のリストに示された、脆弱性アドバイザーが処理するフィールドや場所に個人情報を入れないでください。

データ・センター・レベルで集約されたスキャン結果は処理され、サービスを運用および改善するための匿名メトリックが生成されます。

スキャン結果は、生成されてから 30 日後に削除されます。



## 脆弱性のタイプ
{: #types}

### 脆弱なパッケージ
{: #packages}

脆弱性アドバイザーは、サポートするオペレーティング・システムに基づいたイメージ内の脆弱なパッケージを検査し、その脆弱性に関連したセキュリティー上の注意事項へのリンクを提供します。
{:shortdesc}

スキャン結果に、既知の脆弱性問題があるパッケージが表示されます。以下の表にリストされた Docker イメージ・タイプに対して公開されるセキュリティー上の注意事項を元に、潜在的な脆弱性は毎日更新されます。 脆弱パッケージがスキャンに合格するようにするには通常、脆弱性の修正が反映された新しいバージョンのパッケージが必要です。 同じパッケージに複数の脆弱性がリストされることがあり、その場合は、1 回のパッケージのアップグレードで複数の脆弱性が修正されることがあります。


  |Docker 基本イメージ|セキュリティー上の注意事項のソース|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://git.alpinelinux.org/) および [CIRCL CVE Search ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.circl.lu/)|
  |CentOS|[CentOS announce archives ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://lists.centos.org/pipermail/centos-announce/) および [CentOS CR announce archives ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://usn.ubuntu.com/)|
  {: caption="表 1. 脆弱性アドバイザーが脆弱なパッケージについて検査する、サポートされている Docker 基本イメージ" caption-side="top"}




### 構成の問題
{: #app_configurations}

構成の問題は、アプリのセットアップ方法に関連する潜在的なセキュリティー問題です。報告された問題の多くは、Dockerfile を更新することによって修正できます。
{:shortdesc}

イメージがスキャンされるのは、そのイメージが脆弱性アドバイザーによってサポートされるオペレーティング・システムに基づいている場合のみです。脆弱性アドバイザーは、以下のタイプのアプリについて構成設定を検査します。
-   MySQL
-   NGINX
-   Apache



## コンテナー・スキャナーのインストール
{: #va_install_livescan}

開始する前に以下の手順を実行します。

1.  {{site.data.keyword.Bluemix_notm}} CLI クライアントにログインします。統合されたアカウントがある場合は、`--sso` を使用します。
2.  [`kubectl` CLI](../../containers/cs_cli_install.html#cs_cli_configure) のターゲットを、Helm チャートを使用するクラスターにします。
3.  コンテナー・スキャナーのサービス ID と API キーを作成し、名前を付けます。
    1.  `<scanner_serviceID>` を好みのサービス ID の名前に置き換えて、次のコマンドを実行してサービス ID を作成します。その **CRN** を書き留めます。
    
        ```
    	bx iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  サービス API キーを作成します。ここで、`<scanner_serviceID>` は前の手順で作成したサービス ID で、`<scanner_APIkey_name>` は好みのスキャナー API キーの名前に置き換えます。 
    
        ```
    	bx iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	    スキャナー API キーが返されます。
	
	    後で取得できないため、スキャナー API キーは安全に保管してください。
	    {: tip}
	
    3.  `Writer` 役割を付与するサービス・ポリシーを作成します。
    		
        ```
    	bx iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

Helm チャートを構成するには、以下のようにします。

1.  [クラスター内に Helm をセットアップします](../../containers/cs_integrations.html#helm)。RBAC ポリシーを使用して Helm tiller アクセス権限を付与する場合は、すべての名前空間のコンテナーをスキャナーが監視できるように、tiller 役割がすべての名前空間にアクセス可能であるようにしてください。

2.  IBM チャート・リポジトリー (`ibm-incubator` など) を Helm に追加します。

    ```
    helm repo add ibm-incubator https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

3.  コンテナー・スキャナー Helm チャートのデフォルトの構成設定を、ローカルの YAML ファイルに保存します。チャート・リポジトリー (`ibm-incubator` など) を Helm チャート・パスに含めます。

    ```
    helm inspect values ibm-incubator/ibmcloud-container-scanner > config.yaml
    ```
    {: pre}

4.  `config.yaml` ファイルを編集します。

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
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th>フィールド</th>
    <th>値</th>
    </thead>
    <tbody>
    <tr>
    <td><code>EmitURL</code></td>
    <td>脆弱性アドバイザーの地域エンドポイント URL を入力します。URL を取得するには、<code>bx cr info</code> を実行し、<strong>コンテナー・レジストリー</strong>のアドレスを取得します。<code>registry</code> を <code>va</code> に置き換えます。例: <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>クラスターが入っている {{site.data.keyword.Bluemix_notm}} アカウント ID で置き換えます。アカウント ID を取得するには、<code>bx account list</code> を実行します。</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>コンテナー・スキャナーをインストールする Kubernetes クラスターで置き換えます。クラスター ID をリストするには、<code>bx cs clusters</code> を実行します。</td>
    </tr>
    <tr>
    <td><code>APIKey</code></td>
    <td>先ほど作成したスキャナー API キーで置き換えます。</td>
    </tr>
    </tbody></table>

5.  更新された `config.yaml` ファイルを使用して、クラスターに Helm チャートをインストールします。更新されたプロパティーが、チャートの構成マップに保管されます。`<myscanner>` を、Helm チャートの好みの名前と置き換えます。チャート・リポジトリー (`ibm-incubator` など) を Helm チャート・パスに含めます。

    ```
    helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    **注**: コンテナー・スキャナーは `kube-system` 名前空間にインストールされますが、すべての名前空間からのコンテナーをスキャンします。

6.  チャートのデプロイメント状況を確認します。チャートの準備ができている場合は、出力の先頭付近の **STATUS** フィールドに `DEPLOYED` の値があります。

    ```
    helm status <myscanner>
    ```
    {: pre}

7.  チャートがデプロイされた後、`config.yaml` ファイル内の更新済みの設定が使用されたことを確認します。

    ```
    helm get values <myscanner>
    ```
    {: pre}


IBM Container Scanner がインストールされ、livescan エージェントがクラスター内の [DaemonSet ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) としてデプロイされました。スキャナーは `kube-system` 名前空間にデプロイされますが、`default` など、すべての Kubernetes 名前空間でポッドに割り当てられているすべてのコンテナーをスキャンします。 



## 脆弱性レポートの検討
{: #va_reviewing}

イメージをデプロイする前に、脆弱なパッケージおよび非セキュアなアプリケーション設定の詳細について、イメージの脆弱性アドバイザー・レポートを検討できます。
{:shortdesc}



1.  {{site.data.keyword.Bluemix_notm}} にログインします。
2.  **「カタログ」**をクリックします。
3.  **「インフラストラクチャー」**の下で**「コンテナー」**をクリックします。
4.  **「コンテナー・レジストリー」**タイルをクリックします。
5.  **「脆弱性アドバイザー」**を展開し、**「スキャンされたリポジトリー (Scanned Repositories)」**をクリックします。
6.  `latest` タグが付いたイメージのレポートを表示するには、そのリポジトリーの行をクリックします。 レポートには、問題の総数と、問題が脆弱パッケージと構成問題のどちらなのかが示されます。 リポジトリー内に `latest` タグが存在しない場合、最新のイメージが使用されます。
7.  選択したイメージの各脆弱パッケージについての情報を表示するには、**「検出された脆弱パッケージ (Vulnerable Packages Found)」**表で **VULNERABILITIES** 列のリンクをクリックしてレポートを開きます。
    1.  詳細情報を表示するには、要約を展開します。
    2.  オペレーティング・システム・ディストリビューターの通知がある場合は、**OFFICIAL NOTICE** 列のリンクをクリックします。
8.  各構成問題についての情報を表示するには、**「検出された構成問題 (Configuration Issues Found)」**表で、問題の行をクリックします。
9.  レポートに示された各問題の修正処置を実行し、イメージを再ビルドします。 Dockerfile の一部の問題は、[イメージの問題の解決](#va_report)に記載されているコードを使用して解決することができます。

脆弱性が存在するのに修正しない場合、それらの問題はそのイメージを使用してビルドされたコンテナーのセキュリティーに影響を与える可能性があります。ただし、セキュリティーおよび構成に問題のあるイメージをコンテナー内で使用し続けることができます。

 



## CLI の使用による、脆弱性レポートの検討
{: #va_registry_cli}

CLI を使用して、{{site.data.keyword.registrylong_notm}} で名前空間に保管された Docker イメージのセキュリティーを検討できます。
{:shortdesc}

1.  {{site.data.keyword.Bluemix_notm}} アカウント内のイメージをリストします。保管されている名前空間に関係なくすべてのイメージのリストが返されます。

    ```
    bx cr image-list
    ```
    {: pre}

2.  **SECURITY STATUS** 列で状況を確認します。
    -   `No Issues`: セキュリティー問題は見つかりませんでした。
    -   `X Issues`: 潜在的なセキュリティー問題や脆弱性が見つかりました。
    -   `Scanning`: イメージはスキャン中であり、最終的な脆弱性の状況はまだ決定されていません。
4.  状況の詳細を確認するには、脆弱性アドバイザーのレポートを検討します。

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    CLI 出力には、構成の問題に関する以下の情報が表示されています。
      - セキュリティー・プラクティス: 検出された脆弱性の説明
      - 修正処置: この脆弱性の修正方法の詳細


## イメージにおける一般的な問題の解決
{: #va_report}

脆弱性アドバイザーによって報告される可能性のある一般的な問題の修正例を示します。問題の一部は、Dockerfile を更新することによって修正できます。
{:shortdesc}

### パスワードの最大使用日数、パスワードの最小使用日数、およびパスワードの最小長
{: #va_password}

**問題**: 以下の脆弱性の 1 つ以上を受け取る。

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

**修正**: 次のコードを Dockerfile に追加することによってパスワード・コンプライアンスを設定します。

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```
{: codeblock}

### SSH 脆弱性
{: #ssh}

**問題**: 次の脆弱性が返される。

```
SSH サーバーがインストールされていてはなりません。
```
{: screen}

**修正**: SSH を使用する代わりに、`docker attach` または `docker exec` を使用してコンテナーにアクセスします。 Dockerfile には SSH サーバーをインストールするためのステップが含まれていないことを確認してください。
