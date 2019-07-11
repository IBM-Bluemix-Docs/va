---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-03"

keywords: IBM Cloud Kubernetes Service, IBM Cloud Container Registry, security status of container images, image security, Vulnerability Advisor, security, registry, vulnerabilities, containers, security issues, configuration issues,

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

脆弱性アドバイザーは、{{site.data.keyword.IBM}} やサード・パーティーによって提供されるか、組織のレジストリー名前空間に追加された、コンテナー・イメージのセキュリティー状況を検査します。
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
- {{site.data.keyword.containerlong_notm}} に固有のセキュリティー・プラクティスに基づく評価レポートを提供する
- アプリケーション・タイプのサブセット用の構成ファイルをセキュアにするために推奨情報を提供する
- レポートで報告された[脆弱なパッケージ](#packages)または[構成の問題](#app_configurations)を修正する方法について説明する
- 判断を [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce) に提供する
- アカウント・レベル、名前空間レベル、リポジトリー・レベル、またはタグ・レベルでレポートに適用除外項目を適用し、フラグが付いた問題をユース・ケースに適用しない場合にマークを付ける

レジストリー・ダッシュボードの**「ポリシーの状況」**列にリポジトリーの状況が表示されます。 リンクされたレポートでは、イメージ用の有効なクラウド・セキュリティー・プラクティスが示されます。

脆弱性アドバイザーのダッシュボードには、イメージのセキュリティーの概要と評価が示されます。脆弱性アドバイザーのダッシュボードについて詳しくは、[脆弱性レポートの検討](#va_reviewing)を参照してください。

**データ保護**

アカウント内のイメージとコンテナーをスキャンしてセキュリティー問題を探すために、脆弱性アドバイザーは以下の情報を収集、保管、および処理します。

- ID、説明、およびイメージ名 (レジストリー、名前空間、リポジトリー名、およびイメージ・タグ) が含まれたフリー・フォームのフィールド
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

スキャン結果に、既知の脆弱性問題が含まれているパッケージが表示されます。 次の表にリストされた Docker イメージ・タイプに対して公開されるセキュリティー上の注意事項を使用して、潜在的な脆弱性は毎日更新されます。 脆弱パッケージがスキャンに合格するようにするには通常、脆弱性の修正が反映された新しいバージョンのパッケージが必要です。 同じパッケージに複数の脆弱性がリストされることがあり、その場合は、1 回のパッケージの更新で複数の脆弱性を修正できます。

  |Docker 基本イメージ|セキュリティー上の注意事項のソース|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://git.alpinelinux.org/) および [CIRCL CVE Search ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.circl.lu/)。|
  |CentOS| [CentOS announce archives ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://lists.centos.org/pipermail/centos-announce/) および [CentOS CR announce archives ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://lists.centos.org/pipermail/centos-cr-announce/)。 脆弱性について詳しくは、[CentOS でのパッケージの脆弱性](#va_centos)を参照してください。|
  |Debian|[Debian security announcements ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://lists.debian.org/debian-security-announce/)。|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Security Data API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://access.redhat.com/labsinfo/securitydataapi).|
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

### GUI の使用による、脆弱性レポートの検討
{: #va_reviewing_gui}

GUI を使用して、{{site.data.keyword.registrylong_notm}} で名前空間に保管された Docker イメージのセキュリティーを検討できます。
{:shortdesc}

1. {{site.data.keyword.cloud_notm}} にログインします。
2. **ナビゲーション・メニュー**・アイコンをクリックし、次に**「Kubernetes」**をクリックします。
3. **「レジストリー (Registry)」**をクリックし、次に**「イメージ」**タイルをクリックします。 イメージと各イメージのセキュリティー状況のリストが**「イメージ」**表に表示されます。
4. `latest` タグが付いたイメージのレポートを表示するには、そのイメージの行をクリックします。 そのイメージのデータが表示された**「イメージの詳細 (Image Details)」**タブが開きます。 リポジトリー内に `latest` タグが存在しない場合、最新のイメージが使用されます。
5. セキュリティー状況に問題が示される場合に、その問題について確認するには、**「タイプ別の問題 (Issues by Type)」**タブをクリックします。 **「脆弱性 (Vulnerabilities)」**表と**「構成の問題」**表が開きます。

   - **「脆弱性 (Vulnerabilities)」**表。 各問題の脆弱性 ID、その問題のポリシーの状況、影響を受けるパッケージ、および問題の解決方法が示されます。 その問題の詳細を表示するには、行を展開します。 その問題のベンダーによるセキュリティー上の注意事項へのリンクを含む、その問題の要約が表示されます。 既知の脆弱性問題が含まれているパッケージをリストします。
  
     [脆弱性のタイプ](#types)にリストされた Docker イメージ・タイプに対して公開されるセキュリティー上の注意事項を使用して、リストは毎日更新されます。 脆弱パッケージがスキャンに合格するようにするには通常、脆弱性の修正が反映された新しいバージョンのパッケージが必要です。 同じパッケージに複数の脆弱性がリストされることがあり、その場合は、1 回のパッケージの更新で複数の問題を修正できます。 セキュリティー上の注意事項のコードをクリックして、パッケージに関する詳細情報と、パッケージを更新するための手順を表示してください。

   - **「構成の問題 (Configuration Issues)」**表。 各問題の構成の問題 ID、その問題のポリシー状況、およびセキュリティー・プラクティスが示されます。 その問題の詳細を表示するには、行を展開します。 その問題のセキュリティー上の注意事項へのリンクを含む、その問題の要約が表示されます。
  
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

### CentOS でのパッケージの脆弱性
{: #va_centos}

CentOS を使用している場合、レポートに誤検出が報告される、つまり実際には存在しない脆弱性がレポートに報告されることがあります。 この状況は、セキュリティー通知が Red Hat からリリースされているものの、そのセキュリティー通知が CentOS には適用されない場合、またはフィックスがまだ CentOS に移植されていない場合に生じます。
{:shortdesc}

ご使用のパッケージに脆弱性があるというレポートを受け取った場合は、以下の手順を行ってください。

1. セキュリティー通知コードをクリックして、パッケージを更新する手順を表示します。
2. パッケージを更新する手順を行って、パッケージを更新します。
3. パッケージが更新された場合は、報告された結果が誤検出ではなかったことが分かり、必要な処置が完了します。
4. インストールに使用可能な新しいバージョンがないため、パッケージが更新されない場合は、報告された結果が誤検出であったことが分かります。 このセキュリティー通知の適用除外項目ポリシーを追加できます。[組織の適用除外項目ポリシーの設定](#va_managing_policy)を参照してください。

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
