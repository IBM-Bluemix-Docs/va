---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

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


# 脆弱性アドバイザーを使用したイメージ・セキュリティーの管理
{: #va_index}

脆弱性アドバイザーは、{{site.data.keyword.IBM}} やサード・パーティーによって提供されるか、組織のレジストリー名前空間に追加された、コンテナー・イメージのセキュリティー状況を検査します。 Container Scanner (非推奨) が各クラスターにインストールされている場合、脆弱性アドバイザーは、実行中のコンテナーの状況も検査します。
{:shortdesc}

イメージを名前空間に追加すると、脆弱性アドバイザーによってイメージが自動的にスキャンされ、セキュリティー問題や潜在的な脆弱性が検出されます。 セキュリティー問題が検出された場合、報告された脆弱性を修正するための指示が出されます。

脆弱性アドバイザーは、推奨される修正とベスト・プラクティスが含まれたセキュリティー状況レポートを生成して、[{{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started) のセキュリティー管理を提供します。

脆弱性アドバイザーによって問題が検出されれば、このイメージのデプロイの非推奨を指示する判断になります。 このイメージのデプロイを選択した場合、このイメージからデプロイされたコンテナーには、コンテナーの攻撃や侵害に使用される可能性のある既知の問題が含まれます。 判断は、指定した適用除外項目に基づいて調整されます。 この判断は、{{site.data.keyword.containerlong_notm}} 内の非セキュアなイメージのデプロイメントを防ぐために、Container Image Security Enforcement が使用できます。

脆弱性アドバイザーによって報告されたセキュリティー問題や構成問題を修正することで、{{site.data.keyword.cloud_notm}} インフラストラクチャーをセキュアにすることができます。

## 脆弱性アドバイザーの概要
{: #about}

脆弱性アドバイザーはイメージをセキュアにするための機能を提供します。
{:shortdesc}

以下の機能を使用できます。

- イメージをスキャンして問題がないか調べる
- [Container Scanner](#va_install_container_scanner) が各クラスターにインストールされている (非推奨) 場合に、実行中のコンテナー内の問題をスキャンする
- {{site.data.keyword.containerlong_notm}} に固有のセキュリティー・プラクティスに基づく評価レポートを提供する
- アプリケーション・タイプのサブセット用の構成ファイルをセキュアにするために推奨情報を提供する
- レポートで報告された[脆弱なパッケージ](#packages)または[構成の問題](#app_configurations)を修正する方法について説明する
- 判断を [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce) に提供する
- アカウント・レベル、名前空間レベル、リポジトリー・レベル、またはタグ・レベルでレポートに適用除外項目を適用し、フラグが付いた問題をユース・ケースに適用しない場合にマークを付ける
- {{site.data.keyword.registrylong_notm}} グラフィカル・ユーザー・インターフェースの**「タグ」**ビューで、関連付けられたコンテナーへのリンクを提供する。 Container Scanner (非推奨) がインストールされているクラスターで、実行中のコンテナーとそのイメージを使用しているコンテナーをリストできます。

レジストリー・ダッシュボードの**「ポリシーの状況」**列にリポジトリーの状況が表示されます。 リンクされたレポートでは、イメージ用の有効なクラウド・セキュリティー・プラクティスが示されます。

脆弱性アドバイザーのダッシュボードには、イメージのセキュリティーの概要と評価が示され、Container Scanner (非推奨) がインストールされている場合は、実行中のコンテナーへのリンクが示されます。脆弱性アドバイザーのダッシュボードについて詳しくは、[脆弱性レポートの検討](#va_reviewing)を参照してください。

**データ保護**

アカウント内のイメージとコンテナーをスキャンしてセキュリティー問題を探すために、脆弱性アドバイザーは以下の情報を収集、保管、および処理します。

- ID、説明、およびイメージ名 (レジストリー、名前空間、リポジトリー名、およびイメージ・タグ) が含まれたフリー・フォームのフィールド
- Kubernetes メタデータ。Kubernetes リソースの名前 (ポッド名、ReplicaSet 名、デプロイメント名など) を含む
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

脆弱性アドバイザーは、サポートするオペレーティング・システムを使用しているイメージ内の脆弱なパッケージを検査し、その脆弱性に関連したセキュリティー上の注意事項へのリンクを提供します。
{:shortdesc}

スキャン結果に、既知の脆弱性問題が含まれているパッケージが表示されます。 次の表にリストされた Docker イメージ・タイプに対して公開されるセキュリティー上の注意事項を使用して、潜在的な脆弱性は毎日更新されます。 脆弱パッケージがスキャンに合格するようにするには通常、脆弱性の修正が反映された新しいバージョンのパッケージが必要です。 同じパッケージに複数の脆弱性がリストされることがあり、その場合は、1 回のパッケージのアップグレードで複数の脆弱性を修正できます。

  |Docker 基本イメージ|セキュリティー上の注意事項のソース|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://git.alpinelinux.org/) および [CIRCL CVE Search ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.circl.lu/)。|
  |CentOS| [CentOS announce archives ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://lists.centos.org/pipermail/centos-announce/) および [CentOS CR announce archives ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://lists.centos.org/pipermail/centos-cr-announce/)。|
  |Debian|[Debian security announcements ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://lists.debian.org/debian-security-announce/)。|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://access.redhat.com/errata/#/)。|
  |Ubuntu|[Ubuntu Security Notices ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://usn.ubuntu.com/)。|
  {: caption="表 1. 脆弱性アドバイザーが脆弱なパッケージについて検査する、サポートされている Docker 基本イメージ" caption-side="top"}

### 構成の問題
{: #app_configurations}

構成の問題は、アプリのセットアップ方法に関連する潜在的なセキュリティー問題です。 報告された問題の多くは、Dockerfile を更新することによって修正できます。
{:shortdesc}

イメージがスキャンされるのは、そのイメージが脆弱性アドバイザーによってサポートされるオペレーティング・システムを使用している場合のみです。 脆弱性アドバイザーは、以下のタイプのアプリについて構成設定を検査します。

- MySQL
- NGINX
- Apache

## 脆弱性レポートの検討
{: #va_reviewing}

イメージをデプロイする前に、そのイメージの脆弱性アドバイザー・レポートを検討して、脆弱性の影響を受けるパッケージ、コンテナーやアプリケーションの非セキュアな設定の詳細を知ることができます。 また、イメージが組織のポリシーに準拠しているかどうかを確認することもできます。
{:shortdesc}

検出された問題に対処しない場合、それらの問題はそのイメージを使用してビルドされたコンテナーのセキュリティーに影響を与える可能性があります。 Container Image Security Enforcement がデプロイされていない場合は、セキュリティーおよび構成に問題のあるイメージをコンテナー内で使用し続けることができます。 Container Image Security Enforcement がデプロイされ、イメージに対してアクティブな場合、検出されたすべての問題は、このイメージからデプロイ可能なコンテナーに対するポリシーによって適用除外される必要があります。

Container Image Security Enforcement で脆弱性アドバイザーの問題の適用範囲を構成するには、[ポリシーのカスタマイズ](/docs/services/Registry?topic=registry-security_enforce#customize_policies)を参照してください。
{:tip}

イメージが組織のポリシーによって設定されている要件を満たしていない場合は、イメージをデプロイする前に、その要件を満たすようにイメージを構成する必要があります。 組織のポリシーを表示および変更する方法について詳しくは、[組織の適用除外項目ポリシーの設定](#va_managing_policy)を参照してください。
{:tip}

Container Scanner (非推奨) がデプロイされている場合、イメージをデプロイした後、脆弱性アドバイザーは引き続き、コンテナー内のセキュリティーおよび構成の問題をスキャンします。見つかった問題は、[コンテナー・レポートの検討](#va_reviewing_container)に記載されている手順に従うことによって解決できます。

### GUI の使用による、脆弱性レポートの検討
{: #va_reviewing_gui}

GUI を使用して、{{site.data.keyword.registrylong_notm}} で名前空間に保管された Docker イメージのセキュリティーを検討できます。
{:shortdesc}

1. {{site.data.keyword.cloud_notm}} にログインします。
2. **ナビゲーション・メニュー**・アイコンをクリックし、次に**「Kubernetes」**をクリックします。
3. **「レジストリー (Registry)」**をクリックし、次に**「イメージ」**タイルをクリックします。 イメージと各イメージのセキュリティー状況のリストが**「イメージ」**表に表示されます。
4. `latest` タグが付いたイメージのレポートを表示するには、そのイメージの行をクリックします。 そのイメージのデータが表示された**「イメージの詳細 (Image Details)」**タブが開きます。 リポジトリー内に `latest` タグが存在しない場合、最新のイメージが使用されます。
5. セキュリティー状況に問題が示される場合に、その問題について確認するには、**「タイプ別の問題 (Issues by Type)」**タブをクリックします。 **「脆弱性 (Vulnerabilities)」**表と**「構成の問題」**表が開きます。

   - **「脆弱性 (Vulnerabilities)」**表。各問題の脆弱性 ID、その問題のポリシーの状況、影響を受けるパッケージ、および問題の解決方法が示されます。その問題の詳細を表示するには、行を展開します。 その問題のベンダーによるセキュリティー上の注意事項へのリンクを含む、その問題の要約が表示されます。 既知の脆弱性問題が含まれているパッケージをリストします。
  
     [脆弱性のタイプ](#types)にリストされた Docker イメージ・タイプに対して公開されるセキュリティー上の注意事項を使用して、リストは毎日更新されます。 脆弱パッケージがスキャンに合格するようにするには通常、脆弱性の修正が反映された新しいバージョンのパッケージが必要です。 同じパッケージに複数の脆弱性がリストされることがあり、その場合は、1 回のパッケージのアップグレードで複数の問題を修正できます。 セキュリティー上の注意事項のコードをクリックして、パッケージに関する詳細情報と、パッケージを更新するための手順を表示してください。

   - **「構成の問題 (Configuration Issues)」**表。各問題の構成の問題 ID、その問題のポリシー状況、およびセキュリティー・プラクティスが示されます。その問題の詳細を表示するには、行を展開します。 その問題のセキュリティー上の注意事項へのリンクを含む、その問題の要約が表示されます。
  
     リストには、コンテナーのセキュリティーを向上させるために行うことができる処置の推奨事項と、非セキュアなコンテナーのアプリケーション設定が含まれています。 行を展開すると、問題の解決方法が表示されます。

6. レポートに示された各問題の修正処置を実行し、イメージを再ビルドします。

### CLI の使用による、脆弱性レポートの検討
{: #va_registry_cli}

CLI を使用して、{{site.data.keyword.registrylong_notm}} で名前空間に保管された Docker イメージのセキュリティーを検討できます。
{:shortdesc}

1. {{site.data.keyword.cloud_notm}} アカウント内のイメージをリストします。 保管されている名前空間に関係なくすべてのイメージのリストが返されます。

   ```
   ibmcloud cr image-list
   ```
   {: pre}

2. **SECURITY STATUS** 列で状況を確認します。
    - `No Issues`: セキュリティー問題は見つかりませんでした。
    - `<X> Issues` 検出された潜在的なセキュリティー問題または脆弱性の数。`<X>` は問題の数です。
    - `Scanning`: イメージはスキャン中で、最終的な脆弱性の状況は決定されていません。

3. 状況の詳細を確認するには、脆弱性アドバイザーのレポートを検討します。

   ```
   ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
   ```
   {: pre}

   CLI 出力には、構成の問題に関する以下の情報が表示されています。
      - `セキュリティー・プラクティス`: 検出された脆弱性の説明
      - `修正処置`: この脆弱性の修正方法の詳細

## 組織の適用除外項目ポリシーの設定
{: #va_managing_policy}

{{site.data.keyword.cloud_notm}} 組織のセキュリティーを管理する場合は、ポリシー設定を使用して、問題を適用除外するかどうかを決定できます。 Container Image Security Enforcement を使用して、ポリシーによって適用除外されたすべての問題を考慮した後、セキュリティー問題が含まれないイメージからのみデプロイメントを許可するようにすることができます。
{:shortdesc}

Container Image Security Enforcement がクラスターにデプロイされていない限り、セキュリティー状況に関係なく、どのイメージからもコンテナーをデプロイできます。 Container Image Security Enforcement のデプロイ方法については、[セキュリティー機能のインストール](/docs/services/Registry?topic=registry-security_enforce#security_enforce)を参照してください。

Container Image Security Enforcement を使用すると、脆弱性アドバイザーによって検出されたセキュリティー問題により、コンテナーがイメージからデプロイされなくなります。 検出された問題が含まれたイメージをデプロイできるようにするには、ポリシーに適用除外項目を追加する必要があります。

### GUI を使用した組織の適用除外項目ポリシーの設定
{: #va_managing_policy_gui}

GUI を使用してポリシーに適用除外項目を設定するには、以下のステップを実行します。

1. {{site.data.keyword.cloud_notm}} にログインします。 GUI に脆弱性アドバイザーを表示するには、ログインした状態である必要があります。
2. **ナビゲーション・メニュー**・アイコンをクリックし、次に**「Kubernetes」**をクリックします。
3. **「脆弱性アドバイザー」**の下で、**「ポリシー設定 (Policy Settings)」**をクリックします。
4. **「適用除外項目の作成 (Create Exemption)」**をクリックします。
5. 発行タイプを選択します。
6. 発行 ID を入力します。

   この情報は、[脆弱性レポート](#va_reviewing)にあります。 **「脆弱性 ID (Vulnerability ID)」**列には、CVE またはセキュリティー上の注意事項の問題に使用する ID が含まれています。**「構成問題 ID (Configuration Issue ID)」**列には、構成の問題に使用する ID が含まれています。
   {: tip}

7. 適用除外項目を適用するレジストリー名前空間、リポジトリー、およびタグを選択します。
8. **「保存」**をクリックします。

また、該当する行の上にカーソルを置いて、**「オプションのリストのオープンおよびクローズ」**アイコンをクリックして、適用除外項目を編集および削除することができます。

### CLI を使用した組織の適用除外項目ポリシーの設定
{: #va_managing_policy_cli}

CLI を使用してポリシーに適用除外項目を設定するには、以下のコマンドを実行します。

- セキュリティー問題の適用除外項目を作成するには、[`ibmcloud cr exemption-add`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_add) コマンドを実行します。
- セキュリティー問題の適用除外項目をリストするには、[`ibmcloud cr exemption-list`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_list) コマンドを実行します。
- 適用除外できるセキュリティー問題のタイプをリストするには、[`ibmcloud cr exemption-types`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_types) コマンドを実行します。
- セキュリティー問題の適用除外項目を削除するには、[`ibmcloud cr exemption-rm`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_rm) コマンドを実行します。

コマンドを実行するときに `--help` フラグを使用すると、コマンドの詳細情報を表示できます。

## Container Scanner のインストール (非推奨)
{: #va_install_container_scanner}

Container Scanner は推奨されません。
{: deprecated}

Container Scanner を使用すると、脆弱性アドバイザーはコンテナーの基本イメージに存在しないコンテナーの実行中に検出された問題を報告できます。コンテナーに対してランタイムの変更を行わない場合、イメージ・レポートに同じ問題が表示されるため、Container Scanner は必要ありません。
{:shortdesc}

クラスターで実行されている稼働中のコンテナーのセキュリティー状況を確認するために、Container Scanner をインストールできます。 アプリを保護するために、Container Scanner は定期的に実行中のコンテナーをスキャンし、新たに検出された脆弱性を検出して修正できるようにします。

すべての Kubernetes 名前空間のポッドに割り当てられているコンテナーの脆弱性をモニターするように Container Scanner を設定できます。 脆弱性が見つかった場合は、イメージに関する問題を修正してから、アプリを再デプロイする必要があります。 Container Scanner は、{{site.data.keyword.registrylong_notm}} に保管されているイメージから作成されたコンテナーのみをサポートします。

Container Scanner を使用するには、許可をセットアップしてから [Helm チャート ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.helm.sh/developing_charts) をセットアップし、それを、使用するクラスターに関連付ける必要があります。

### Container Scanner (非推奨) のサービス許可のセットアップ
{: #va_install_container_scanner_permissions}

Container Scanner は推奨されません。
{: deprecated}

Container Scanner では、サービスが動作できるように許可が設定されている必要があります。
{:shortdesc}

サービスの許可を設定するには、以下の手順を実行します。

1. {{site.data.keyword.cloud_notm}} CLI クライアントにログインします。 統合されたアカウントがある場合は、`--sso` を使用します。
2. [`kubectl` CLI のターゲット](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)を、Helm チャートを使用するクラスターにします。
3. Container Scanner のサービス ID と API キーを作成し、名前を付けます。
    1. サービス ID を作成するには、次のコマンドを実行します。ここで、`<scanner_serviceID>` は、そのサービス ID に付けた名前です。 その **CRN** を書き留めます。

       ```
       ibmcloud iam service-id-create <scanner_serviceID>
       ```
       {: codeblock}

    2. サービス API キーを作成します。ここで、`<scanner_serviceID>` は前の手順で作成したサービス ID で、`<scanner_APIkey_name>` はスキャナー API キーに付けた名前です。

       ```
       ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
       ```
       {: codeblock}
       スキャナー API キーが返されます。

       後で取得することはできないため、スキャナー API キーは安全に保管してください。 また、スキャナーがインストールされているクラスターごとに個別のサービス API キーを用意してください。
       {: important}

    3. `Writer` 役割を付与するサービス・ポリシーを作成します。

       ```
       ibmcloud iam service-policy-create --resource-type scaningress --service-name container-registry --roles Writer <scanner_serviceID>
       ```
       {: codeblock}

### Helm チャートの構成 (非推奨)
{: #va_install_container_scanner_helm}

Container Scanner は推奨されません。
{: deprecated}

Helm チャートを構成し、それを使用するクラスターに、関連付けます。
{:shortdesc}

Helm チャートを構成するには、以下の手順を実行します。

1. [IBM Cloud Kubernetes サービスに Helm をセットアップします](/docs/containers?topic=containers-helm#helm)。 役割ベースのアクセス制御 (RBAC) ポリシーを使用して Tiller へのアクセス権限を付与する場合は、Tiller 役割がすべての名前空間にアクセス可能であるようにしてください。 Tiller 役割にすべての名前空間に対するアクセス権限を付与すると、Container Scanner がすべての名前空間のコンテナーを監視できるようになります。

2. IBM チャート・リポジトリー (`ibm` など) を Helm に追加します。

   ```
   helm repo add ibm https://icr.io/helm/ibm
   ```
   {: pre}

3. Container Scanner Helm チャートのデフォルトの構成設定を、ローカルの YAML ファイルに保存します。 チャート・リポジトリー (`ibm` など) を Helm チャート・パスに含めます。

   ```
   helm inspect values ibm/ibmcloud-container-scanner > config.yaml
   ```
   {: pre}

4. `config.yaml` ファイルを編集します。

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
   <caption>表 2. YAML ファイル・コンポーネントについて</caption>
   <thead>
   <th>フィールド</th>
   <th>値</th>
   </thead>
   <tbody>
   <tr>
   <td><code>EmitURL</code></td>
   <td><code>&lt;regional_emit_URL&gt;</code> を、脆弱性アドバイザーの地域エンドポイント URL に置き換えます。 URL を取得するには、<code>ibmcloud cr info</code> を実行し、<strong>コンテナー・レジストリー</strong>のアドレスを取得します。 例: <code>https<span comment="make the link not a link">://us.</span>icr.io</code>。 <code>/va</code> をこのアドレスの最後に追加します。 例: <code>https<span comment="make the link not a link">://us.</span>icr.io/va</code>。 詳細については、[ローカル・リージョン](/docs/services/Registry?topic=registry-registry_overview#registry_regions_local) を参照してください。</td>
   </tr>
   <tr>
   <td><code>AccountID</code></td>
   <td><code>&lt;IBM_Cloud_account_ID&gt;</code> を、クラスターが入っている {{site.data.keyword.cloud_notm}} アカウント ID に置き換えます。 アカウント ID を取得するには、<code>ibmcloud account list</code> を実行します。</td>
   </tr>
   <tr>
   <td><code>ClusterID</code></td>
   <td><code>&lt;cluster_ID&gt;</code> を、Container Scanner をインストールする Kubernetes クラスターに置き換えます。 クラスター ID をリストするには、<code>ibmcloud ks clusters</code> を実行します。 <br> **ヒント**: 名前ではなく、クラスターの ID を使用します。
   </td>
   </tr>
   <tr>
   <td><code>APIKey</code></td>
   <td><code>&lt;scanner_APIkey&gt;</code> を、先ほど作成したスキャナー API キーに置き換えます。</td>
   </tr>
   </tbody></table>

5. 更新された `config.yaml` ファイルを使用して、クラスターに Helm チャートをインストールします。 更新されたプロパティーが、チャートの ConfigMap に保管されます。 `<myscanner>` を、Helm チャートの好みの名前と置き換えます。 チャート・リポジトリー (`ibm` など) を Helm チャート・パスに含めます。

   ```
   helm install -f config.yaml --name=<myscanner> ibm/ibmcloud-container-scanner
   ```
   {: pre}

   Container Scanner は `kube-system` 名前空間にインストールされますが、すべての名前空間からのコンテナーをスキャンします。
   {:tip}

6. チャートのデプロイメント状況を確認します。 チャートの準備ができている場合は、**STATUS** フィールドに `DEPLOYED` の値があります。

   ```
   helm status <myscanner>
   ```
   {: pre}

7. チャートがデプロイされた後、`config.yaml` ファイル内の更新済みの設定が使用されたことを確認します。

   ```
   helm get values <myscanner>
   ```
   {: pre}

Container Scanner がインストールされ、エージェントがクラスターに [DaemonSet ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) としてデプロイされました。 Container Scanner は `kube-system` 名前空間にデプロイされますが、`default` など、すべての Kubernetes 名前空間でポッドに割り当てられているすべてのコンテナーをスキャンします。

## ファイアウォールの内側からの Container Scanner の実行 (非推奨)
{: #va_firewall}

Container Scanner は推奨されません。
{: deprecated}

ファイアウォールで発信接続がブロックされている場合は、そのファイアウォールを構成する必要があります。
{:shortdesc}

ワーカーノードが IP アドレスの TCP ポート `443` の Container Scanner にアクセスすることを許可するように、ファイアウォールを構成するには、[クラスターがパブリック・ファイアウォールを超えてインフラストラクチャー・リソースや他のサービスへアクセスすることを許可する](/docs/containers?topic=containers-firewall#firewall_outbound)のステップ 3 を参照してください。

## コンテナー・レポートの確認 (非推奨)
{: #va_reviewing_container}

Container Scanner は推奨されません。
{: deprecated}

ダッシュボードでコンテナーの状況を調べて、そのセキュリティーが組織のポリシーに準拠しているかどうかを判別できます。 コンテナーのセキュリティー・レポートを検討することもできます。レポートには、脆弱なパッケージと非セキュアなコンテナーやアプリケーションの設定に関する詳細、およびコンテナーが組織ポリシーに準拠しているかどうかが示されます。
{:shortdesc}

**「ポリシーの状況」**フィールドを調べて、スペース内で実行されているコンテナーが、引き続き組織ポリシーに準拠していることを確認します。 状況は、以下のいずれかの条件で示されます。

- `ポリシーに準拠している (Compliant with Policy)`: セキュリティーの点でも構成の点でも問題は検出されませんでした。
- `ポリシーに準拠していない (Not Compliant with Policy)`: 脆弱性アドバイザーによってセキュリティーまたは構成の潜在的な問題が検出されたため、このコンテナーはポリシーに準拠していません。 脆弱イメージのデプロイメントが組織ポリシーで許可されている場合、イメージは「`デプロイに注意が必要`」状態でデプロイされている可能性があり、それをデプロイしたユーザーに警告が送信されます。
- `評価不完全 (Incomplete Assessment)`: スキャンは完了していません。 スキャンはまだ実行中である可能性があります。または、そのコンテナー・インスタンスのオペレーティング・システムが非互換である可能性があります。

コンテナーが可能な限りセキュアであることをセキュリティー・レポートを表示して確認し、報告されたセキュリティーまたは構成の問題に対処します。それには、次の手順に従います。

1. レポートを表示するコンテナーを選択します。
    1. **ナビゲーション・メニュー**・アイコンをクリックし、次に**「Kubernetes」**をクリックします。
    2. **「レジストリー (Registry)」**をクリックし、**「リポジトリー」**タイルをクリックしたら、目的のリポジトリーの行を展開します。
    3. 目的のイメージの行を選択します。
    4. **「関連付けられたコンテナー (Associated Containers)」**タブを選択し、目的のコンテナーの行を選択します。 セキュリティー・レポートが開きます。
2. 各セクションを確認して、イメージ内の各パッケージにセキュリティーと構成の問題がないかどうかを調べます。

    - **脆弱性**: 既知の脆弱性問題が含まれているパッケージをリストします。 [脆弱性のタイプ](#types)にリストされた Docker イメージ・タイプに対して公開されるセキュリティー上の注意事項を使用して、リストは毎日更新されます。 脆弱パッケージがスキャンに合格するようにするには通常、脆弱性の修正が反映された新しいバージョンのパッケージが必要です。 同じパッケージに複数の脆弱性がリストされることがあり、その場合は、1 回のパッケージのアップグレードで複数の問題を修正できます。 セキュリティー上の注意事項のコードをクリックして、パッケージに関する詳細情報と、パッケージを更新するための手順を表示してください。

    - **構成の問題**: コンテナーのセキュリティーを向上させるための推奨事項と、非セキュアなコンテナーのアプリケーション設定をリストします。 行を展開すると、問題の解決方法が表示されます。

   リストされた項目ごとに、修正アクションまたは提案事項が示されます。

3. 各セキュリティー問題のポリシーの状況を検討します。 ポリシーの状況は、この問題が適用除外されているかどうかを示します。

    - `アクティブ`: 適用除外されていない問題があり、その問題がセキュリティーの状況に影響を及ぼしています。
    - `適用除外`: この問題は、ポリシー設定によって適用除外されています。
    - `部分的に適用除外`: この問題は、複数のセキュリティー上の注意事項に関連付けられています。 セキュリティー上の注意事項がすべて適用除外されているわけではありません。

4. 問題を解決するためのコンテナーの更新方法を決定します。

    コンテナー・イメージの問題を修正するには、古いインスタンスを削除して再デプロイする必要があります。これは、既存のコンテナー内にあるデータはすべて失われることを意味します。コンテナーを再デプロイするための適切な方法を選択するには、コンテナーのアーキテクチャーについて十分に理解しておいてください。
    {: important}

    **例**

    - コンテナーとそれが計算するデータが分離されている場合は、コンテナーを停止して削除し、必要な変更をイメージに加えた後に再デプロイすることで、データが損失されないようにすることができます。
    - [Delivery Pipeline](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-deliverypipeline_about#deliverypipeline_about) などの {{site.data.keyword.cloud_notm}} サービスを使用して、脆弱なコンテナー・インスタンスの更新を支援することができます。
    - マイクロサービス・アーキテクチャーでは、別のコンテナー・インスタンスにトラフィックをルーティングしておいて、その間にセキュリティーまたは構成の問題を修正してから、新しいイメージを red-black デプロイメントにプッシュすることができます。

5. これで問題を解決できない場合は、ポリシー設定で問題を適用除外して、この問題がコンテナーのデプロイメントを妨げないようにします。 問題を適用除外するには、**「オプションのリストのオープンおよびクローズ」**アイコンをクリックし、**「適用除外項目の作成 (Create Exemption)」**をクリックします。[組織の適用除外項目ポリシーの設定](#va_managing_policy)を参照してください。

6. **セキュリティー**・レポートに示された問題を修正し、選択した方法に応じて、イメージを再ビルドするかコンテナーを再デプロイします。
