---

copyright:
  years: 2017
lastupdated: "2017-12-05"

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

O Vulnerability Advisor verifica o status de segurança das imagens de contêiner antes da implementação.
{:shortdesc}


## Sobre o Vulnerability Advisor 
{: #about}

O Vulnerability Advisor fornece gerenciamento de segurança para o {{site.data.keyword.containerlong}}. O Vulnerability Advisor gera um
relatório de status de segurança, sugere correções e melhores práticas, além de fornecer gerenciamento para restringir
a execução de imagens não seguras. A correção dos problemas de segurança e configuração que são relatados pelo Vulnerability Advisor pode ajudar a proteger a infraestrutura do {{site.data.keyword.cloud_notm}}.
{:shortdesc}

O Vulnerability Advisor inclui os recursos a seguir:

-   Varre imagens em busca de vulnerabilidades
-   Fornece um relatório de avaliação com base nos padrões de segurança, como ISO 27002,
[Center of Internet Security ![ícone do link externo](../../icons/launch-glyph.svg "ícone dolink externo")](https://www.cisecurity.org/)e práticas de segurança específicas para o

{{site.data.keyword.containerlong_notm}}
-   Detecta malware baseado em arquivo
-   Fornece recomendações para proteger arquivos de configuração para um subconjunto de tipos de aplicativos
-   Fornece instruções sobre como corrigir um problema de vulnerabilidade ou de configuração relatado em seus
relatórios
   

    


**Pacotes vulneráveis**

O Vulnerability Advisor verifica pacotes vulneráveis em imagens que são baseadas em sistemas operacionais suportados e fornece um link para quaisquer avisos de segurança relevantes sobre a vulnerabilidade. 

Os pacotes com problemas de vulnerabilidade conhecidos são exibidos. As vulnerabilidades possíveis são atualizadas diariamente
de avisos de segurança publicados para os tipos de imagem do Docker que são listados na tabela a seguir. Geralmente, para que um pacote vulnerável passe pela varredura, é necessário uma
versão mais recente do pacote que inclua uma correção para a vulnerabilidade. O mesmo pacote pode listar múltiplas vulnerabilidades e,
nesse caso, um upgrade de pacote único pode tratar de múltiplas vulnerabilidades. As informações na coluna **AÇÃO
CORRETIVA** descrevem como melhorar a segurança.

Imagens base suportadas são descritas na tabela a seguir.

  |Imagem base Docker|Origem dos avisos de segurança|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://git.alpinelinux.org/) e [Procura CIRCL CVE ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cve.circl.lu/)|
  |CentOS| [Archives de anúncio CentOS ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://lists.centos.org/pipermail/centos-announce/) e [Archives de anúncio CentOS CR ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Anúncios de segurança Debian ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Errata do produto Red Hat ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Avisos de segurança do Ubuntu ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ubuntu.com/usn/)|
  {: caption="Tabela 1. Imagens base do Docker verificadas pelo Vulnerability Advisor em busca de pacotes vulneráveis" caption-side="top"}
  



**Configurações do aplicativo**

Lista configurações do aplicativo para a
imagem que não é segura. As informações na coluna **AÇÃO CORRETIVA** descrevem como
melhorar a segurança




**Relatório de segurança**

No painel Registro, a coluna **RELATÓRIO DE SEGURANÇA** exibe o status dos seus repositórios.

O Vulnerability Advisor verifica vulnerabilidades conhecidas nas definições de configuração para os tipos de aplicativos a seguir:
-   MySQL
-   NGINX
-   Apache

O relatório identifica boas práticas de segurança em nuvem para suas imagens. É possível acessar uma lista completa dos
problemas de segurança e configuração que são verificados pelo Vulnerability Advisor.

O painel do Vulnerability Advisor fornece uma visão geral e avaliação da segurança para uma imagem. 

Para descobrir mais sobre o painel do Vulnerability Advisor, consulte [Revisando um relatório de
vulnerabilidade](#va_reviewing).





## Revisando um relatório de vulnerabilidade para sua imagem
{: #va_reviewing}

Antes de implementar uma imagem, é possível revisar seu relatório do Vulnerability Advisor, que lhe dá detalhes sobre
quaisquer pacotes vulneráveis e configurações do aplicativo não seguras.
{:shortdesc}

As imagens de contêiner são fornecidas pela IBM, terceiros ou podem ser incluídas por sua organização.

Revise os problemas potenciais de segurança e configuração da imagem concluindo as etapas a seguir:

1.  Efetue login no {{site.data.keyword.Bluemix_notm}}. Deve-se ter efetuado login para ver o Vulnerability
Advisor na interface gráfica com o usuário.
2.  Clique em **Catálogo**.
3.  Sob **Infraestrutura**, clique em **Contêineres**. 
4.  Clique no azulejo **Registro do contêiner**.
5.  Expanda **Vulnerability Advisor** e clique em **Repositórios escaneados**. 
6.  Para ver o relatório para a imagem que é marcada como `mais recente`, clique na linha para esse repositório. 
O relatório mostra o número total de problemas e se eles são pacotes vulneráveis ou problemas de configuração. Se nenhuma marcação
`mais recente` existir no repositório, a imagem mais recente será usada.
7.  Para visualizar informações sobre cada pacote vulnerável, na tabela **Pacotes vulneráveis
localizados**, clique no link na coluna **VULNERABILIDADES** para abrir o relatório.
    1.  Para ver mais informações, expanda o resumo.
    2.  Para ver o aviso do distribuidor do sistema operacional, clique no link na coluna **NOTA OFICIAL**.
8.  Para visualizar informações sobre cada problema de configuração, na tabela **Problemas de configuração localizados**, clique na linha para o problema. 
9.  Execute a ação corretiva para cada problema mostrado no relatório e reconstrua a imagem. Alguns problemas no Dockerfile
podem ser resolvidos usando o código que é fornecido em [Resolvendo problemas nas imagens](#va_report).

Se existirem vulnerabilidades e você não as corrigir, esses problemas poderão impactar o uso da imagem para um contêiner. É
possível continuar a usar uma imagem que tenha problemas de segurança e configuração em um contêiner.

 




## Revisando a segurança de imagem para as imagens do Docker que estão armazenadas em um namespace usando a CLI 
{: #va_registry_cli}

É possível revisar a segurança das imagens do Docker que estão armazenadas em seu namespaces em
{{site.data.keyword.registrylong_notm}} usando a CLI para localizar informações sobre potenciais vulnerabilidades.
{:shortdesc}

Ao incluir uma imagem no registro, a imagem é automaticamente escaneada pelo Vulnerability Advisor para detectar problemas de
segurança e potenciais vulnerabilidades. Os contêineres que são implementados por meio de imagens vulneráveis podem ser atacados e comprometidos. As imagens serão varridas somente se forem baseadas em um sistema operacional suportado pelo Vulnerability Advisor.

Se problemas de segurança forem localizados, instruções serão fornecidas para ajudá-lo a corrigir a vulnerabilidade relatada.

Para verificar o status de vulnerabilidade das imagens em sua conta do {{site.data.keyword.Bluemix_notm}}, conclua as etapas a seguir.

1.  Liste todas as imagens em sua conta do {{site.data.keyword.Bluemix_notm}}. O comando a seguir retorna uma lista de todas as imagens,
independentemente do namespace no qual elas estão armazenadas.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Verifique o status na coluna **VULNERABILITY STATUS**. Um dos seguintes status é exibido:
    -   `OK` Esse status significa que nenhum problema de segurança foi localizado.
    -   `Vulnerável` Esse status significa que um potencial problema de segurança ou vulnerabilidade foi
localizado.
    -   `Desconhecido` Esse status é exibido enquanto a imagem está sendo escaneada até que o status de vulnerabilidade final possa
ser determinado.
    -   `S.O. não suportado` Esse status é exibido se a imagem não é suportada para ser escaneada pelo
Vulnerability Advisor.
4.  Para descobrir mais sobre o status, revise o relatório do Vulnerability Advisor.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    Em sua saída da CLI, é possível localizar a lista de pacotes vulneráveis, uma descrição da vulnerabilidade que foi localizada e
um link para instruções sobre como resolver isso.


## Resolvendo problemas comuns em imagens 
{: #va_report}

Correções de exemplo para problemas comuns que são relatados pelo Vulnerability Advisor.
{:shortdesc}

O Vulnerability Advisor fornece ações corretivas com problemas relatados de segurança ou de configuração. Alguns dos problemas relatados podem ser corrigidos atualizando o seu Dockerfile. Para ajudá-lo a resolver problemas
comuns mais rapidamente, use os exemplos de código a seguir para implementar a solução em seu
Dockerfile.

### Idade máxima da senha, dias mínimos da senha e comprimento mínimo da senha
{: #va_password}

**Problema**: você recebe um ou todos os erros a seguir:

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

### Vulnerabilidade do SSH 
{: #ssh}

**Problema**: o erro a seguir é retornado:

```
SSH server should not be installed.
```
{: screen}

**Correção**: em vez de usar SSH, use `docker attach` ou `docker exec`
para acessar seu contêiner. Assegure-se de que o Dockerfile não contenha nenhuma etapa para instalar um servidor SSH.

