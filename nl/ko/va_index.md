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

# 취약성 어드바이저로 이미지 보안 관리 
{: #va_index}

취약성 어드바이저에서는 배치하기 전에 컨테이너 이미지의 보안 상태를 확인합니다.
{:shortdesc}


## 취약성 어드바이저 정보 
{: #about}

취약성 어드바이저에서는 {{site.data.keyword.containerlong}}의 보안 관리를 제공합니다. 취약성 어드바이저에서는 보안 상태 보고서를 생성하고 수정사항과 우수 사례를 제안하며 안전하지 않은 이미지가 실행되지 않게 제한하는 관리 기능을 제공합니다. 취약성 어드바이저에서 보고한 보안 및 구성 문제를 수정하면 {{site.data.keyword.cloud_notm}} 인프라를 보호할 수 있습니다.
{:shortdesc}

취약성 어드바이저에는 다음 기능이 포함되어 있습니다.

-   취약성을 확인하기 위해 이미지 스캔
-   ISO 27002 등의 보안 표준을 기반으로 하는 평가 보고서 외에도 {{site.data.keyword.containerlong_notm}}에 고유한 보안 사례 제공
-   파일 기반 악성코드 발견
-   애플리케이션 유형의 서브세트에 대한 구성 파일을 보호하기 위한 권장사항 제공
-   보고서에서 보고된 취약성 또는 구성 문제를 수정하는 방법에 관한 지시사항 제공

<dl>
  <dt><strong>취약한 패키지</strong></dt>
  <dd>취약성 어드바이저에서는 지원되는 운영 체제를 기반으로 하는 이미지에서 취약한 패키지를 확인하고 취약성에 관한 보안 알림과 연결된 링크를 제공합니다. 취약성 어드바이저에서는 매일 이 보안 알림과 비교하여 내부 목록을 업데이트합니다. 지원되는 기본 이미지는 다음 표에서 설명합니다.</dd>
</dl>

  |Docker 기본 이미지|보안 알림 소스|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://git.alpinelinux.org/) 및 [CIRCL CVE Search ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.circl.lu/)|
  |CentOS| [CentOS 발표 아카이브 ![외부 링크 아이콘n](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.centos.org/pipermail/centos-announce/) 및 [CentOS CR 발표 아카이브 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian 보안 공지사항 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux(RHEL)|[Red Hat Product Errata ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu 보안 공지사항 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ubuntu.com/usn/)|
  {: caption="표 1. 취약성 어드바이저에서 취약한 패키지를 검사하는 Docker 기본 이미지" caption-side="top"}










## CLI를 사용하여 {{site.data.keyword.registrylong_notm}}의 네임스페이스에 저장되는 Docker 이미지의 이미지 보안 검토 
{: #va_registry_cli}

잠재적 취약점에 대한 정보를 찾기 위해 네임스페이스에 저장된 Docker 이미지의 보안을 검토할 수 있습니다.
{:shortdesc}

{{site.data.keyword.registrylong_notm}}에 이미지를 추가하면 취약성 어드바이저에서 보안 문제와 잠재적 취약성을 발견하기 위해 자동으로 이미지를 스캔합니다. 취약한 이미지에서 배치되는 컨테이너는 공격의 대상이 되고 손상될 수 있습니다. 이미지는 취약성 어드바이저에서 지원하는 운영 체제를 기반으로 하는 경우에만 스캔됩니다.

취약성 어드바이저에서 다음과 같은 취약성을 확인합니다. 보안 문제가 발견되면 보고된 취약성을 수정하는 데 유용한 지시사항이 제공됩니다.

{{site.data.keyword.Bluemix_notm}} 계정에서 이미지의 취약성 상태를 확인하려면 다음 단계를 완료하십시오.

1.  {{site.data.keyword.Bluemix_notm}} 계정의 모든 이미지를 나열하십시오. 다음 명령을 실행하면 저장된 네임스페이스와 상관없이 모든 이미지의 목록을 리턴합니다.

    ```
    bx cr image-list
    ```
    {: pre}

2.  VULNERABILITY STATUS 열의 상태를 확인하십시오. 다음 상태 중 하나가 표시됩니다.
    -   정상. 이 상태는 보안 문제가 없음을 나타냅니다.
    -   취약. 이 상태는 잠재적 보안 문제 또는 취약성이 있음을 나타냅니다.
    -   알 수 없음. 이미지를 스캔하는 동안 최종 취약성 상태를 판별할 수 있을 때까지 이 상태가 표시됩니다.
    -   지원되지 않는 OS. 이 상태는 취약성 어드바이저에서 이미지를 스캔하도록 지원되지 않는 경우 표시됩니다.
4.  상태에 대한 자세한 정보는 취약성 어드바이저 보고서를 검토하십시오.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    CLI 출력에는 취약성 패키지 목록, 발견된 취약성에 대한 설명 및 수정 방법에 대한 지시사항이 연결된 링크가 있습니다.


## 이미지의 문제점 해결 
{: #va_report}

취약성 어드바이저에서 보고한 공통 문제점의 수정사항 예제.
{:shortdesc}

취약성 어드바이저에서는 보고된 보안 또는 구성 문제와 함께 정정 조치를 제공합니다. 보고된 일부 문제는 Dockerfile을 업데이트하여 수정할 수 있습니다. 공통 문제점을 더 신속하게 해결하려면 다음 코드 예제를 사용하여 Dockerfile에서 솔루션을 구현하십시오.

### 최대 비밀번호 사용 기간, 최소 비밀번호 사용 기간(일) 및 최소 비밀번호 길이
{: #va_password}

문제점: 다음 오류 중 하나 또는 모두가 표시됩니다.

```
Maximum password age must be set to 90 days.
```
{: screen}

```
Minimum password length must be 8.
```
{: screen}

```
Minimum days that must elapse between user-initiated password changes should be 1.
```
{: screen}

수정사항: Dockerfile에 다음 코드를 추가하여 비밀번호 규제 준수를 설정하십시오.

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### SSH 취약성 
{: #ssh}

문제점: 다음 오류가 리턴됩니다.

```
SSH server should not be installed.
```
{: screen}

수정사항: SSH를 사용하지 않고 `docker attach` 또는 `docker exec`를 사용하여 컨테이너에 액세스하십시오. Dockerfile에 SSH Server를 설치하는 단계가 포함되어 있지 않도록 하십시오.

