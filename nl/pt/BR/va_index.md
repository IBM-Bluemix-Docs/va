---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-25"

keywords: security status of container images, image security, Vulnerability Advisor, security, registry, vulnerabilities

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

# Gerenciando a segurança de imagens com o Vulnerability Advisor
{: #va_index}

O Vulnerability Advisor verifica o status de segurança das imagens de contêiner que são fornecidas pela {{site.data.keyword.IBM}}, por terceiros ou que estão incluídas no namespace de registro de sua organização. Se o Container Scanner estiver instalado em cada cluster, o Vulnerability Advisor verificará também o status dos contêineres que estão em execução.
{:shortdesc}

Quando você inclui uma imagem em um namespace, ela é automaticamente varrida pelo Vulnerability Advisor para detectar problemas de segurança e potenciais vulnerabilidades. Se problemas de segurança forem localizados, instruções serão fornecidas para ajudá-lo a corrigir a vulnerabilidade relatada.

O Vulnerability Advisor fornece o gerenciamento de segurança para o [{{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-index#index), gerando um relatório de status de segurança que inclui correções sugeridas e melhores práticas.

Quaisquer problemas encontrados pelo Vulnerability Advisor resultam em um veredito que indica que não é aconselhável implementar essa imagem. Se você escolher implementar a imagem, quaisquer contêineres que forem implementados usando a imagem incluirão problemas conhecidos que podem ser usados para atacar ou, de outra forma, comprometer o contêiner. O veredito é ajustado com base em qualquer isenção especificada. Essa avaliação pode ser usada pelo Container Image Security Enforcement para evitar a implementação de imagens não seguras no {{site.data.keyword.containerlong_notm}}.

A correção dos problemas de segurança e de configuração relatados pelo Vulnerability Advisor pode ajudar a proteger a infraestrutura do seu {{site.data.keyword.cloud_notm}}.

## Sobre o Vulnerability Advisor
{: #about}

O Vulnerability Advisor fornece funções para ajudar você a proteger as suas imagens.
{:shortdesc}

As funções a seguir estão disponíveis:

- Varreduras de imagens para problemas
- Verifica problemas em contêineres que estão em execução se um [Container Scanner](#va_install_container_scanner) está instalado em cada cluster
- Fornece um relatório de avaliação que é baseado em práticas de segurança que são específicas para o {{site.data.keyword.containerlong_notm}}
- Fornece recomendações para proteger arquivos de configuração para um subconjunto de tipos de aplicativos
- Fornece instruções sobre como corrigir um [pacote vulnerável](#packages) ou [problema de configuração](#app_configurations) relatado em seus relatórios
- Fornece avaliações para o [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce)
- Aplica isenções para os relatórios em uma conta, um namespace, um repositório ou um nível de tag para marcar quando os problemas sinalizados não se aplicam ao seu caso de uso
- Fornece links para contêineres associados na visualização **Tag** da interface gráfica com o usuário do {{site.data.keyword.registrylong_notm}}. É possível listar os contêineres que estão em execução e que estão usando essa imagem em um cluster no qual o Container Scanner está instalado.

No painel Registro, a coluna **Status da política** exibe o status de seus repositórios. O relatório vinculado identifica as boas práticas de segurança de nuvem para as suas imagens.

O painel do Vulnerability Advisor fornece uma visão geral e uma avaliação de segurança para uma imagem e, se o Container Scanner estiver instalado, os links para os contêineres que estão em execução. Se desejar saber mais sobre o painel do Vulnerability Advisor, consulte [Revisando um relatório de vulnerabilidade](#va_reviewing).

**Proteção de Dados**

Para varrer imagens e contêineres em sua conta para buscar problemas de segurança, o Vulnerability Advisor coleta, armazena e processa as informações a seguir:

- Campos de formato livre, incluindo IDs, descrições e nomes de imagens (registro, namespace, nome do repositório e tag de imagem)
- Metadados do Kubernetes, incluindo nomes de recursos do Kubernetes, como pod, ReplicaSet e nomes de implementação
- Metadados sobre os modos de arquivo e os registros de data e hora de criação dos arquivos de configuração
- O conteúdo dos arquivos de configuração do sistema e do aplicativo em imagens e contêineres
- Pacotes e bibliotecas instalados (incluindo suas versões)

Não coloque informações pessoais em nenhum campo ou local que o Vulnerability Advisor processa, conforme identificado na lista anterior.

Os resultados de varredura, agregados em um nível de data center, são processados para produzir métricas anonimadas para operar e melhorar o serviço.

Os resultados de varredura são excluídos 30 dias após serem gerados.

## Tipos de vulnerabilidades
{: #types}

### Pacotes vulneráveis
{: #packages}

O Vulnerability Advisor verifica pacotes vulneráveis em imagens que estão usando sistemas operacionais suportados e fornece um link para qualquer aviso de segurança relevante sobre a vulnerabilidade.
{:shortdesc}

Os pacotes que contêm problemas de vulnerabilidade conhecidos são exibidos nos resultados da varredura. As possíveis vulnerabilidades são atualizadas diariamente usando os avisos de segurança publicados para os tipos de imagem do Docker listados na tabela a seguir. Geralmente, para que um pacote vulnerável passe pela varredura, é necessário uma versão mais recente do pacote que inclua uma correção para a vulnerabilidade. O mesmo pacote pode listar múltiplas vulnerabilidades e, nesse caso, o upgrade de um único pacote pode abordar várias vulnerabilidades.

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

As imagens serão varridas apenas se estiverem usando um sistema operacional suportado pelo Vulnerability Advisor. O Vulnerability Advisor verifica as definições de configuração para os tipos de apps a seguir:

- MySQL
- NGINX
- Apache

## Revisando um relatório de vulnerabilidade
{: #va_reviewing}

Antes de implementar uma imagem, é possível revisar o relatório do Vulnerability Advisor para obter detalhes sobre pacotes vulneráveis e configurações não seguras de contêiner ou de aplicativo. Também é possível verificar se a imagem está em conformidade com as políticas organizacionais.
{:shortdesc}

Se você não resolver nenhum dos problemas descobertos, esses problemas poderão afetar a segurança dos contêineres que são criados usando essa imagem. Se o Container Image Security Enforcement não for implementado, será possível continuar a usar uma imagem com problemas de segurança e de configuração em um contêiner. Se o Container Image Security Enforcement estiver implementado e ativo para a imagem, todos os problemas descobertos deverão ser isentos pela política para que os contêineres possam ser implementados usando essa imagem.

Para configurar o escopo de cumprimento de problemas do Vulnerability Advisor no Container Image Security Enforcement, consulte [Customizando políticas](/docs/services/Registry?topic=registry-security_enforce#customize_policies).
{:tip}

Caso a sua imagem não atenda aos requisitos que são configurados pela política de sua organização, deve-se configurar a imagem para atender a esses requisitos antes de poder implementá-la. Para obter mais informações sobre como visualizar e mudar a política de organização, consulte [Configurando políticas de isenção organizacional](#va_managing_policy).
{:tip}

Se o Container Scanner for implementado, após a implementação da imagem, o Vulnerability Advisor continuará verificando problemas de segurança e de configuração no contêiner. É possível resolver quaisquer problemas encontrados seguindo as etapas que estão descritas em [Revisando um relatório de contêiner](#va_reviewing_container).

### Revisando um relatório de vulnerabilidade usando a GUI
{: #va_reviewing_gui}

É possível revisar a segurança das imagens do Docker que estão armazenadas em seus namespaces no {{site.data.keyword.registrylong_notm}} usando a GUI.
{:shortdesc}

1. Efetue login no {{site.data.keyword.Bluemix_notm}}.
2. Clique no ícone **Menu de navegação** e, em seguida, clique em
**Kubernetes**.
3. Clique em **Registro** e, em seguida, clique no bloco **Imagens**. Uma lista de suas imagens e o status de segurança
de cada imagem é exibido na tabela **Imagens**.
4. Para ver o relatório para a imagem que está marcada como `latest`,
clique na linha para essa imagem. A guia **Detalhes da imagem** é aberta mostrando
os dados dessa imagem. Se nenhuma marcação `mais recente` existir no repositório, a imagem mais recente será usada.
5. Se o status de segurança mostrar quaisquer problemas, para descobrir quais são eles, clique
na guia **Problemas por tipo**. As tabelas **Vulnerabilidades** e **Problemas de configuração** são abertas.

   - **Vulnerabilidades** Essa tabela mostra o ID de vulnerabilidade para cada problema, o status da política para esse problema, os pacotes afetados e como resolver o problema. Para
ver mais informações sobre esse problema, expanda a linha. É exibido um resumo desse problema, com
um link para o aviso de segurança do fornecedor desse problema. Lista os pacotes que contêm problemas de vulnerabilidade conhecidos.
  
     A lista é atualizada diariamente usando avisos de segurança publicados para os tipos de imagem do Docker listados em [Tipos de vulnerabilidades](#types). Geralmente, para que um pacote vulnerável passe pela varredura, é necessário uma versão mais recente do pacote que inclua uma correção para a vulnerabilidade. O mesmo pacote pode listar várias vulnerabilidades. Nesse caso, o upgrade de um único pacote pode corrigir vários problemas. Clique no código de aviso de segurança para visualizar mais informações sobre o pacote e as etapas para atualizá-lo.

   - **Problemas de configuração** Essa tabela mostra o ID do problema de
configuração para cada problema, o status da política para esse problema e a prática de segurança. Para
ver mais informações sobre esse problema, expanda a linha. É exibido um resumo desse problema com um link para o aviso de segurança dele.
  
     A lista contém sugestões de ações que podem ser tomadas para aumentar a segurança do
contêiner e quaisquer configurações de aplicativo para o contêiner que não são seguras. Expanda a linha para visualizar como resolver o problema.

6. Conclua a ação corretiva para cada problema mostrado no relatório e reconstrua a imagem.

### Revisando um relatório de vulnerabilidade usando a CLI
{: #va_registry_cli}

É possível revisar a segurança de imagens do Docker que estão localizadas em seus namespaces no {{site.data.keyword.registrylong_notm}} usando a CLI.
{:shortdesc}

1. Liste as imagens em sua conta do {{site.data.keyword.Bluemix_notm}}. Uma lista de todas as imagens é retornada, independentemente do namespace no qual elas estão armazenadas.

   ```
   ibmcloud cr image-list
   ```
   {: pre}

2. Verifique o status na coluna **STATUS DE SEGURANÇA**.
    - **Nenhum problema** Nenhum problema de segurança foi localizado.
    - **`<X>`Problemas** `<X>` problemas de segurança ou vulnerabilidades em potencial foram localizados, em que `<X>`  é o número de problemas.
    - **Varrendo** A imagem está sendo varrida e o status de vulnerabilidade final ainda não foi determinado.

3. Para visualizar os detalhes do status, revise o relatório do Vulnerability Advisor:

   ```
   ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
   ```
   {: pre}

   Na saída da CLI, é possível visualizar as informações a seguir sobre os problemas de configuração.
      - **Prática de segurança** Uma descrição da vulnerabilidade que foi localizada
      - **Ação corretiva** Detalhes sobre como corrigir a vulnerabilidade

## Configurando políticas de isenção organizacional
{: #va_managing_policy}

Se você desejar gerenciar a segurança de uma organização do {{site.data.keyword.Bluemix_notm}}, será possível usar sua configuração de política para determinar se um problema está isento ou não. É possível optar por usar o Container Image Security Enforcement para garantir que a implementação seja permitida apenas em imagens que não contêm problemas de segurança após a contabilidade de quaisquer problemas isentos pela sua política.
{:shortdesc}

É possível implementar contêineres usando qualquer imagem, independentemente do status de segurança, a menos que o Container Image Security Enforcement esteja implementado em seu cluster. Para descobrir como implementar o Container Image Security Enforcement, consulte [Instalando o Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce).

Ao usar o Container Image Security Enforcement, qualquer problema de segurança que for detectado pelo Vulnerability Advisor evita que um contêiner seja implementado usando a imagem. Para permitir que uma imagem com problemas detectados seja implementada, as isenções devem ser incluídas em sua política.

### Configurando políticas de isenção organizacional usando a GUI
{: #va_managing_policy_gui}

Se você desejar configurar isenções para a política usando a GUI, conclua as etapas a seguir:

1. Efetue login no {{site.data.keyword.Bluemix_notm}}. Deve-se efetuar login para ver o Vulnerability Advisor na GUI.
2. Clique no ícone **Menu de navegação** e, em seguida, clique em **Kubernetes**.
3. Em  ** Vulnerability Advisor **, clique em  ** Configurações de política **.
4. Clique em **Criar isenção**.
5. Selecione o tipo de problema.
6. Insira o ID do problema.

   É possível localizar essas informações em seu [relatório de vulnerabilidade](#va_reviewing). A coluna **ID de Vulnerabilidade** contém o ID a ser usado para problemas de aviso de CVE ou de segurança; a coluna **ID do problema de configuração** contém o ID a ser usado para problemas de configuração.
   {: tip}

7. Selecione o namespace de registro, o repositório e a tag para os quais você deseja que a isenção se aplique.
8. Clique **Salvar.**

Também é possível editar e remover isenções ao passar o mouse sobre a linha relevante e clicar no ícone **abrir e fechar a lista de opções**.

### Configurando políticas de isenção organizacional usando a CLI
{: #va_managing_policy_cli}

Se você desejar configurar isenções para a política usando a CLI, será possível executar os comandos a seguir:

- Para criar uma isenção para um problema de segurança, execute o comando [`ibmcloud cr exemption-add`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_add).
- Para listar suas isenções para problemas de segurança, execute o comando [`ibmcloud cr exemption-list`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_list).
- Para listar os tipos de problemas de segurança que você pode isentar, execute o comando [`ibmcloud cr exemption-types`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_types).
- Para excluir uma isenção para um problema de segurança, execute o comando [`ibmcloud cr exemption-rm`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_rm).

Para obter mais informações sobre os comandos, é possível usar a sinalização `-- help` ao executar o comando.

## Instalando o Scanner do Contêiner
{: #va_install_container_scanner}

O Container Scanner permite que o Vulnerability Advisor relate quaisquer problemas localizados em contêineres em execução que não estão presentes na imagem base do contêiner. Se você não fizer modificações de tempo de execução em seu contêiner, o Container Scanner não será necessário porque o relatório de imagem mostrará os mesmos problemas.
{:shortdesc}

Para verificar o status de segurança de contêineres em tempo real que estão em execução em seu cluster, é possível instalar o Container Scanner. Para proteger seu app, o Container Scanner varre regularmente seus contêineres em execução para que seja possível detectar e retificar quaisquer vulnerabilidades detectadas recentemente.

É possível configurar o Container Scanner para monitorar as vulnerabilidades nos contêineres que são designados aos pods em todos os namespaces do Kubernetes. Quando as vulnerabilidades são localizadas, deve-se retificar quaisquer problemas com a imagem e, em seguida, reimplementar seu app. O Container Scanner suporta somente contêineres que são criados por meio de imagens que estão armazenadas no {{site.data.keyword.registrylong_notm}}.

Para usar o Container Scanner, deve-se configurar as permissões e, em seguida, configurar um [Gráfico Helm ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.helm.sh/developing_charts) e associá-lo ao cluster no qual você deseja usá-lo.

### Configurar permissões de serviço para o Container Scanner
{: #va_install_container_scanner_permissions}

O Container Scanner requer que as permissões sejam configuradas para que o serviço possa operar.
{:shortdesc}

Para configurar permissões de serviço, conclua as etapas a seguir:

1. Efetue login no {{site.data.keyword.Bluemix_notm}} CLI do cliente. Se você tiver uma conta federada, use `--sso`.
2. [Destine a CLI
kubectl](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) ao cluster no qual você deseja usar um gráfico do Helm.
3. Crie um ID de serviço e uma chave de API para o Container Scanner e atribua um nome a ele:
    1. Para criar um ID de serviço, execute o comando a seguir, em que `<scanner_serviceID>` é um nome de sua escolha para o ID do serviço. Observe o **CRN**.

       ```
       ibmcloud iam service-id-create <scanner_serviceID>
       ```
       {: codeblock}

    2. Crie uma chave API de serviço, em que `<scanner_serviceID>` é o ID do serviço que você criou na etapa anterior e `<scanner_APIkey_name>` é um nome de sua escolha para a chave API do scanner.

       ```
       ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
       ```
       {: codeblock}
       A chave API do scanner é retornado.

       Assegure-se de armazenar sua chave API do scanner com segurança porque ela não pode ser recuperada posteriormente. Além disso, assegure-se de que você tenha uma chave de API de serviço separada para cada cluster no qual o scanner está instalado.
       {: tip}

    3. Crie uma política de serviço que conceda a função `Writer`.

       ```
       ibmcloud iam service-policy-create --resource-type scaningress --service-name container-registry --roles Writer <scanner_serviceID>
       ```
       {: codeblock}

### Configurar o gráfico Helm
{: #va_install_container_scanner_helm}

Configure um gráfico Helm e associe-o ao cluster no qual deseja usá-lo.
{:shortdesc}

Para configurar um gráfico Helm, conclua as etapas a seguir:

1. [Configure o Helm no IBM Cloud Kubernetes Service](/docs/containers?topic=containers-integrations#helm). Se você usar uma política de controle de acesso baseado na função (RBAC) para conceder acesso ao Tiller, assegure-se de que a função do Tiller tenha acesso a todos os namespaces. Fornecer à função do Tiller acesso a todos os namespaces assegura que o Container Scanner possa observar contêineres em todos os namespaces.

2. Inclua o repositório de gráficos da IBM em seu Helm, como `ibm`.

   ```
   helm repo add ibm https://icr.io/helm/ibm
   ```
   {: pre}

3. Salve as definições de configuração padrão para o gráfico Helm do Container Scanner em um arquivo YAML local. Inclua o repositório de gráficos, como `ibm`, no caminho do gráfico do Helm.

   ```
   helm inspect values ibm/ibmcloud-container-scanner > config.yaml
   ```
   {: pre}

4. Edite o arquivo `config.yaml`.

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
   <caption>Tabela 2. Entendendo os componentes de arquivo YAML</caption>
   <thead>
   <th>Campo</th>
   <th>Valor</th>
   </thead>
   <tbody>
   <tr>
   <td><code>EmitURL</code></td>
   <td>Insira a URL de terminal regional do Vulnerability Advisor. Para obter a URL, execute <code>ibmcloud cr info</code> e recupere o endereço do <strong>Registro do contêiner</strong>. Por exemplo, <code>https<span comment="make the link not a link">://uk.</span>icr.io</code>. Inclua <code>/va</code> ao final deste endereço. Por exemplo, <code>https<span comment="make the link not a link">://uk.</span>icr.io/va</code></td>
   </tr>
   <tr>
   <td><code>AccountID</code></td>
   <td>Substitua <code>AccountID</code> pelo ID da conta do {{site.data.keyword.Bluemix_notm}} em que seu cluster está. Para obter o ID da conta, execute <code>ibmcloud account list</code>.</td>
   </tr>
   <tr>
   <td><code>ClusterID</code></td>
   <td>Substitua <code>ClusterID</code> pelo cluster do Kubernetes em que você deseja instalar o seu Container Scanner. Para listar os IDs do cluster, execute <code>ibmcloud ks clusters</code>. <br> **Dica:** use o ID do cluster, não o nome.
   </td>
   </tr>
   <tr>
   <td><code>Chave</code></td>
   <td>Substitua <code>APIKey</code> pela chave API do scanner que você criou anteriormente.</td>
   </tr>
   </tbody></table>

5. Instale o gráfico Helm em seu cluster com o arquivo `config.yaml` atualizado. As propriedades atualizadas são armazenadas em um configmap para seu gráfico. Substitua `<myscanner>` por um nome de sua escolha para seu gráfico Helm. Inclua o repositório de gráficos, como `ibm`, no caminho do gráfico do Helm.

   ```
   helm install -f config.yaml --name=<myscanner> ibm/ibmcloud-container-scanner
   ```
   {: pre}

   O Container Scanner está instalado no namespace `kube-system`, mas varre contêineres de todos os namespaces.
   {:tip}

6. Verifique o status de implementação do gráfico. Quando o gráfico estiver pronto, o campo **STATUS** terá um valor de `DEPLOYED`.

   ```
   helm status <myscanner>
   ```
   {: pre}

7. Depois que o gráfico for implementado, verifique se as configurações atualizadas no arquivo `config.yaml` foram usadas.

   ```
   helm get values <myscanner>
   ```
   {: pre}

O Container Scanner agora está instalado e o agente é implementado como um [DaemonSet![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) em seu cluster. Embora o Container Scanner esteja implementado no namespace `kube-system`, ele varre todos os contêineres que são designados a pods em todos os namespaces do Kubernetes, tal como `default`.

## Executando o Container Scanner por trás de um firewall
{: #va_firewall}

Caso o firewall bloqueie conexões de saída, deve-se configurá-lo para permitir que os nós do trabalhador acessem o Container Scanner na porta TCP `443` nos endereços IP na tabela a seguir.
{:shortdesc}

 

<p>
  <table summary=" As linhas devem ser lidas da esquerda para a direita, com o local do servidor na coluna um e os endereços IP a serem correspondidos na coluna dois.">
  <caption>Tabela 3. Endereços IP a serem abertos para o tráfego de saída</caption>
    <thead>
      <th>Endereço</th>
      <th>Endereço IP</th>
    </thead>
    <tbody>
      <tr>
        <td>Dallas</td>
        <td><code> 169.47.103.118 </code><br><code> 169.48.165.6 </code></td>
      </tr>
      <tr>
         <td>Frankfurt</td>
         <td><code> 159.8.220.182 </code><br><code> 158.177.74.102 </code></td>
      </tr>
      <tr>
        <td>London</td>
        <td><code> 158.175.71.134 </code><br><code> 5.10.111.190 </code></td>
      </tr>
      <tr>
         <td>Sidney</td>
         <td><code> 168.1.40.158 </code><br><code> 130.198.65.182 </code></td>
      </tr>
      <tr>
        <td>Washington DC</td>
         <td><code> 169.60.73.158 </code><br><code> 169.61.84.102 </code></td>
      </tr>
    </tbody>
  </table>
</p>

## Revisando um relatório de contêiner
{: #va_reviewing_container}

Em seu painel, é possível ver o status de um contêiner para determinar se sua segurança está em conformidade com a política de sua organização. Também é possível revisar o relatório de segurança de um contêiner, o qual
detalha quaisquer pacotes vulneráveis e as configurações não seguras de contêiner ou de aplicativo e se o
contêiner é compatível com as políticas organizacionais.
{:shortdesc}

Verifique se os contêineres que estão em execução em seu espaço continuam a ser compatíveis com a política organizacional revisando o campo **Status da política**. O status é exibido como uma das condições a seguir:

- **Compatível com a política** Nenhum problema de segurança ou de configuração foi localizado.
- **Não compatível com a política** O Vulnerability Advisor localizou problemas de segurança ou de configuração em potencial que fizeram com que o contêiner não fosse
compatível com a política. Se sua política organizacional permitir a implementação de imagens vulneráveis, a imagem poderá ser implementada no estado `Deploy with Caution` e um aviso será enviado ao usuário que a implementou.
- **Avaliação incompleta** A varredura não foi concluída. A varredura ainda pode estar em execução ou o sistema operacional daquele contêiner pode não ser compatível.

Verifique se o seu contêiner está o mais seguro possível visualizando seu relatório de segurança
e tome ação com relação a qualquer problema de segurança ou de configuração relatado concluindo as etapas a seguir:

1. Selecione o contêiner para cujo relatório deseja visualizar:
    1. Clique no ícone **Menu de navegação** e, em seguida, clique em **Kubernetes**.
    2. Clique em **Registro** e, em seguida, clique no bloco **Repositórios** e expanda a
linha para o repositório desejado.
    3. Selecione a linha para a imagem desejada.
    4. Selecione a guia **Contêineres associados** e, em seguida, selecione a linha para o contêiner desejado. O relatório de segurança é aberto.
2. Revise as seções para ver os problemas potenciais de segurança e de configuração de cada pacote na imagem:

    - **Vulnerabilidades** Lista pacotes que contêm problemas conhecidos
de vulnerabilidade. A lista é atualizada diariamente usando avisos de segurança publicados para os tipos de imagem do Docker listados em [Tipos de vulnerabilidades](#types). Geralmente, para que um pacote vulnerável passe pela varredura, é necessário uma versão mais recente do pacote que inclua uma correção para a vulnerabilidade. O mesmo pacote pode listar várias vulnerabilidades. Nesse caso, o upgrade de um único pacote pode corrigir vários problemas. Clique no código de aviso de segurança para visualizar mais informações sobre o pacote e as etapas para atualizá-lo.

    - **Problemas de configuração** Lista as sugestões que podem ser tomadas
para aumentar a segurança do contêiner e quaisquer configurações de aplicativo para o contêiner que
não são seguras. Expanda a linha para visualizar como resolver o problema.

   São fornecidas ações corretivas ou sugestões para cada item listado.

3. Revise o status da política para cada problema de segurança. O status da política indica se esse problema está isento.

    - **Ativo** Você tem um problema que não é isento e o problema está
afetando seu status de segurança.
    - **Isento** Esse problema é isento por suas configurações de política.
    - **Parcialmente isento** Esse problema está associado a mais de um aviso de segurança. Os avisos de segurança não estão todos isentos.

4. Decida como atualizar o contêiner para que seja possível resolver os problemas.

    **Importante** Para corrigir problemas com a imagem do contêiner, deve-se excluir a instância antiga e fazer a reimplementação, o que significa perder quaisquer dados dentro do contêiner
existente. Assegure-se de que tenha um bom entendimento da arquitetura de seu contêiner para escolher o método apropriado de reimplementação do contêiner.

    **Exemplo**

    - Se o seu contêiner for desacoplado dos dados que ele calcula, será possível parar o contêiner e excluí-lo, fazer as mudanças necessárias na imagem e reimplementar, sem perda de dados.
    - É possível usar um serviço do {{site.data.keyword.Bluemix_notm}}, tal como [Delivery Pipeline](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-deliverypipeline_about#deliverypipeline_about), para ajudar na atualização da instância de contêiner vulnerável.
    - Em uma arquitetura de microsserviços, é possível rotear o tráfego para outra instância de
contêiner enquanto corrige problemas de segurança ou de configuração e envia a nova imagem por push em
uma implementação vermelho-preto.

5. Se não for possível corrigir o problema agora, será possível isentá-lo nas configurações da política, o que impede que o problema bloqueie a implementação do contêiner. Para isentar o problema, clique no ícone **abrir e fechar a lista de opções** e clique em **Criar isenção**, consulte [Configurando políticas de isenção organizacional](#va_managing_policy).

6. Corrija os problemas descritos no relatório de **segurança** e reconstrua a imagem ou reimplemente o contêiner de acordo com o método escolhido.
