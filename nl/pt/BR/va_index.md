---

copyright:
  years: 2017
lastupdated: "2017-10-26"

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
-   Fornece um relatório de
avaliação com base nos padrões de segurança, como ISO 27002, bem como práticas de segurança específicas para o
{{site.data.keyword.containerlong_notm}}
-   Detecta malware baseado em arquivo
-   Fornece recomendações para proteger arquivos de configuração para um subconjunto de tipos de aplicativos
-   Fornece instruções sobre como corrigir um problema de vulnerabilidade ou de configuração relatado em seus
relatórios

<dl>
  <dt><strong>Pacotes vulneráveis</strong></dt>
  <dd>O Vulnerability Advisor verifica pacotes vulneráveis em imagens que são baseadas em sistemas operacionais suportados e fornece um link para quaisquer avisos de segurança relevantes sobre a vulnerabilidade. O Vulnerability Advisor atualiza sua lista interna com relação a esses avisos de segurança diariamente. Imagens base suportadas são descritas na tabela a seguir.</dd>
</dl>

  |Imagem base Docker|Origem dos avisos de segurança|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://git.alpinelinux.org/) e [Procura CIRCL CVE ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cve.circl.lu/)|
  |CentOS| [Archives de anúncio CentOS ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://lists.centos.org/pipermail/centos-announce/) e [Archives de anúncio CentOS CR ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Anúncios de segurança Debian ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Errata do produto Red Hat ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Avisos de segurança do Ubuntu ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ubuntu.com/usn/)|
  {: caption="Tabela 1. Imagens base do Docker verificadas pelo Vulnerability Advisor em busca de pacotes vulneráveis" caption-side="top"}










## Revisando a segurança de imagens do Docker que estão armazenadas em um namespace no {{site.data.keyword.registrylong_notm}} usando a CLI 
{: #va_registry_cli}

É possível revisar a segurança de imagens do Docker que estão armazenadas em um namespace para localizar informações sobre
possíveis vulnerabilidades.
{:shortdesc}

Ao incluir uma imagem no {{site.data.keyword.registrylong_notm}}, ela é varrida automaticamente pelo Vulnerability Advisor para detectar problemas de segurança e possíveis vulnerabilidades. Os contêineres que são implementados por meio de imagens vulneráveis podem ser atacados e comprometidos. As imagens serão varridas somente se forem baseadas em um sistema operacional suportado pelo Vulnerability Advisor.

O Vulnerability Advisor verifica as vulnerabilidades a seguir. Se problemas de segurança forem localizados, instruções serão fornecidas para ajudá-lo a corrigir a vulnerabilidade relatada.

Para verificar o status de vulnerabilidade das imagens em sua conta do {{site.data.keyword.Bluemix_notm}}, conclua as etapas a seguir.

1.  Liste todas as imagens em sua conta do {{site.data.keyword.Bluemix_notm}}. O comando a seguir retorna uma lista de todas as imagens,
independentemente do namespace no qual elas estão armazenadas.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Verifique o status na coluna VULNERABILITY STATUS. Um dos seguintes status é exibido:
    -   OK. Esse status significa que nenhum problema de segurança foi encontrado.
    -   Vulnerável. Esse status significa que um problema de segurança ou uma vulnerabilidade em potencial foi
localizada.
    -   Unknown. Esse status é exibido enquanto a imagem está sendo varrida até que o status de
vulnerabilidade final possa ser determinado.
    -   Sistema operacional não suportado. Esse status é exibido se a imagem não é suportada para ser varrida pelo
Vulnerability Advisor.
4.  Para descobrir mais sobre o status, revise o relatório do Vulnerability Advisor.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    Em sua saída da CLI, é possível localizar a lista de pacotes vulneráveis, uma descrição da vulnerabilidade que foi localizada e
um link para instruções sobre como resolver isso.


## Resolvendo problemas nas imagens 
{: #va_report}

Correções de exemplo para problemas comuns que são relatados pelo Vulnerability Advisor.
{:shortdesc}

O Vulnerability Advisor fornece ações corretivas com problemas relatados de segurança ou de configuração. Alguns dos problemas relatados podem ser corrigidos atualizando o seu Dockerfile. Para ajudá-lo a resolver problemas
comuns mais rapidamente, use os exemplos de código a seguir para implementar a solução em seu
Dockerfile.

### Idade máxima da senha, dias mínimos da senha e comprimento mínimo da senha
{: #va_password}

Problema: você recebe um ou todos os erros a seguir:

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

Correção: configure a conformidade de senha incluindo o código a seguir no
Dockerfile.

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### Vulnerabilidade do SSH 
{: #ssh}

Problema: o erro a seguir é retornado:

```
SSH server should not be installed.
```
{: screen}

Correção: em vez de usar SSH, use `docker attach` ou `docker exec` para
acessar o seu contêiner. Assegure-se de que o Dockerfile não contenha nenhuma etapa para instalar um servidor SSH.

