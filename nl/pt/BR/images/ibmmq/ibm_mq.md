---

copyright:
  years: 2015, 2017

lastupdated: "2017-03-07"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Imagem de contêiner do IBM MQ Bluemix
{: #ibm_mq}

A imagem ibm-mq é fornecida para {{site.data.keyword.containershort}}. Ela inclui o {{site.data.keyword.IBM}} MQ Advanced for Developers (sem garantia). 

{:shortdesc}


## Como ele Funciona
{: #ibm_mq_how}

O {{site.data.keyword.IBM_notm}} MQ Advanced for Developers contém tudo o que você precisa para começar a desenvolver suas próprias soluções do sistema de mensagens e aplicativos {{site.data.keyword.IBM_notm}} MQ. 

Para desenvolver soluções de sistema mensagens e aplicativos {{site.data.keyword.IBM_notm}} MQ no {{site.data.keyword.Bluemix_notm}}, é possível implementar a imagem ibm-mq Docker no {{site.data.keyword.containershort}}. Em seguida, é possível criar e executar gerenciadores de filas e usar a UI da web ou um terminal para configurá-los para atender aos seus requisitos de solução do sistema de mensagens ou de aplicativo.

**Nota:** é possível usar a imagem ibm-mq para desenvolvimento e teste de unidade apenas. Também é possível usar a imagem para explorar o produto, aprender com tutoriais e avaliar a contribuição que o {{site.data.keyword.IBM_notm}} MQ pode fazer para sua organização.

## O que está incluído
{: #ibm_mq_what}

Esta imagem ibm-mq contém o pacote de software para {{site.data.keyword.IBM_notm}} MQ Advanced for Developers Versão 9.0.x, que é uma versão de função integral do produto que é possível usar para desenvolvimento e teste de unidade. É possível fazer download dessa versão gratuitamente e você terá a liberdade de utilizá-la dentro dos
termos da licença.

O {{site.data.keyword.IBM_notm}} MQ Advanced for Developers está disponível em sistemas operacionais Windows 64 bits e em sistemas operacionais Linux em x86-64. Todos os pré-requisitos do produto estão incluídos no pacote de download. Para obter mais informações, consulte [Downloads do IBM MQ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](http://www-03.ibm.com/software/products/lv/ibm-mq-advanced-for-developers){: new_window}

Para obter informações sobre os recursos que estão incluídos no {{site.data.keyword.IBM_notm}} MQ Advanced for Developers, consulte [IBM MQ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}



## Restrições de uso
{: #ibm_mq_usage}

A tabela a seguir mostra as restrições que se aplicam ao uso grátis da imagem ibm-mq em {{site.data.keyword.Bluemix_notm}}:

| Ambiente	| Restrições de uso grátis |
|-------------|-------------------------|
| Desenvolvimento	| Uso grátis ilimitado da imagem ibm-mq para criar contêineres únicos para desenvolvimento. |
| Teste | Uso grátis ilimitado da imagem ibm-mq para criar contêineres únicos para teste de unidade. Verifique as [Informações de licença](getstarted.html#ibm_mq_license ) para obter informações adicionais sobre qual teste é permitido. |
| Produção | A licença do {{site.data.keyword.IBM_notm}} MQ Advanced for Developer não permite o uso para produção. |

**Nota:** a precificação para a imagem ibm-mq independe da precificação para os contêineres que você usar em {{site.data.keyword.Bluemix_notm}}.


## Informações sobre licença
{: #ibm_mq_license}

Informações sobre licenças:

*	Os arquivos do Docker e os scripts associados estão licenciados no [Apache License 2.0 ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](http://www.apache.org/licenses/LICENSE-2.0.html){: new_window}.
* [Informações sobre licença para {{site.data.keyword.IBM_notm}} MQ Advanced for Developers ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/973FA648A5DE42948525806E004CC757?OpenDocument){: new_window}.

**Nota:** a licença não permite distribuição adicional. Os termos para {{site.data.keyword.IBM_notm}} MQ na imagem restringem o uso ao desenvolvimento e teste de unidade.


## Introdução
{: #ibm_mq_get_started}

Conclua as etapas a seguir para executar e gerenciar o {{site.data.keyword.IBM_notm}} MQ em um contêiner que é executado no {{site.data.keyword.Bluemix_notm}}:

1. [Provisione um contêiner que se baseia na imagem ibm-mq](ibm_mq.html#ibm_mq_provision)

    **Nota:** o contêiner está ligado ao estado de execução do Gerenciador de filas dentro dele. Se você parar o Gerenciador de Filas usando o console ou a linha de comandos, o contêiner será interrompido logo depois. 

2. Gerenciar recursos do {{site.data.keyword.IBM_notm}} MQ que estão em execução em um contêiner. É possível fazer isso graficamente por meio do console da web do IBM MQ ou programaticamente usando comandos por meio de um terminal. 

    1. Escolha qualquer uma das opções a seguir para gerenciar recursos do {{site.data.keyword.IBM_notm}} MQ no contêiner:
    
        *	Ative o console da web do {{site.data.keyword.IBM_notm}} MQ e gerencie o {{site.data.keyword.IBM_notm}} MQ graficamente. Para obter mais informações, consulte [Ativar o console da web do IBM MQ](ibm_mq.html#ibm_mq_webui).
        *	Em um terminal, use a CLI do Docker. Para obter mais informações, consulte [Configurando um terminal para usar a CLI do Docker](ibm_mq.html#ibm_mq_docker_commands).
        * Em um terminal, use a CLI do serviço de contêiner do IBM Bluemix. Para obter mais informações, consulte
[Configurando um terminal para usar a CLI do serviço de contêiner do IBM Bluemix](ibm_mq.html#ibm_mq_containers_cli)
        
    2. Configure o Gerenciador de Filas do {{site.data.keyword.IBM_notm}} MQ que é executado no contêiner. 

        * Para gerenciar o Gerenciador de Filas por meio da linha de comandos, use o programa *runmqsc*. Execute o comando a seguir: `runmqsc QM_Name` em que *QM_Name* é o nome de seu Gerenciador de Filas. Para obter mais informações, consulte [Gerenciando seu contêiner por meio da linha de comandos](ibm_mq.html#ibm_mq_access).
    
        * Para gerenciar o Gerenciador de Filas graficamente, use a UI da Web.
        
    3. Siga as instruções no [IBM MQ Knowledge Center ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window} para criar Filas de mensagens no seu Gerenciador de Filas e colocar ou obter mensagens para elas.
    
        Por exemplo, execute em `runmqsc` o comando a seguir para criar uma nova Fila local: `DEFINE QLOCAL('MY.Q')`, em que *MY.Q* é o nome da Fila local do terminal. É possível visualizar a fila no console da web ou é possível exibir a fila e suas propriedades executando em 'runmqsc` o comando a seguir: `DISPLAY QLOCAL('MY.Q')`.
    
3. [Opcional] Monitore os logs de MQ do {{site.data.keyword.IBM_notm}} dentro do contêiner. Para obter mais informações, consulte [Gerenciando seu contêiner por meio da linha de comandos](ibm_mq.html#ibm_mq_access).

Para obter mais informações sobre como começar na Introdução do IBM MQ, consulte [Página de boas-vindas do IBM MQ Versão 9.0.x ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}

Para obter mais informações sobre a imagem e como construir sua própria imagem localmente no Docker, consulte [a página MQ Docker GitHub ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/mq-docker){: new_window}
    
Para obter os blogs e informações mais recentes sobre o IBM MQ, consulte [O blog do MQ Developerworks ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/developerworks/community/blogs/messaging){: new_window}


### Fornecendo um contêiner que se baseia na imagem ibm-mq
{: #ibm_mq_provision}

Conclua as etapas a seguir para provisionar um contêiner do Docker no {{site.data.keyword.Bluemix_notm}} que é baseado na imagem ibm-mq que é fornecida pelo {{site.data.keyword.IBM_notm}}:

1.	Efetue login no {{site.data.keyword.Bluemix_notm}}. Efetue login com seu ID do {{site.data.keyword.Bluemix_notm}}.
2.	No catálogo, selecione **Contêineres** e escolha a imagem *ibm-mq*.
3.	Selecione **Único** para criar um contêiner de instância única, que pode ser usado
para propósitos de desenvolvimento e teste.
4.	Insira o nome do contêiner; por exemplo, mq.
5.	Selecione o tamanho do contêiner.
6.	No campo Endereço IP público, selecione **Solicitar e ligar IP público**.
7.	No campo Portas públicas, especifique **1414/tcp, 9443/tcp**.
8.	Expanda as Opções avançadas, clique em **Incluir uma nova variável de ambiente** e insira as variáveis de ambiente a seguir: LICENSE e MQ_QMGR_NAME.

    * Configure o valor de **LICENSE** como **accept** para concordar com os termos de licença do IBM MQ Advanced for Developers. 
    
        Se você desejar visualizar a licença antes de aceitá-la, poderá configurar LICENSE como "visualizar" e a licença será impressa nos logs do contêiner. 
    
        Se inglês não for seu primeiro idioma, será possível visualizar o arquivo de licença em um idioma diferente, configurando outra variável de ambiente chamada **LANG** como um dos valores a seguir: *zh_TW* para Chinese_TW, *zh* para chinês, *cs* para tcheco, *en* para inglês, *fr* para francês, *de* para alemão, *el* para grego, *id* para indonésio, *it* para italiano, *ja* para japonês, *ko* para coreano, *lt* para lituano, *pl* para polonês, *pt* para português, *ru* para russo, *sl* para esloveno, *es* para espanhol ou *tr* para turco.

    * Configure o valor de **MQ_QMGR_NAME** com o valor *MYMQ* em que *MYMQ* é o seu nome do Gerenciador de Filas escolhido.
 
        **Nota:** se você não especificar valores para as variáveis de ambiente LICENSE e MQ_QMGR_NAME, o contêiner será criado, mas não iniciado. 
    
    * [Opcional] Por padrão, o {{site.data.keyword.IBM_notm}} MQ Web Console é iniciado no contêiner. Para mudar o comportamento padrão, configure o valor da variável de ambiente **MQ_DISABLE_WEB_CONSOLE** como *true*.
    
        **Nota:** para usar o console da web, não configure a variável de ambiente *MQ_DISABLE_WEB_CONSOLE*.

    * [Opcional] Para ver os logs MQ por meio da UI do {{site.data.keyword.Bluemix_notm}}, configure a variável de
ambiente **LOG_LOCATIONS** para o valor `/var/mqm/qmgr/QM_Name/errors/AMQERR01.LOG` em que
*QM_Name* é o valor que você configura para a variável de ambiente *MQ_QMGR_NAME*.
    
    *	[Opcional] Por padrão, os objetos padrão do {{site.data.keyword.IBM_notm}} MQ Developer são criados. Para mudar o comportamento padrão, configure a variável de ambiente **MQ_DEV** como *false* e modifique os valores de instalação padrão para o IBM MQ com seus valores customizados. Para obter mais informações, consulte [Modificar os valores de instalação padrão](ibm_mq.html#ibm_mq_dev_default)
 
9. [Opcional] Na seção Opções avançadas, designe um volume. Use volumes do {{site.data.keyword.Bluemix_notm}} para armazenar dados do {{site.data.keyword.IBM_notm}} MQ e configuração persistente sobre migrações de contêiner. Os dados do {{site.data.keyword.IBM_notm}} MQ que não estão armazenados em um volume serão perdidos se um contêiner for finalizado.

    A imagem ibm-mq pode ser usada com volumes {{site.data.keyword.Bluemix_notm}}, no entanto, os volumes devem ser montados como **/mnt/mqm** para serem detectados e usados pelo {{site.data.keyword.IBM_notm}} MQ.

    Por padrão, quaisquer contêineres criados sem um volume perderão quaisquer dados do IBM MQ quando o contêiner for interrompido. Se você quiser evitar isso, deverá usar mensagens persistentes e Volumes do {{site.data.keyword.Bluemix_notm}}.

    1. Clique em **Criar um volume**, insira um nome de volume e selecione um compartilhamento de arquivo.
    2. Clique em **Designar um volume existente** e selecione o volume que você criou. Insira **/mnt/mqm** na caixa de caminho. Assegure-se de que a caixa **Somente leitura** não esteja marcada.

    **Nota:** ao montar um volume para uso como o diretório de dados do IBM MQ, deve-se montá-lo no local */mnt/mqm*. Ao montá-lo em */mnt/mqm*, o script ibm-mq detectará o volume e configurará o sistema de arquivos IBM MQ para usá-lo.
     
10.	Clique em **Criar** e, em seguida, espere o contêiner iniciar a execução.

    Após o contêiner ser criado, o painel do contêiner será aberto. Verifique se o status do contêiner está configurado como **Em execução**. 

    Para marcar os logs de seu contêiner, clique em **Monitoramento e logs** e selecione a guia **Criação de log**. Os logs são exibidos. Para uma análise avançada de logs, clique em **Visualização avançada** para exibir seus logs no Kibana.

Você criou e iniciou um gerenciador de filas. Agora é possível conectar um aplicativo MQ com os detalhes da conexão a seguir:

| Propriedade da conexão | Valor |
|-------|-------|
| Endereço IP  | Endereço IP público do contêiner |
| Porta | 1414 |
| Canal | DEV.APP.SVRCONN |


### Gerenciando seu contêiner por meio da linha de comandos
{: #ibm_mq_access}

Depois que o contêiner que executa o {{site.data.keyword.IBM_notm}} MQ é implementado no
{{site.data.keyword.Bluemix_notm}}, é possível acessar seu contêiner e o status do contêiner e validar a instalação
do {{site.data.keyword.IBM_notm}} MQ.

Conclua as etapas a seguir para verificar a instalação e a configuração do {{site.data.keyword.IBM_notm}} MQ no contêiner:

1.	Configure a CLI do IBM Containers. Para obter mais informações, consulte
[a página da CLI do Bluemix](https://plugins.ng.bluemix.net/ui/home.html) e siga as instruções para instalar o
plug-in de serviço mais recente do Cloud Foundry Contêiner.

2.	Em um terminal, efetue login na organização e no espaço do {{site.data.keyword.Bluemix_notm}} em que o contêiner está em execução com seu ID do {{site.data.keyword.Bluemix_notm}}. Execute os seguintes comandos:

    1. `bx login`
    2. `bx ic init`
    
3.	Verifique o status do contêiner. Execute o comando a seguir: `bx ic ps`

4.	Anexe uma sessão bash ao contêiner:

    `bx ic exec -it container_name /bin/bash`
    
    em que container_name é o nome do contêiner
    
5.	Execute comandos para gerenciar sua configuração do MQ, por exemplo:

| Comando: | Ação |
|---------|---------|
| dspmqver | Verifique a versão do IBM MQ e as informações de construção. |
| dspmq | Verifique o status do Gerenciador de Filas que está em execução no contêiner. | 
| runmqsc | Gerencie recursos do MQ. |

Para monitorar logs do MQ, escolha uma das opções a seguir:

* Se seu terminal estiver configurado para executar comandos da CLI do {{site.data.keyword.containershort}}, execute o comando a seguir: `bx ic exec container_id tail -f /var/mqm/qmgr/QM_ Name/errors/AMQERR01.LOG` em que *container_id* é o nome do seu contêiner e *QM_ Name* é o nome do Gerenciador de Filas.
    
* Se seu terminal estiver configurado para executar comandos da CLI do Docker, execute o comando a seguir: `docker exec container_id tail -f /var/mqm/qmgr/QM_Name/errors/AMQERR01.LOG` em que *container_id* é o nome do seu contêiner e *QM_Name* é o nome do seu Gerenciador de Filas.
    
    

### Ativando o console da web do IBM MQ
{: #ibm_mq_webui}

Conclua as etapas a seguir para ativar o console da web do {{site.data.keyword.IBM_notm}} MQ:

1.	No painel do {{site.data.keyword.Bluemix_notm}}, selecione seu contêiner e verifique se o status do contêiner está configurado como *Em execução*.
2.	Verifique se o seu contêiner possui a porta pública 9443 configurada.
3.	Verifique os detalhes do contêiner para localizar o IP público ou solicite e ligue um se ainda não tiver feito.
4.	Em um navegador da web, ative a URL a seguir: `https://DockerContainerPublicIP:9443/ibmmq/console/` em que DockerContainerPublicIP é o IP público do seu contêiner.

    Um aviso de segurança é aberto porque o certificado do TLS que o servidor da web usa ao hospedar o console do MQ é gerado e autoassinado. Clique em **Incluir exceção** e confirme.

    **Nota:** em um ambiente de produção, use o seu próprio certificado que é reconhecido e confiável por seu navegador. Esse certificado deve ser emitido por uma autoridade de certificação. 
    
    O navegador abre a interface com o usuário da web.
    
5. Efetue login no console do {{site.data.keyword.IBM_notm}} MQ com as credenciais:

    1.	Usuário: admin
    2.	Senha: passw0rd
    
Agora você está pronto para administrar o IBM MQ.

### Configurando um terminal para usar a CLI de serviço de contêiner do IBM Bluemix
{: #ibm_mq_containers_cli}

Use a CLI do {{site.data.keyword.containershort}} para executar comandos de administração do {{site.data.keyword.IBM_notm}} MQ diretamente em um contêiner.

Conclua as etapas a seguir para configurar um terminal para executar comandos bx ic para gerenciar o IBM MQ:

1.	Configure a CLI do {{site.data.keyword.containershort}}. Para obter mais informações, consulte
[a página da CLI do Bluemix](https://plugins.ng.bluemix.net/ui/home.html) e siga as instruções para instalar o
plug-in de serviço mais recente do Cloud Foundry Contêiner.

2.	Em um terminal, efetue login no {{site.data.keyword.Bluemix_notm}}. Execute os seguintes comandos:

    1. `bx login`
    2. `bx ic init`
    
3.	Anexe uma sessão bash ao contêiner:

    `bx ic exec -it container_name /bin/bash`
    
    em que container_name é o nome do contêiner
    


### Configurando um terminal para usar a CLI do Docker
{: #ibm_mq_docker_commands}

Use a CLI do Docker para executar comandos de administração do {{site.data.keyword.IBM_notm}} MQ diretamente em um contêiner.

Conclua as etapas a seguir para configurar um terminal para executar comandos do Docker para gerenciar o IBM MQ:

1.	Configure a CLI do {{site.data.keyword.containershort}}. Para obter mais informações, consulte
[a página da CLI do Bluemix](https://plugins.ng.bluemix.net/ui/home.html) e siga as instruções para instalar o
plug-in de serviço mais recente do Cloud Foundry Contêiner.

2.	Em um terminal, efetue login no {{site.data.keyword.Bluemix_notm}}. Execute os seguintes comandos:

    1. `bx login`
    2. `bx ic init`
    3. Copie os valores que são fornecidos para as variáveis de ambiente a seguir: DOCKER_HOST, DOCKER_CERT_PATH e DOCKER_TLS_VERIFY.
    4. Substitua o ambiente local do Docker, configurando as variáveis a seguir para se conectar ao {{site.data.keyword.containershort}}:
        
        `export DOCKER_HOST=<your_host_value>`
        
        `export DOCKER_CERT_PATH=<your_cert_path_value>`
        
        `export DOCKER_TLS_VERIFY=1`

3.	Anexe uma sessão bash ao contêiner:

    `bx ic exec -it container_name /bin/bash`
    
    em que container_name é o nome do contêiner
    
**Nota:** somente alguns comandos do Docker são suportados por essa opção.




### Modifique os valores de instalação padrão
{: #ibm_mq_dev_default}

Quando o gerenciador de filas for criado, ele será criado com os padrões do {{site.data.keyword.IBM_notm}} MQ Developer. Esses objetos padrão são projetados para ajudá-lo a iniciar rapidamente o desenvolvimento de aplicativos ou até mesmo testar o {{site.data.keyword.IBM_notm}} MQ. 

O Gerenciador de Filas será criado com os objetos padrão a seguir:

* Um Usuário administrador do MQ **admin** com uma senha de *passw0rd*.
*	Um Usuário do aplicativo do MQ **app** sem senha.
*	Um listener **DEV.LISTENER.TCP** configurado para ligar à porta 1414/TCP.
*	Um canal **DEV.APP.SVRCONN** configurado para manipular conexões de aplicativos cliente do MQ.
*	Um canal **DEV.ADMIN.SVRCONN** configurado para manipular conexões do administrador do MQ.
*	3 Filas **DEV.QUEUE.1**, **DEV.QUEUE.2**, **DEV.QUEUE.3** onde é possível colocar mensagens.
*	Uma Fila de mensagens não entregues **DEV.DEAD.LETTER.QUEUE** na qual mensagens que não puderem ser entregues serão colocadas.
*	Um tópico base **DEV.BASE.TOPIC** com a sequência de tópicos de */dev*.
*	Um registro de Autenticação de canal para bloquear todas as conexões do cliente, a menos que eles estejam se conectando por meio do canal **DEV.APP.SVRCONN** ou **DEV.ADMIN.SVRCONN**.
*	Um registro de Autenticação de canal para bloquear todos os usuários administrativos que estejam se conectando, exceto por meio do canal **DEV.ADMIN.SVRCONN**.
*	Um registro de Autenticação de canal para permitir apenas que o usuário administrador se conecte ao canal **DEV.ADMIN.SVRCONN**.
*	Autenticação de conexão configurada para exigir que aplicativos do administrador passem credenciais válidas ao se conectar e que adotem esse ID do usuário para verificações de autoridade.

Com esses objetos padrão, seria possível conectar um aplicativo cliente que pode colocar e obter mensagens em uma fila ou assinar e publicar no tópico.

Se você desejar customizar os padrões do desenvolvedor que são criados, será possível incluir as variáveis de ambiente a seguir quando criar seu contêiner por meio da imagem ibm-mq.

| Variável do ambiente | Purpose | Padrão |
|----------------------|---------|---------|
| MQ_ADMIN_PASSWORD | Configure essa variável para mudar a senha do administrador do MQ criada por meio do padrão para sua própria escolha. A senha deve ter pelo menos 8 caracteres de comprimento. O nome do usuário para o usuário administrativo é fixado como admin. | passw0rd |
| MQ_APP_PASSWORD | Configure essa variável para configurar a senha para o usuário do aplicativo. O nome do usuário para o usuário do aplicativo é fixado como app. Se você configurar essa variável de ambiente, então deverá fornecer o ID do usuário e a senha ao conectar clientes do MQ. <br> A senha deve ter pelo menos 8 caracteres de comprimento. |  |
| MQ_TLS_KEYSTORE | Configure essa variável de ambiente como o local de um keystore PKCS#12 que contém o certificado que você deseja que o Console da web do MQ e o IBM MQ Queue Manager usem para operações TLS. <br> Quando essa variável de ambiente estiver configurada, os canais do desenvolvedor criados serão ativados para TLS usando a especificação de código 'TLS_RSA_WITH_AES_256_GCM_SHA384'. <br> **Nota:** é necessário tornar o arquivo keystore disponível dentro do seu contêiner do {{site.data.keyword.Bluemix_notm}}. Para fazer isso, é possível montar um volume que contenha seu keystore quando você criar o contêiner. |  |
| MQ_TLS_PASSPHRASE | Configure essa variável de ambiente para configurar a senha para o keystore especificado na variável de ambiente MQ_TLS_KEYSTORE. |  |
| MQ_DEV | Configure essa variável de ambiente para controlar se os objetos padrão do {{site.data.keyword.IBM_notm}} MQ Developer serão criados. | verdadeiro |
| MQ_DISABLE_WEB_CONSOLE | Configure essa variável de ambiente para desativar a configuração e o início do console da web se você desejar conservar o uso de CPU/RAM de seu contêiner. |  |



