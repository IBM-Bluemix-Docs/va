---

copyright:
  years: 2015, 2017

lastupdated: "2017-03-07"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# IBM MQ Bluemix Container イメージ
{: #ibm_mq}

ibm-mq イメージは {{site.data.keyword.containershort}} 用に提供されています。これには、{{site.data.keyword.IBM}} MQ Advanced for Developers が含まれています (保証なし)。 

{:shortdesc}


## 機能
{: #ibm_mq_how}

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers には、ユーザー独自のメッセージング・ソリューションおよび {{site.data.keyword.IBM_notm}} MQ アプリケーションの開発を開始するために必要なものすべてが含まれています。 

{{site.data.keyword.Bluemix_notm}} でメッセージング・ソリューションおよび {{site.data.keyword.IBM_notm}} MQ アプリケーションを開発するために、{{site.data.keyword.containershort}} に ibm-mq Docker イメージをデプロイできます。次に、キュー・マネージャーを作成して実行し、それらのキュー・マネージャーを、メッセージング・ソリューションまたはアプリケーションの要件を満たすように Web UI または端末を使用して構成します。

**注:** ibm-mq イメージは開発および単体テストのみに使用できます。製品の探索、チュートリアルでの学習、そして {{site.data.keyword.IBM_notm}} MQ を使った場合の組織にとってのメリットの評価にも、このイメージを使用できます。

## 含まれている内容
{: #ibm_mq_what}

この ibm-mq イメージには {{site.data.keyword.IBM_notm}} MQ Advanced for Developers バージョン 9.0.x のソフトウェア・パッケージが含まれています。これは、製品の全機能バージョンであり、開発および単体テストに使用できます。このバージョンは無料でダウンロードでき、ライセンス条件の範囲内で自由に使用できます。

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers は、Windows 64 ビット版オペレーティング・システムと Linux (x86-64) オペレーティング・システムで利用できます。この製品の前提条件のすべてがダウンロード・パッケージに含まれています。詳しくは、[IBM MQ Downloads ![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www-03.ibm.com/software/products/lv/ibm-mq-advanced-for-developers){: new_window} を参照してください。

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers に含まれるフィーチャーについて詳しくは、[IBM MQ ![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window} を参照してください。



## 使用上の制約事項
{: #ibm_mq_usage}

次の表は、{{site.data.keyword.Bluemix_notm}} での ibm-mq イメージの無料使用に適用される制約事項を示しています。

| 環境| 無料使用上の制約事項|
|-------------|-------------------------|
| 開発	| ibm-mq イメージを無制限に無料使用して、開発用に単一コンテナーを作成できます。|
| テスト | ibm-mq イメージを無制限に無料使用して、単体テスト用に単一コンテナーを作成できます。許可されるテストについて詳しくは、[ライセンス情報](getstarted.html#ibm_mq_license )を参照してください。|
| 実動 | {{site.data.keyword.IBM_notm}} MQ Advanced for Developer ライセンスは実動での使用を許可していません。|

**注:** ibm-mq イメージの価格設定は、{{site.data.keyword.Bluemix_notm}} で使用するコンテナーの価格設定とは無関係です。


## ライセンス情報
{: #ibm_mq_license}

ライセンスに関する情報:

*	Dockerfile および関連するスクリプトは、[Apache License 2.0 ![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.apache.org/licenses/LICENSE-2.0.html){: new_window} で使用許諾されます。
* [{{site.data.keyword.IBM_notm}} MQ Advanced for Developers のライセンス情報 ![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/973FA648A5DE42948525806E004CC757?OpenDocument){: new_window}。

**注:** 他者へのライセンスの配布は許可されていません。イメージ内の {{site.data.keyword.IBM_notm}} MQ のご利用条件では、開発と単体テストに使用が制限されています。


## 概説
{: #ibm_mq_get_started}

{{site.data.keyword.Bluemix_notm}} で実行しているコンテナー内で {{site.data.keyword.IBM_notm}} MQ を実行および管理するには、以下の手順を実行します。

1. [ibm-mq イメージに基づくコンテナーをプロビジョンします](ibm_mq.html#ibm_mq_provision)。

    **注:** コンテナーは、その内部にあるキュー・マネージャーの実行状態と連携しています。コンソールまたはコマンド・ラインを使用してキュー・マネージャーを停止すると、コンテナーは少し後に停止します。 

2. コンテナー内で実行中の {{site.data.keyword.IBM_notm}} MQ リソースを管理します。IBM MQ Web コンソールからグラフィカルに行うか、または、端末からコマンドを使用してプログラマチックに行うことができます。 

    1. 以下のいずれかのオプションを選択して、コンテナー内の {{site.data.keyword.IBM_notm}} MQ リソースを管理します。
    
        *	{{site.data.keyword.IBM_notm}} MQ Web コンソールを起動し、{{site.data.keyword.IBM_notm}} MQ をグラフィカルに管理します。詳しくは、[IBM MQ Web コンソールの起動](ibm_mq.html#ibm_mq_webui)を参照してください。
        *	端末から Docker CLI を使用します。詳しくは、[Docker CLI を使用するための端末のセットアップ](ibm_mq.html#ibm_mq_docker_commands)を参照してください。
        * 端末から IBM Bluemix Container Service CLI を使用します。 詳しくは、[IBM Bluemix Container Service CLI を使用するための端末のセットアップ](ibm_mq.html#ibm_mq_containers_cli)を参照してください。
        
    2. コンテナー内で実行される {{site.data.keyword.IBM_notm}} MQ キュー・マネージャーを構成します。 

        * キュー・マネージャーをコマンド・ラインを介して管理するには、*runmqsc* プログラムを使用します。コマンド `runmqsc QM_Name` を実行します (*QM_Name* は、キュー・マネージャーの名前です)。詳しくは、[コマンド・ラインを使用したコンテナーの管理](ibm_mq.html#ibm_mq_access)を参照してください。
    
        * キュー・マネージャーをグラフィカルに管理するには、Web UI を使用します。
        
    3. [IBM MQ Knowledge Center ![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window} で説明されている手順に従って、キュー・マネージャーにメッセージ・キューを作成し、それらのメッセージ・キューにメッセージを出し入れします。
    
        例えば、端末から `runmqsc` で `DEFINE QLOCAL('MY.Q')` コマンドを実行して新規ローカル・キューを作成します (*MY.Q* は、ローカル・キューの名前です)。Web コンソールでキューを表示するか、runmqsc で `DISPLAY QLOCAL('MY.Q')` コマンドを実行してキューおよびそのプロパティーを表示することができます。
    
3. [オプション] コンテナー内の {{site.data.keyword.IBM_notm}} MQ ログをモニターします。詳しくは、[コマンド・ラインを使用したコンテナーの管理](ibm_mq.html#ibm_mq_access)を参照してください。

IBM MQ の概要説明について詳しくは、[IBM MQ バージョン 9.0.x ウェルカム・ページ ![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window} を参照してください。

イメージ、および独自のイメージを Docker でローカルに作成する方法について詳しくは、[MQ Docker GitHub ページ ![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/mq-docker){: new_window} を参照してください。
    
IBM MQ に関するブログおよび最新情報については、[MQ Developer 作業ブログ ![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/developerworks/community/blogs/messaging){: new_window} を参照してください。


### ibm-mq イメージに基づくコンテナーのプロビジョン
{: #ibm_mq_provision}

{{site.data.keyword.IBM_notm}} から提供されている ibm-mq イメージに基づく Docker コンテナーを {{site.data.keyword.Bluemix_notm}} でプロビジョンするには、以下の手順を実行します。

1.	{{site.data.keyword.Bluemix_notm}} にログインします。{{site.data.keyword.Bluemix_notm}} ID を使用してログインしてください。
2.	カタログから**「コンテナー」**を選択し、*ibm-mq* イメージを選択します。
3.	**「単一」**を選択して単一インスタンス・コンテナーを作成します。このコンテナーは開発およびテストの目的に使用できます。
4.	コンテナーの名前 (例: mq) を入力します。
5.	コンテナーのサイズを選択します。
6.	「パブリック IP アドレス」フィールドで、**「要求およびバインド・パブリック IP」**を選択します。
7.	「パブリック・ポート」フィールドで、**1414/tcp, 9443/tcp** を指定します。
8.	詳細オプションを展開し、**「新規環境変数の追加」**をクリックし、環境変数 LICENSE および MQ_QMGR_NAME を入力します。

    * IBM MQ Advanced for Developers ライセンス条項に同意するため、**LICENSE** の値を **accept** に設定します。 
    
        ライセンスに同意する前にライセンスを表示したい場合は、LICENSE を「view」に設定できます。そうすると、ライセンスがコンテナー・ログに出力されます。 
    
        第 1 言語が英語でない場合、**LANG** という別の環境変数を、*zh_TW* (中国語 (繁体字))、*zh* (中国語)、*cs* (チェコ語)、*en* (英語)、*fr* (フランス語)、*de* (ドイツ語)、*el* (ギリシャ語)、*id* (インドネシア語)、*it* (イタリア語)、*ja* (日本語)、*ko* (韓国語)、*lt* (リトアニア語)、*pl* (ポーランド語)、*pt* (ポルトガル語)、*ru* (ロシア語)、*sl* (スロベニア語)、*es* (スペイン語)、*tr* (トルコ語) のいずれかに設定することで、異なる言語のライセンス・ファイルを表示できます。

    * **MQ_QMGR_NAME** を値 *MYMQ* を使用して設定します。ここで、*MYMQ* は選択したキュー・マネージャー名です。
 
        **注:** 環境変数 LICENSE および MQ_QMGR_NAME に値が指定されていない場合、コンテナーは作成されますが、開始しません。 
    
    * [オプション] デフォルトでは、{{site.data.keyword.IBM_notm}} MQ Web コンソールがコンテナー内で開始されます。このデフォルト動作を変更するには、**MQ_DISABLE_WEB_CONSOLE** 環境変数の値を *true* に設定します。
    
        **注:** Web コンソールを使用するには、*MQ_DISABLE_WEB_CONSOLE* 環境変数を設定しないでください。

    * [オプション] {{site.data.keyword.Bluemix_notm}} UI を使用して MQ ログを確認するには、環境変数 **LOG_LOCATIONS** を値 `/var/mqm/qmgr/QM_Name/errors/AMQERR01.LOG` に設定します。ここで、*QM_Name* は、環境変数 *MQ_QMGR_NAME* に設定した値です。
    
    *	[オプション] デフォルトでは、{{site.data.keyword.IBM_notm}} MQ Developer デフォルト・オブジェクトが作成されます。このデフォルト動作を変更するには、**MQ_DEV** 環境変数を *false* に設定し、IBM MQ のデフォルトのインストール値をカスタム値に変更します。詳しくは、[デフォルトのインストール値の変更](ibm_mq.html#ibm_mq_dev_default)を参照してください。
 
9. [オプション] 「詳細オプション」セクションで、ボリュームを割り当てます。コンテナーのマイグレーションを経ても {{site.data.keyword.IBM_notm}} MQ データおよび構成が永続的に保管されるようにするには、{{site.data.keyword.Bluemix_notm}} ボリュームを使用します。ボリュームに保管されていない {{site.data.keyword.IBM_notm}} MQ データは、コンテナーが終了すると失われます。

    ibm-mq イメージは {{site.data.keyword.Bluemix_notm}} ボリュームと共に使用できますが、ボリュームは {{site.data.keyword.IBM_notm}} MQ で検出および利用できるように **/mnt/mqm** にマウントされる必要があります。

    デフォルトでは、ボリュームなしで作成されたコンテナーでは、コンテナーが停止したときに IBM MQ データはすべて失われます。これを防止するには、永続メッセージングおよび {{site.data.keyword.Bluemix_notm}} ボリュームを使用する必要があります。

    1. **「ボリュームの作成」**をクリックし、ボリューム名を入力し、ファイル共有を選択します。
    2. **「既存ボリュームの割り当て」**をクリックし、作成したボリュームを選択します。パスを入力するボックスに **/mnt/mqm** と入力します。**「読み取り専用」**ボックスにチェック・マークが付いていないことを確認します。

    **注:** IBM MQ データ・ディレクトリーとして使用するためにボリュームをマウントするときには、ロケーション */mnt/mqm* にマウントする必要があります。*/mnt/mqm* にマウントすることによって、ibm-mq スクリプトは、そのボリュームを検出し、それを使用するように IBM MQ ファイル・システムを構成します。
     
10.	**「作成」**をクリックして、コンテナーが実行を開始するまで待ちます。

    コンテナーが作成された後、コンテナー・ダッシュボードが開きます。コンテナーの状況が**実行中**に設定されていることを確認します。 

    コンテナーのログを確認するには、**「モニタリングおよびログ」**をクリックし、**「ロギング」**タブを選択します。ログが表示されます。ログの拡張分析を行うには、**「詳細ビュー」**をクリックしてログを Kibana で表示します。

キュー・マネージャーが作成され、開始されました。これで、以下の接続詳細を使用して MQ アプリケーションを接続できるようになりました。

| 接続プロパティー | 値 |
|-------|-------|
| IP アドレス| コンテナーのパブリック IP アドレス|
| ポート | 1414 |
| チャネル| DEV.APP.SVRCONN |


### コマンド・ラインを使用したコンテナーの管理
{: #ibm_mq_access}

{{site.data.keyword.IBM_notm}} MQ を実行するコンテナーが {{site.data.keyword.Bluemix_notm}} にデプロイされた後、コンテナーにアクセスし、コンテナーの状況を確認し、{{site.data.keyword.IBM_notm}} MQ インストールを検証することができます。

コンテナー内の {{site.data.keyword.IBM_notm}} MQ のセットアップと構成を検証するには、以下の手順を実行します。

1.	IBM Containers CLI をセットアップします。 詳しくは、[Bluemix CLI ページ](https://plugins.ng.bluemix.net/ui/home.html)を参照し、指示に従って最新の Cloud Foundry Container サービス・プラグインをインストールします。

2.	端末から、{{site.data.keyword.Bluemix_notm}} ID を使用して、コンテナーが実行されている {{site.data.keyword.Bluemix_notm}} 組織およびスペースにログインします。以下のコマンドを実行します。

    1. `bx login`
    2. `bx ic init`
    
3.	コンテナーの状況を確認します。コマンド `bx ic ps` を実行します。

4.	Bash セッションをコンテナーにアタッチします。

    `bx ic exec -it container_name /bin/bash`
    
    ここで、container_name はコンテナーの名前です。
    
5.	コマンドを実行して MQ セットアップを管理します。以下に例を示します。

| コマンド | アクション |
|---------|---------|
| dspmqver | IBM MQ バージョンおよびビルド情報を確認します。|
| dspmq | コンテナー内で実行中のキュー・マネージャーの状況を確認します。| 
| runmqsc | MQ リソースを管理します。|

MQ ログをモニターするには、以下のいずれかのオプションを選択します。

* {{site.data.keyword.containershort}} CLI コマンドを実行するように端末が構成されている場合、コマンド `bx ic exec container_id tail -f /var/mqm/qmgr/QM_ Name/errors/AMQERR01.LOG` を実行します。ここで、*container_id* はコンテナーの名前、*QM_ Name* はキュー・マネージャーの名前です。
    
* Docker CLI コマンドを実行するように端末が構成されている場合、コマンド `docker exec container_id tail -f /var/mqm/qmgr/QM_Name/errors/AMQERR01.LOG` を実行します。ここで、*container_id* はコンテナーの名前、*QM_Name* はキュー・マネージャーの名前です。
    
    

### IBM MQ Web コンソールの起動
{: #ibm_mq_webui}

{{site.data.keyword.IBM_notm}} MQ Web コンソールを起動するには、以下の手順を実行します。

1.	{{site.data.keyword.Bluemix_notm}} ダッシュボードで、コンテナーを選択し、コンテナーの状況が*実行中*に設定されていることを確認します。
2.	コンテナーにパブリック・ポート 9443 が構成されていることを検証します。
3.	コンテナー詳細を確認してパブリック IP を見つけるか、または、まだ構成されていない場合はパブリック IP を要求してバインドします。
4.	Web ブラウザーから URL `https://DockerContainerPublicIP:9443/ibmmq/console/` を起動します。ここで、DockerContainerPublicIP は、コンテナーのパブリック IP です。

    MQ コンソールをホスティングするときに Web サーバーが使用する TLS 証明書が生成され、自己署名されているため、セキュリティー警告が開きます。**「例外の追加」**をクリックし、確認します。

    **注:** 実稼働環境では、ご使用のブラウザーで認識および信頼される独自の証明書を使用してください。この証明書は、認証局によって発行されたものである必要があります。 
    
    ブラウザーで Web ユーザー・インターフェースが開きます。
    
5. 次の資格情報を使用して {{site.data.keyword.IBM_notm}} MQ コンソールにログインします。

    1.	ユーザー: admin
    2.	パスワード: passw0rd
    
これで IBM MQ を管理する準備ができました。

### IBM Bluemix Container Service CLI を使用するための端末のセットアップ
{: #ibm_mq_containers_cli}

{{site.data.keyword.containershort}} CLI を使用して、コンテナー内で直接 {{site.data.keyword.IBM_notm}} MQ 管理コマンドを実行します。

bx ic コマンドを実行して IBM MQ を管理するように端末をセットアップするには、以下の手順を実行します。

1.	{{site.data.keyword.containershort}} CLI をセットアップします。詳しくは、[Bluemix CLI ページ](https://plugins.ng.bluemix.net/ui/home.html)を参照し、指示に従って最新の Cloud Foundry Container サービス・プラグインをインストールします。

2.	端末から、{{site.data.keyword.Bluemix_notm}} にログインします。以下のコマンドを実行します。

    1. `bx login`
    2. `bx ic init`
    
3.	Bash セッションをコンテナーにアタッチします。

    `bx ic exec -it container_name /bin/bash`
    
    ここで、container_name はコンテナーの名前です。
    


### Docker CLI を使用するための端末のセットアップ
{: #ibm_mq_docker_commands}

Docker CLI を使用して、コンテナー内で直接 {{site.data.keyword.IBM_notm}} MQ 管理コマンドを実行します。

Docker コマンドを実行して IBM MQ を管理するように端末をセットアップするには、以下の手順を実行します。

1.	{{site.data.keyword.containershort}} CLI をセットアップします。詳しくは、[Bluemix CLI ページ](https://plugins.ng.bluemix.net/ui/home.html)を参照し、指示に従って最新の Cloud Foundry Container サービス・プラグインをインストールします。

2.	端末から、{{site.data.keyword.Bluemix_notm}} にログインします。以下のコマンドを実行します。

    1. `bx login`
    2. `bx ic init`
    3. 環境変数 DOCKER_HOST、DOCKER_CERT_PATH、および DOCKER_TLS_VERIFY に提供される値をコピーします。
    4. {{site.data.keyword.containershort}} に接続するため、以下の変数を設定してローカル Docker 環境を指定変更します。
        
        `export DOCKER_HOST=<your_host_value>`
        
        `export DOCKER_CERT_PATH=<your_cert_path_value>`
        
        `export DOCKER_TLS_VERIFY=1`

3.	Bash セッションをコンテナーにアタッチします。

    `bx ic exec -it container_name /bin/bash`
    
    ここで、container_name はコンテナーの名前です。
    
**注:** このオプションでは一部の Docker コマンドのみがサポートされます。




### デフォルトのインストール値の変更
{: #ibm_mq_dev_default}

キュー・マネージャーが作成される際、{{site.data.keyword.IBM_notm}} MQ Developer のデフォルトを使用して作成されます。これらのデフォルト・オブジェクトは、アプリケーションの開発をすぐに始められるように、あるいは、{{site.data.keyword.IBM_notm}} MQ を試してみることができるように設計されています。 

キュー・マネージャー作成時に使用されるデフォルト・オブジェクトは次のとおりです。

* *passw0rd* というパスワードを持つ MQ 管理者ユーザー **admin**。
*	パスワードを持たない MQ アプリケーション・ユーザー **app**。
*	ポート 1414/TCP にバインドするように構成されたリスナー **DEV.LISTENER.TCP**。
*	MQ クライアント・アプリケーション接続を処理するように構成されたチャネル **DEV.APP.SVRCONN**。
*	MQ 管理接続を処理するように構成されたチャネル **DEV.ADMIN.SVRCONN**。
*	メッセージを入れることのできる 3 つのキュー **DEV.QUEUE.1**、**DEV.QUEUE.2**、**DEV.QUEUE.3**。
*	配信不能メッセージが入れられる送達不能キュー**DEV.DEAD.LETTER.QUEUE**。
*	トピック・ストリング */dev* を持つ基本トピック **DEV.BASE.TOPIC**。
*	チャネル **DEV.APP.SVRCONN** または **DEV.ADMIN.SVRCONN** を経由して接続する場合を除いてすべてのクライアント接続をブロックするチャネル認証レコード。
*	チャネル **DEV.ADMIN.SVRCONN** を経由する場合を除いてすべての管理ユーザー接続をブロックするチャネル認証レコード。
*	ユーザー admin がチャネル **DEV.ADMIN.SVRCONN** に接続することのみを許可するチャネル認証レコード。
*	管理アプリケーションが接続時に有効な資格情報を渡すこと、およびそのユーザー ID を権限検査に採用することが必要であるように設定された接続認証。

これらのデフォルト・オブジェクトを使用して、キューにメッセージを出し入れしたり、トピックにサブスクライブまたはパブリッシュを行ったりできるクライアント・アプリケーションを接続できます。

作成された Developer デフォルトをカスタマイズしたい場合は、ibm-mq イメージからコンテナーを作成するときに以下の環境変数を追加できます。

| 環境変数 | 目的| デフォルト|
|----------------------|---------|---------|
| MQ_ADMIN_PASSWORD | 作成された MQ 管理者のパスワードのデフォルトを変更して、自身で選択したものにするには、この変数を設定します。パスワードの長さは、少なくとも 8 文字でなければなりません。管理ユーザーのユーザー名は admin に固定されています。| passw0rd |
| MQ_APP_PASSWORD | アプリケーション・ユーザーのパスワードを設定するには、この変数を設定します。アプリケーション・ユーザーのユーザー名は app に固定されています。この環境変数が設定されていると、MQ クライアントを接続するときにユーザー ID とパスワードの指定が必要になります。<br> パスワードの長さは、少なくとも 8 文字でなければなりません。|  |
| MQ_TLS_KEYSTORE | この環境変数を、MQ Web コンソールおよび IBM MQ キュー・マネージャーが TLS 操作に使用するようにしたい証明書を含んでいる PKCS#12 鍵ストアのロケーションに設定します。<br> この環境変数が設定されていると、作成されるデベロッパー・チャネルでは、CipherSpec‘TLS_RSA_WITH_AES_256_GCM_SHA384’を使用して TLS が有効になります。<br> **注:** 鍵ストア・ファイルを {{site.data.keyword.Bluemix_notm}} コンテナー内で使用可能にする必要があります。これを行うには、コンテナーを作成するときに、鍵ストアを含んでいるボリュームをマウントします。|  |
| MQ_TLS_PASSPHRASE | 環境変数 MQ_TLS_KEYSTORE に指定した鍵ストアのパスワードを設定するには、この環境変数を設定します。|  |
| MQ_DEV | {{site.data.keyword.IBM_notm}} MQ Developer デフォルト・オブジェクトが作成されるかどうかを制御するには、この環境変数を設定します。| true |
| MQ_DISABLE_WEB_CONSOLE | コンテナーの CPU/RAM 使用量を節減したい場合は、この環境変数を設定して Web コンソールの構成と開始を無効にします。|  |



