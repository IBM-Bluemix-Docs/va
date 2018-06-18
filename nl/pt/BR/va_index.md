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


# Gerenciando a segurança de imagens com o Vulnerability Advisor
{: #va_index}

O Vulnerability Advisor verifica o status de segurança das imagens de contêiner que são fornecidas pela IBM, por terceiros ou são incluídas no namespace do registro de sua organização.{:shortdesc}


Quando você inclui uma imagem em um namespace, ela é automaticamente varrida pelo Vulnerability Advisor para detectar problemas de segurança e potenciais vulnerabilidades. Se problemas de segurança forem localizados, instruções serão fornecidas para ajudá-lo a corrigir a vulnerabilidade relatada. Ainda é possível implementar contêineres de imagens vulneráveis, mas tenha em mente que eles podem ser atacados e estar comprometidos.


## Sobre o Vulnerability Advisor
{: #about}

O Vulnerability Advisor fornece gerenciamento de segurança para o {{site.data.keyword.containerlong}}. O Vulnerability Advisor gera um
relatório de status de segurança, sugere correções e melhores práticas, além de fornecer gerenciamento para restringir
a execução de imagens não seguras. A correção dos problemas de segurança e configuração que são relatados pelo Vulnerability Advisor pode ajudar a proteger a infraestrutura do {{site.data.keyword.cloud_notm}}.
{:shortdesc}

O Vulnerability Advisor inclui os recursos a seguir:

-   Varre imagens em busca de vulnerabilidades
-   Fornece um relatório de avaliação que é baseado em práticas de segurança que são específicas para o {{site.data.keyword.containerlong_notm}}
-   Fornece recomendações para proteger arquivos de configuração para um subconjunto de tipos de aplicativos
-   Fornece instruções sobre como corrigir um [pacote vulnerável](#packages) ou [problema de configuração](#app_configurations) relatado em seus relatórios

No painel Registro, a coluna **RELATÓRIO DE SEGURANÇA** exibe o status dos seus repositórios. O relatório identifica boas práticas de segurança em nuvem para suas imagens. 

O painel do Vulnerability Advisor fornece uma visão geral e avaliação da segurança para uma imagem. Para descobrir mais sobre o painel do Vulnerability Advisor, consulte [Revisando um relatório de
vulnerabilidade](#va_reviewing).
	
	
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

Os pacotes com problemas de vulnerabilidade conhecida são exibidos nos resultados de varredura. As vulnerabilidades possíveis são atualizadas diariamente
a partir de avisos de segurança publicados para os tipos de imagem do Docker que são listados na tabela a seguir. Geralmente, para que um pacote vulnerável passe pela varredura, é necessário uma
versão mais recente do pacote que inclua uma correção para a vulnerabilidade. O mesmo pacote pode listar múltiplas vulnerabilidades e,
nesse caso, um upgrade de pacote único pode tratar de múltiplas vulnerabilidades.


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
{: #va_install_livescan}

Antes de iniciar:

1.  Efetue login no {{site.data.keyword.Bluemix_notm}} CLI do cliente. Se você tiver uma conta federada, use `--sso`.
2.  [Destine sua CLI do `kubectl`](../../containers/cs_cli_install.html#cs_cli_configure) para o cluster no qual você deseja usar um gráfico Helm.
3.  Crie um ID do serviço e uma chave API para o scanner de contêiner e dê a ele um nome:
    1.  Crie um ID do serviço executando o comando a seguir, substituindo `<scanner_serviceID>` por um nome de sua escolha para o ID do serviço. Observe o **CRN**.
    
        ```
    	bx iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  Crie uma chave API de serviço, em que `<scanner_serviceID>` é o ID do serviço criado na etapa anterior, e substitua `<scanner_APIkey_name>` por um nome de sua escolha para a chave API do scanner. 
    
        ```
    	bx iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	    A chave API do scanner é retornado.
	
	    Assegure-se de armazenar sua chave API do scanner com segurança porque ela não pode ser recuperada posteriormente.
	    {: tip}
	
    3.  Crie uma política de serviço que conceda a função `Writer`.
    		
        ```
    	bx iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
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
    <td>Insira a URL de terminal regional do Vulnerability Advisor. Para obter a URL, execute <code>bx cr info</code> e recupere o endereço do <strong>Registro do contêiner</strong>. Substitua <code>registry</code> por <code>va</code>. Por exemplo: <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>Substitua pelo ID da conta do {{site.data.keyword.Bluemix_notm}} em que seu cluster está. Para obter o ID da conta, execute <code>bx account list</code>.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>Substitua pelo cluster do Kubernetes em que deseja instalar o scanner do contêiner. Para listar os IDs do cluster, execute <code>bx cs clusters</code>.</td>
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
    
    **Nota**: o scanner do contêiner é instalado no namespace `kube-system`, mas varre os contêineres de todos os namespaces.

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


O IBM Container Scanner está agora instalado e o agente livescan é implementado como um [DaemonSet ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) em seu cluster. Embora o scanner esteja implementado no namespace `kube-system`, ele varre todos os contêineres que estão designados a pods em todos os seus namespaces do Kubernetes, como `default`. 



## Revisando um relatório de vulnerabilidade
{: #va_reviewing}

Antes de implementar uma imagem, é possível revisar seu relatório do Vulnerability Advisor para obter detalhes sobre quaisquer pacotes vulneráveis e configurações de app não seguro.
{:shortdesc}



1.  Efetue login no {{site.data.keyword.Bluemix_notm}}.
2.  Clique em **Catálogo**.
3.  Em **Infraestrutura**, clique em **Contêineres**.
4.  Clique no azulejo **Registro do contêiner**.
5.  Expanda **Vulnerability Advisor** e clique em **Repositórios varridos**.
6.  Para ver o relatório para a imagem que é marcada como `mais recente`, clique na linha para esse repositório. O relatório mostra o número total de problemas e se eles são pacotes vulneráveis ou problemas de configuração. Se nenhuma marcação
`mais recente` existir no repositório, a imagem mais recente será usada.
7.  Para visualizar informações sobre cada pacote vulnerável para a imagem selecionada, na tabela **Pacotes vulneráveis localizados**, clique no link na coluna **VULNERABILIDADES** para abrir o relatório.
    1.  Para ver mais informações, expanda o resumo.
    2.  Se o aviso de um distribuidor de sistema operacional for fornecido, clique no link na coluna **NOTA OFICIAL**.
8.  Para visualizar informações sobre cada problema de configuração, na tabela **Problemas de configuração localizados**, clique na linha para o problema.
9.  Execute a ação corretiva para cada problema mostrado no relatório e reconstrua a imagem. Alguns problemas no Dockerfile
podem ser resolvidos usando o código que é fornecido em [Resolvendo problemas nas imagens](#va_report).

Se existirem vulnerabilidades que não forem corrigidas, esses problemas poderão afetar a segurança dos contêineres construídos com essa imagem. No entanto, será possível continuar a usar uma imagem que tenha problemas de segurança e configuração em um contêiner.

 



## Revisando um relatório de vulnerabilidade usando a CLI
{: #va_registry_cli}

É possível revisar a segurança de imagens do Docker que estão localizadas em seus namespaces no {{site.data.keyword.registrylong_notm}} usando a CLI.
{:shortdesc}

1.  Liste as imagens em sua conta do {{site.data.keyword.Bluemix_notm}}. Uma lista de todas as imagens é retornada, independentemente do namespace no qual elas estão armazenadas.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Verifique o status na coluna **STATUS DE SEGURANÇA**.
    -   `No Issues`: nenhum problema de segurança foi localizado.
    -   `X Issues`: potenciais problema de segurança ou vulnerabilidades foram localizados.
    -   `Scanning`: a imagem está sendo varrida e o status de vulnerabilidade final ainda não está determinado.
4.  Para visualizar os detalhes para o status, revise o relatório do Vulnerability Advisor.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    Na saída da CLI, é possível visualizar as informações a seguir sobre os problemas de configuração.
      - Prática de segurança: uma descrição da vulnerabilidade que foi localizada
      - Ação corretiva: detalhes sobre como corrigir a vulnerabilidade


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

**Correção**: em vez de usar SSH, use `docker attach` ou `docker exec`
para acessar seu contêiner. Assegure-se de que o Dockerfile não contenha nenhuma etapa para instalar um servidor SSH.
