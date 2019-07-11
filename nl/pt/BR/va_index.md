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


# Gerenciando a segurança de imagens com o Vulnerability Advisor
{: #va_index}

O Vulnerability Advisor verifica o status de segurança das imagens de contêiner que são fornecidas pela {{site.data.keyword.IBM}}, por terceiros ou que estão incluídas no namespace de registro de sua organização.
{:shortdesc}

Quando você inclui uma imagem em um namespace, ela é automaticamente varrida pelo Vulnerability Advisor para detectar problemas de segurança e potenciais vulnerabilidades. Se problemas de segurança forem localizados, instruções serão fornecidas para ajudá-lo a corrigir a vulnerabilidade relatada.

O Vulnerability Advisor fornece o gerenciamento de segurança para o [{{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started), gerando um relatório de status de segurança que inclui correções sugeridas e melhores práticas.

Quaisquer problemas encontrados pelo Vulnerability Advisor resultam em um veredito que indica que não é aconselhável implementar essa imagem. Se você escolher implementar a imagem, quaisquer contêineres que forem implementados usando a imagem incluirão problemas conhecidos que podem ser usados para atacar ou, de outra forma, comprometer o contêiner. O veredito é ajustado com base em qualquer isenção especificada. Essa avaliação pode ser usada pelo Container Image Security Enforcement para evitar a implementação de imagens não seguras no {{site.data.keyword.containerlong_notm}}.

A correção dos problemas de segurança e de configuração relatados pelo Vulnerability Advisor pode ajudar a proteger a infraestrutura do seu {{site.data.keyword.cloud_notm}}.

## Sobre o Vulnerability Advisor
{: #about}

O Vulnerability Advisor fornece funções para ajudar você a proteger as suas imagens.
{:shortdesc}

As funções a seguir estão disponíveis:

- Varreduras de imagens para problemas
- Fornece um relatório de avaliação que é baseado em práticas de segurança que são específicas para o {{site.data.keyword.containerlong_notm}}
- Fornece recomendações para proteger arquivos de configuração para um subconjunto de tipos de aplicativos
- Fornece instruções sobre como corrigir um [pacote vulnerável](#packages) ou [problema de configuração](#app_configurations) relatado em seus relatórios
- Fornece avaliações para o [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce)
- Aplica isenções para os relatórios em uma conta, um namespace, um repositório ou um nível de tag para marcar quando os problemas sinalizados não se aplicam ao seu caso de uso

No painel Registro, a coluna **Status da política** exibe o status de seus repositórios. O relatório vinculado identifica as boas práticas de segurança de nuvem para as suas imagens.

O painel do Vulnerability Advisor fornece uma visão geral e avaliação da segurança para uma imagem. Se desejar saber mais sobre o painel do Vulnerability Advisor, consulte [Revisando um relatório de vulnerabilidade](#va_reviewing).

**Proteção de Dados**

Para varrer imagens e contêineres em sua conta para buscar problemas de segurança, o Vulnerability Advisor coleta, armazena e processa as informações a seguir:

- Campos de formato livre, incluindo IDs, descrições e nomes de imagens (registro, namespace, nome do repositório e tag de imagem)
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

Os pacotes que contêm problemas de vulnerabilidade conhecidos são exibidos nos resultados da varredura. As possíveis vulnerabilidades são atualizadas diariamente usando os avisos de segurança publicados para os tipos de imagem do Docker listados na tabela a seguir. Geralmente, para que um pacote vulnerável passe pela varredura, é necessário uma versão mais recente do pacote que inclua uma correção para a vulnerabilidade. O mesmo pacote pode listar várias vulnerabilidades e, neste caso, uma única atualização de pacote pode tratar várias vulnerabilidades.

  |Imagem base Docker|Origem dos avisos de segurança|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://git.alpinelinux.org/) e [CIRCL CVE Search ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cve.circl.lu/).|
  |CentOS| [Archives de anúncio CentOS ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://lists.centos.org/pipermail/centos-announce/) e [Archives de anúncio CentOS CR ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://lists.centos.org/pipermail/centos-cr-announce/). Para obter mais informações sobre vulnerabilidades, consulte [Vulnerabilidades em pacotes no CentOS](#va_centos).|
  |Debian|[Anúncios de segurança Debian ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://lists.debian.org/debian-security-announce/).|
  |Red Hat Enterprise Linux (RHEL)|[API de dados de segurança do Red Hat ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://access.redhat.com/labsinfo/securitydataapi).|
  |Ubuntu|[Avisos de segurança do Ubuntu ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://usn.ubuntu.com/).|
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

### Revisando um relatório de vulnerabilidade usando a GUI
{: #va_reviewing_gui}

É possível revisar a segurança das imagens do Docker que estão armazenadas em seus namespaces no {{site.data.keyword.registrylong_notm}} usando a GUI.
{:shortdesc}

1. Efetue login no {{site.data.keyword.cloud_notm}}.
2. Clique no ícone **Menu de navegação** e, em seguida, clique em
**Kubernetes**.
3. Clique em **Registro** e, em seguida, clique no bloco **Imagens**. Uma lista de suas imagens e o status de segurança
de cada imagem é exibido na tabela **Imagens**.
4. Para ver o relatório para a imagem que está marcada como `latest`,
clique na linha para essa imagem. A guia **Detalhes da imagem** é aberta mostrando
os dados dessa imagem. Se nenhuma marcação `mais recente` existir no repositório, a imagem mais recente será usada.
5. Se o status de segurança mostrar quaisquer problemas, para descobrir quais são eles, clique
na guia **Problemas por tipo**. As tabelas **Vulnerabilidades** e **Problemas de configuração** são abertas.

   - Tabela **Vulnerabilidades**. Mostra o ID de vulnerabilidade para cada problema, o status da política para esse problema, os pacotes afetados e como resolver o problema. Para
ver mais informações sobre esse problema, expanda a linha. É exibido um resumo desse problema, com
um link para o aviso de segurança do fornecedor desse problema. Lista os pacotes que contêm problemas de vulnerabilidade conhecidos.
  
     A lista é atualizada diariamente usando avisos de segurança publicados para os tipos de imagem do Docker listados em [Tipos de vulnerabilidades](#types). Geralmente, para que um pacote vulnerável passe pela varredura, é necessário uma versão mais recente do pacote que inclua uma correção para a vulnerabilidade. O mesmo pacote pode listar várias vulnerabilidades e, neste caso, uma única atualização de pacote pode corrigir vários problemas. Clique no código de aviso de segurança para visualizar mais informações sobre o pacote e as etapas para atualizá-lo.

   - Tabela **Problemas de configuração**. Mostra o ID do problema de configuração para cada problema, o status da política para esse problema e a prática de segurança. Para
ver mais informações sobre esse problema, expanda a linha. É exibido um resumo desse problema com um link para o aviso de segurança dele.
  
     A lista contém sugestões de ações que podem ser tomadas para aumentar a segurança do
contêiner e quaisquer configurações de aplicativo para o contêiner que não são seguras. Expanda a linha para visualizar como resolver o problema.

6. Conclua a ação corretiva para cada problema mostrado no relatório e reconstrua a imagem.

### Revisando um relatório de vulnerabilidade usando a CLI
{: #va_registry_cli}

É possível revisar a segurança de imagens do Docker que estão localizadas em seus namespaces no {{site.data.keyword.registrylong_notm}} usando a CLI.
{:shortdesc}

1. Liste as imagens em sua conta do {{site.data.keyword.cloud_notm}}. Uma lista de todas as imagens é retornada, independentemente do namespace no qual elas estão armazenadas.

   ```
   ibmcloud cr image-list
   ```
   {: pre}

2. Verifique o status na coluna **STATUS DE SEGURANÇA**.
    - `Nenhum problema` Nenhum problema de segurança foi localizado.
    - `<X> Issues` O número de potenciais problemas de segurança ou vulnerabilidades localizados, em que `<X>` é o número de problemas.
    - `Scanning` A imagem está sendo varrida e o status final de vulnerabilidade não é determinado.

3. Para visualizar os detalhes do status, revise o relatório do Vulnerability Advisor:

   ```
   ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
   ```
   {: pre}

   Na saída da CLI, é possível visualizar as informações a seguir sobre os problemas de configuração.
      - `Prática de segurança` Uma descrição da vulnerabilidade que foi localizada
      - `Ação corretiva` Detalhes sobre como corrigir a vulnerabilidade

### Vulnerabilidades em pacotes no CentOS
{: #va_centos}

Se você estiver usando o CentOS, poderá obter falsos positivos em seu relatório, ou seja, o relatório pode relatar uma vulnerabilidade quando não há uma. Essa situação ocorre quando um aviso de segurança é liberado pelo Red Hat, mas o aviso de segurança não é aplicável ou a correção ainda não foi portada para o CentOS.
{:shortdesc}

Se você receber um relatório que diga que seu pacote tem vulnerabilidades, conclua as etapas a seguir:

1. Visualize as etapas para atualizar o pacote clicando no código de aviso de segurança.
2. Atualize o pacote concluindo as etapas para atualizar o pacote.
3. Se o pacote estiver atualizado, o resultado não era um falso positivo e a ação necessária está concluída.
4. Se o pacote não for atualizado porque nenhuma versão mais nova está disponível para instalação, o resultado era um falso positivo. É possível incluir uma política de isenção para esse aviso de segurança, consulte [Configurando políticas de isenção organizacional](#va_managing_policy).

## Configurando políticas de isenção organizacional
{: #va_managing_policy}

Se você desejar gerenciar a segurança de uma organização do {{site.data.keyword.cloud_notm}}, será possível usar sua configuração de política para determinar se um problema está isento ou não. É possível usar o Container Image Security Enforcement para assegurar que a implementação seja permitida apenas por meio de imagens que não contêm problemas de segurança após a contabilidade para qualquer problema isento por sua política.
{:shortdesc}

É possível implementar contêineres usando qualquer imagem, independentemente do status de segurança, a menos que o Container Image Security Enforcement esteja implementado em seu cluster. Para descobrir como implementar o Container Image Security Enforcement, consulte [Instalando o Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce).

Ao usar o Container Image Security Enforcement, qualquer problema de segurança que for detectado pelo Vulnerability Advisor evita que um contêiner seja implementado usando a imagem. Para permitir que uma imagem com problemas detectados seja implementada, as isenções devem ser incluídas em sua política.

### Configurando políticas de isenção organizacional usando a GUI
{: #va_managing_policy_gui}

Se você desejar configurar isenções para a política usando a GUI, conclua as etapas a seguir:

1. Efetue login no {{site.data.keyword.cloud_notm}}. Deve-se efetuar login para ver o Vulnerability Advisor na GUI.
2. Clique no ícone **Menu de navegação** e, em seguida, clique em
**Kubernetes**.
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
