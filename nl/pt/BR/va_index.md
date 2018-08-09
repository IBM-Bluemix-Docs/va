---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Gerenciando a segurança de imagens com o Vulnerability Advisor
{: #va_index}

O Vulnerability Advisor verifica o status de segurança das imagens de contêiner que são fornecidas pelo {{site.data.keyword.IBM}}, por terceiros ou incluídas no namespace de registro de sua organização e, se você tiver instalado o scanner de contêiner em cada cluster, ele também verifica o status dos contêineres em execução.
{:shortdesc}

Quando você inclui uma imagem em um namespace, ela é automaticamente varrida pelo Vulnerability Advisor para detectar problemas de segurança e potenciais vulnerabilidades. Se problemas de segurança forem localizados, instruções serão fornecidas para ajudá-lo a corrigir a vulnerabilidade relatada. 

O Vulnerability Advisor fornece o gerenciamento de segurança para o {{site.data.keyword.registrylong_notm}}, gerando um relatório de status de segurança que inclui correções sugeridas e melhores práticas. 

Todos os problemas localizados resultam em um veredito que indica que não é aconselhável implementar essa imagem. Se você escolher implementar a imagem, qualquer contêiner que seja implementado a partir dela terá problemas conhecidos que poderão ser usados para atacar ou, de outra forma, comprometer o contêiner. O Vulnerability Advisor ajusta o seu veredito com base em quaisquer isenções que você especificou. Esse veredito pode ser usado pelo cumprimento de segurança de imagem para evitar a implementação de imagens não seguras no {{site.data.keyword.containerlong_notm}}. 

A correção dos problemas de segurança e de configuração relatados pelo Vulnerability Advisor pode ajudar a proteger a infraestrutura do seu {{site.data.keyword.cloud_notm}}.


## Sobre o Vulnerability Advisor
{: #about}

O Vulnerability Advisor fornece funções para ajudar você a proteger as suas imagens.

{:shortdesc}

 As funções a seguir estão disponíveis:

-   Varreduras de imagens para problemas
-   Varreduras de problemas nos contêineres em execução se você tiver [instalado o scanner de contêiner](#va_install_container_scanner) em cada cluster
-   Fornece um relatório de avaliação que é baseado em práticas de segurança que são específicas para o {{site.data.keyword.containerlong_notm}}
-   Fornece recomendações para proteger arquivos de configuração para um subconjunto de tipos de aplicativos
-   Fornece instruções sobre como corrigir um [pacote vulnerável](#packages) ou [problema de configuração](#app_configurations) relatado em seus relatórios
-   Fornece veredictos para  [ aplicação de segurança de imagem ](../Registry/registry_security_enforce.html#security_enforce)
-   Aplica isenções para os relatórios em uma conta, um namespace, um repositório ou um nível de tag para marcar quando os problemas sinalizados não se aplicam ao seu caso de uso
-   Fornece links para contêineres associados da visualização da **tag** da interface gráfica com o usuário do {{site.data.keyword.registrylong_notm}}. É possível listar os contêineres que estão em execução e os que estão usando essa imagem em um cluster que possui o scanner de contêiner instalado.


No painel Registro, a coluna **Status da política** exibe o status de seus repositórios. O relatório vinculado identifica as boas práticas de segurança de nuvem para as suas imagens. 

O painel do Vulnerability Advisor fornece uma visão geral e uma avaliação da segurança para uma imagem e, se o scanner de contêiner estiver instalado, se vincula aos contêineres em execução. Se desejar saber mais sobre o painel do Vulnerability Advisor, consulte [Revisando um relatório de vulnerabilidade](#va_reviewing).
	
	
**Proteção de Dados**

Para varrer imagens e contêineres em sua conta para buscar problemas de segurança, o Vulnerability Advisor coleta, armazena e processa as informações a seguir:
- campos de texto de formato livre, incluindo IDs, descrições e nomes de imagem (registro, namespace, nome do repositório, e tag de imagem)
- metadados do Kubernetes, incluindo nomes de recursos do Kubernetes como nomes de pod, de conjunto de réplicas e de implementação
- metadados sobre os modos de arquivo e registros de data e hora de criação dos arquivos de configuração
- o conteúdo de arquivos de configuração do sistema e aplicativo em imagens e contêineres
- pacotes e bibliotecas instalados (incluindo suas versões)

Não coloque informações pessoais em nenhum campo ou local que o Vulnerability Advisor processa, conforme identificado na lista anterior.

Os resultados de varredura, agregados em um nível de data center, são processados para produzir métricas anonimadas para operar e melhorar o serviço.

Os resultados de varredura são excluídos 30 dias após serem gerados.


## Tipos de vulnerabilidades
{: #types}


### Pacotes vulneráveis
{: #packages}

O Vulnerability Advisor verifica pacotes vulneráveis em imagens que são baseadas em sistemas operacionais suportados e fornece um link para quaisquer avisos de segurança relevantes sobre a vulnerabilidade.
{:shortdesc}

Os pacotes com problemas de vulnerabilidade conhecida são exibidos nos resultados de varredura. As vulnerabilidades possíveis são atualizadas diariamente a partir de avisos de segurança publicados para os tipos de imagem do Docker que são listados na tabela a seguir. Geralmente, para que um pacote vulnerável passe pela varredura, é necessário uma versão mais recente do pacote que inclua uma correção para a vulnerabilidade. O mesmo pacote pode listar múltiplas vulnerabilidades e, nesse caso, um upgrade de pacote único pode tratar de múltiplas vulnerabilidades.


  |Imagem base Docker|Origem dos avisos de segurança|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://git.alpinelinux.org/) e [Procura CIRCL CVE ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cve.circl.lu/)|
  |CentOS| [Archives de anúncio CentOS ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://lists.centos.org/pipermail/centos-announce/) e [Archives de anúncio CentOS CR ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Anúncios de segurança Debian ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Errata do produto Red Hat ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Avisos de segurança do Ubuntu ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://usn.ubuntu.com/)|
  {: caption="Tabela 1. Imagens base do Docker suportadas que o Vulnerability Advisor verifica para pacotes vulneráveis" caption-side="top"}


### Problemas de configuração
{: #app_configurations}

Os problemas de configuração são potenciais problemas de segurança que estão relacionados a como um app é configurado. Muitos dos problemas relatados podem ser corrigidos atualizando seu Dockerfile.
{:shortdesc}

As imagens serão varridas somente se forem baseadas em um sistema operacional suportado pelo Vulnerability Advisor. O Vulnerability Advisor verifica as definições de configuração para os tipos de apps a seguir:
-   MySQL
-   NGINX
-   Apache




## Instalando o scanner do contêiner
{: #va_install_container_scanner}

Antes de iniciar:

1.  Efetue login no {{site.data.keyword.Bluemix_notm}} CLI do cliente. Se você tiver uma conta federada, use `--sso`.
2.  [Destine sua CLI do `kubectl`](../../containers/cs_cli_install.html#cs_cli_configure) para o cluster no qual você deseja usar um gráfico Helm.
3.  Crie um ID do serviço e uma chave API para o scanner de contêiner e dê a ele um nome:
    1.  Crie um ID do serviço executando o comando a seguir, substituindo `<scanner_serviceID>` por um nome de sua escolha para o ID do serviço. Observe o **CRN**.
    
        ```
    	ibmcloud iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  Crie uma chave API de serviço, em que `<scanner_serviceID>` é o ID do serviço criado na etapa anterior, e substitua `<scanner_APIkey_name>` por um nome de sua escolha para a chave API do scanner. 
    
        ```
    	ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	    A chave API do scanner é retornado.
	
	    Assegure-se de armazenar sua chave API do scanner com segurança porque ela não pode ser recuperada posteriormente.
	    {: tip}
	
    3.  Crie uma política de serviço que conceda a função `Writer`.
    		
        ```
    	ibmcloud iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

Para configurar o gráfico Helm:

1.  [Configure o Helm em seu cluster](../../containers/cs_integrations.html#helm). Se você usar uma política RBAC para conceder o acesso de tiller do Helm, certifique-se de que a função tiller tenha acesso a todos os namespaces para que o scanner possa observar os contêineres em todos os namespaces.

2.  Inclua o repositório do gráfico IBM em seu Helm, como `ibm-incubator`.

    ```
    Repositório comando add ibm-incubadora https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

3.  Salve as definições de configuração padrão para o gráfico Helm do scanner do contêiner em um arquivo YAML local. Inclua o repositório do gráfico, como `ibm-incubator`, no caminho do gráfico Helm.

    ```
    Comando inspect valores ibm-incubator/ibmcloud-container-scanner > config.yaml
    ```
    {: pre}

4.  Edite o arquivo `config.yaml`.

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
    <caption>Entendendo os componentes de arquivo YAML</caption>
    <thead>
    <th>Campo</th>
    <th>Valor</th>
    </thead>
    <tbody>
    <tr>
    <td><code>EmitURL</code></td>
    <td>Insira a URL de terminal regional do Vulnerability Advisor. Para obter a URL, execute <code>ibmcloud cr info</code> e recupere o endereço do <strong>Registro do contêiner</strong>. Substitua <code>registry</code> por <code>va</code>. Por exemplo: <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>Substitua pelo ID da conta do {{site.data.keyword.Bluemix_notm}} em que seu cluster está. Para obter o ID da conta, execute <code>ibmcloud account list</code>.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>Substitua pelo cluster do Kubernetes em que deseja instalar o scanner do contêiner. Para listar os IDs do cluster, execute <code>ibmcloud ks clusters</code>. <br> **Dica**: use o ID do cluster, não o nome.
    </td>
    </tr>
    <tr>
    <td><code>Chave</code></td>
    <td>Substitua pela chave API do scanner criado anteriormente.</td>
    </tr>
    </tbody></table>

5.  Instale o gráfico Helm em seu cluster com o arquivo `config.yaml` atualizado. As propriedades atualizadas são armazenadas em um configmap para seu gráfico. Substitua `<myscanner>` por um nome de sua escolha para seu gráfico Helm. Inclua o repositório do gráfico, como `ibm-incubator`, no caminho do gráfico Helm.

    ```
    helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    O scanner de contêiner é instalado no namespace `kube-system`, mas varre os contêineres de todos os namespaces.
    {:tip}

6.  Verifique o status de implementação do gráfico. Quando o gráfico está pronto, o campo **STATUS** próximo à parte superior da saída tem um valor de `DEPLOYED`.

    ```
    helm status <myscanner>
    ```
    {: pre}

7.  Depois que o gráfico for implementado, verifique se as configurações atualizadas no arquivo `config.yaml` foram usadas.

    ```
    helm get values <myscanner>
    ```
    {: pre}


Agora, o IBM Container Scanner está instalado e o agente está implementado como um
[DaemonSet
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) em seu cluster. Embora o scanner esteja implementado no namespace kube-system, ele varre todos os contêineres que estão designados a pods em todos os seus namespaces do Kubernetes, como default. 


## Executando o scanner de contêiner por trás de um firewall
{: #va_firewall}

Se o seu firewall bloquear as conexões de saída, ele deverá ser configurado para permitir que os nós do trabalhador acessem o scanner de contêiner na porta TCP <code>443</code> nos endereços IP na tabela a seguir.
{:shortdesc}

 

<p>
  <table summary="As linhas devem ser lidas da esquerda para a direita, com a zona do servidor na coluna 1 e os endereços IP para correspondência na coluna 2.">
  <caption>Endereços IP a serem abertos para o tráfego de saída</caption>
      <thead>
      <th>Região</th>
      <th>Endereço IP</th>
      </thead>
    <tbody>
      <tr>
         <td>AP Sul</td>
         <td><code> 168.1.40.158 </code><br><code> 130.198.65.182 </code></td>
      </tr>
      <tr>
         <td>União Europeia Central</td>
         <td><code> 159.8.220.182 </code><br><code> 158.177.74.102 </code></td>
        </tr>
      <tr>
        <td>Sul do Reino Unido</td>
        <td><code> 158.175.71.134 </code><br><code> 5.10.111.190 </code></td>
      </tr>
      <tr>
        <td>Leste dos EUA</td>
         <td><code> 169.60.73.158 </code><br><code> 169.61.84.102 </code></td>
      </tr>
      <tr>
        <td>Sul dos EUA</td>
        <td><code> 169.47.103.118 </code><br><code> 169.48.165.6 </code></td>
      </tr>
      </tbody>
    </table>
</p>


## Revisando um relatório de vulnerabilidade usando a GUI
{: #va_reviewing}

Antes de implementar uma imagem, é possível revisar seu relatório do Vulnerability Advisor para obter detalhes sobre quaisquer pacotes vulneráveis e configurações de app não seguro.
{:shortdesc}



1.  Efetue login no {{site.data.keyword.Bluemix_notm}}.
2.  Clique em **Catálogo**.
3.  Em **Infraestrutura**, clique em **Contêineres**.
4.  Clique no azulejo **Registro do contêiner**.
5.  Expanda **Vulnerability Advisor** e clique em **Repositórios varridos**.
6.  Para ver o relatório para a imagem que é marcada como `mais recente`, clique na linha para esse repositório. O relatório mostra o número total de problemas e se eles são pacotes vulneráveis ou problemas de configuração. Se nenhuma marcação `mais recente` existir no repositório, a imagem mais recente será usada.
7.  Para visualizar informações sobre cada pacote vulnerável para a imagem selecionada, na tabela **Pacotes vulneráveis localizados**, clique no link na coluna **VULNERABILIDADES** para abrir o relatório.
    1.  Para ver mais informações, expanda o resumo.
    2.  Se o aviso de um distribuidor de sistema operacional for fornecido, clique no link na coluna **NOTA OFICIAL**.
8.  Para visualizar informações sobre cada problema de configuração, na tabela **Problemas de configuração localizados**, clique na linha para o problema.
9.  Execute a ação corretiva para cada problema mostrado no relatório e reconstrua a imagem. Alguns problemas no Dockerfile podem ser resolvidos usando o código que é fornecido em [Resolvendo problemas nas imagens](#va_report).

Se existirem vulnerabilidades que não forem corrigidas, esses problemas poderão afetar a segurança dos contêineres construídos com essa imagem. No entanto, será possível continuar a usar uma imagem que tenha problemas de segurança e configuração em um contêiner.

 


## Revisando um relatório de vulnerabilidade usando a CLI
{: #va_registry_cli}

É possível revisar a segurança de imagens do Docker que estão localizadas em seus namespaces no {{site.data.keyword.registrylong_notm}} usando a CLI.
{:shortdesc}

1.  Liste as imagens em sua conta do {{site.data.keyword.Bluemix_notm}}. Uma lista de todas as imagens é retornada, independentemente do namespace no qual elas estão armazenadas.

    ```
    ibmcloud cr image-list
    ```
    {: pre}

2.  Verifique o status na coluna **STATUS DE SEGURANÇA**.
    -   `No Issues`: nenhum problema de segurança foi localizado.
    -   `X Issues`: potenciais problema de segurança ou vulnerabilidades foram localizados.
    -   `Scanning`: a imagem está sendo varrida e o status de vulnerabilidade final ainda não está determinado.
4.  Para visualizar os detalhes para o status, revise o relatório do Vulnerability Advisor.

    ```
    ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    Na saída da CLI, é possível visualizar as informações a seguir sobre os problemas de configuração.
      - Prática de segurança: uma descrição da vulnerabilidade que foi localizada
      - Ação corretiva: detalhes sobre como corrigir a vulnerabilidade


## Revisando um relatório de contêiner
{: #va_reviewing_container}

Em seu painel, é possível ver o status de um contêiner para determinar se sua segurança está em conformidade com a política de sua organização. Também é possível revisar o relatório de segurança de um contêiner, que detalha os pacotes vulneráveis, as configurações não seguras de contêiner ou de aplicativo e se o contêiner está em conformidade com as políticas organizacionais.
{:shortdesc}

Verifique no relatório de segurança se o contêiner tem a maior segurança possível e adote ações em quaisquer problemas relatados de segurança ou de configuração, concluindo as etapas a seguir:

1.  Selecione o contêiner para cujo relatório deseja visualizar:
    1.  No Catálogo, selecione **Contêineres**, clique em **Registro do contêiner**.
    2.  Selecione a guia **Repositórios privados** e selecione a linha para o repositório desejado.
    3.  Selecione a linha para a tag de imagem desejada.
    4.  Selecione a guia **Contêineres associados** e, em seguida, selecione a linha para o contêiner desejado. O relatório de segurança é aberto.
2.  Revise as seções para ver os problemas potenciais de segurança e de configuração de cada pacote na imagem:

      -   **Vulnerabilidades**: lista pacotes com problemas conhecidos de vulnerabilidade que são atualizados diariamente por meio dos avisos de segurança publicados para os tipos de imagem do Docker listados em [Gerenciando a segurança de imagens com o Vulnerability Advisor](va_index.html). Geralmente, para que um pacote vulnerável passe pela varredura, é necessário uma versão mais recente do pacote que inclua uma correção para a vulnerabilidade. O mesmo pacote pode listar várias vulnerabilidades e, nesse caso, um upgrade de pacote único pode corrigir vários problemas. Clique no código de aviso de segurança para revisar mais informações sobre o pacote e as etapas para atualizar o pacote.

    -   **Problemas de configuração**: lista sugestões para aumentar a segurança do contêiner e quaisquer configurações do aplicativo para o contêiner que não são seguras. Expanda a linha para visualizar como resolver o problema.

   São fornecidas ações corretivas ou sugestões para cada item listado.
   
3.  Revise o status da política para cada problema de segurança. O status da política indica se esse problema está isento.

    -  **Ativo**: você tem um problema que não está isento e está afetando o seu status de segurança.
    -  **Isento**: esse problema foi isento pelas suas configurações de política.
    -  **Parcialmente isento**: esse problema está associado a mais de um aviso de segurança. Os avisos de segurança não estão todos isentos.

4.  Decida como atualizar o contêiner para que seja possível resolver os problemas.

    **Importante:** para corrigir problemas com a imagem de contêiner, deve-se excluir a instância antiga e reimplementar, o que significa perder quaisquer dados dentro do contêiner existente. Assegure-se de que tenha um bom entendimento da arquitetura de seu contêiner para escolher o método apropriado de reimplementação do contêiner.

    Por exemplo:

    -   Se o seu contêiner for desacoplado dos dados que ele calcula, será possível parar o contêiner e excluí-lo, fazer as mudanças necessárias na imagem e reimplementar, sem perda de dados.
    -   É possível usar um serviço do {{site.data.keyword.Bluemix_notm}} para ajudar, como o [Delivery Pipeline](../ContinuousDelivery/pipeline_about.html), com a atualização da instância de contêiner vulnerável.
    -   Em uma arquitetura de microsserviços, é possível rotear o tráfego para outra instância de contêiner durante a correção dos problemas de segurança ou de configuração e enviar por push a nova imagem para uma implementação vermelho/preto.

5.  Se não for possível corrigir o problema no momento, será possível isentá-lo em suas configurações de política, o que o impedirá de bloquear a implementação do contêiner. Para isentar o problema, clique no ícone de **abrir e fechar a lista de opções** e clique em **Criar isenção**.

6.  Corrija os problemas descritos no relatório de **segurança** e reconstrua a imagem ou reimplemente o contêiner de acordo com o método escolhido. Alguns problemas no Dockerfile podem ser resolvidos usando o código que é fornecido em [Resolvendo problemas nas imagens](/docs/services/va/va_index.html#va_report).


## Resolvendo problemas comuns em imagens
{: #va_report}

Revise as correções de exemplo para problemas comuns que podem ser relatados pelo Vulnerability Advisor. É possível corrigir alguns problemas atualizando o Dockerfile.
{:shortdesc}


### Idade máxima da senha, dias mínimos da senha e comprimento mínimo da senha
{: #va_password}

**Problema**: você recebe uma ou mais das vulnerabilidades a seguir:

```
Maximum password age must be set to 90 days.
```
{: screen}

```
Minimum password length must be 8.
```
{: screen}

```
O mínimo de dias que deve decorrer entre mudanças de senha iniciadas pelo usuário deve ser 1.
```
{: screen}

**Correção**: configure a conformidade de senha incluindo o código a seguir no Dockerfile.

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```
{: codeblock}


### Vulnerabilidade do SSH
{: #ssh}

**Problema**: a vulnerabilidade a seguir é retornada:

```
SSH server should not be installed.
```
{: screen}

**Correção**: em vez de usar SSH, use `docker attach` ou `docker exec` para acessar seu contêiner. Assegure-se de que o Dockerfile não contenha nenhuma etapa para instalar um servidor SSH.
