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
-   ISO 27002, [인터넷 보안의 중심 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.cisecurity.org/) 및 {{site.data.keyword.containerlong_notm}}에 특정한 보안 사례와 같은 보안 표준을 기반으로 하는 평가 보고서 제공
-   파일 기반 악성코드 발견
-   애플리케이션 유형의 서브세트에 대한 구성 파일을 보호하기 위한 권장사항 제공
-   보고서에서 보고된 취약성 또는 구성 문제를 수정하는 방법에 관한 지시사항 제공
   

    


**취약한 패키지**

취약성 어드바이저에서는 지원되는 운영 체제를 기반으로 하는 이미지에서 취약한 패키지를 확인하고 취약성에 관한 보안 알림과 연결된 링크를 제공합니다. 

알려진 취약성 문제가 포함된 패키지가 표시됩니다. 가능한 취약성은 다음 표에 나열된 Docker 이미지 유형에 대해 공개된 보안 공지사항을 통해 매일 업데이트됩니다. 일반적으로 취약한 패키지의 스캔을 통과하려면 취약성에 대한 수정사항을 포함한 이후 버전의 패키지가 필요합니다. 동일한 패키지에는 다중 취약성이 나열될 수 있습니다. 이 경우 단일 패키지 업그레이드 시 다중 취약성이 해결될 수 있습니다. **정정 조치** 열의 정보에는 보안 향상 방법에 대한 설명이 있습니다. 

지원되는 기본 이미지는 다음 표에서 설명합니다.

  |Docker 기본 이미지|보안 알림 소스|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://git.alpinelinux.org/) 및 [CIRCL CVE Search ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.circl.lu/)|
  |CentOS| [CentOS 발표 아카이브 ![외부 링크 아이콘n](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.centos.org/pipermail/centos-announce/) 및 [CentOS CR 발표 아카이브 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian 보안 공지사항 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux(RHEL)|[Red Hat Product Errata ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu 보안 공지사항 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ubuntu.com/usn/)|
  {: caption="표 1. 취약성 어드바이저에서 취약한 패키지를 검사하는 Docker 기본 이미지" caption-side="top"}
  



**애플리케이션 구성**

비보안 상태인 이미지의 애플리케이션 설정을 나열합니다. **정정 조치** 열의 정보에는 보안 향상 방법에 대한 설명이 있습니다. 




**보안 보고서**

레지스트리 대시보드의 **보안 보고서** 열에는 저장소의 상태가 표시됩니다. 

취약성 어드바이저는 다음 애플리케이션 유형의 구성 설정에서 알려진 취약성을 확인합니다. 
-   MySQL
-   NGINX
-   Apache

보고서는 사용자의 이미지에 대한 양호한 클라우드 보안 사례를 식별합니다. 취약성 어드바이저로 확인되는 보안 및 구성 문제의 전체 목록에 액세스할 수 있습니다. 

취약성 어드바이저 대시보드는 이미지에 대한 보안의 개요 및 평가를 제공합니다.  

취약성 어드바이저 대시보드에 대한 자세한 정보는 [취약성 보고서 검토](#va_reviewing)를 참조하십시오. 





## 이미지에 대한 취약성 보고서 검토
{: #va_reviewing}

이미지를 배치하기 전에 취약한 패키지 및 비보안 애플리케이션 설정에 대한 세부사항을 제공하는 취약성 어드바이저 보고서를 검토할 수 있습니다.
{:shortdesc}

IBM, 써드파티에서 컨테이너 이미지를 제공하거나 사용자의 조직이 컨테이너 이미지를 추가할 수 있습니다. 

다음 단계를 완료하여 잠재적 이미지 보안 및 구성 문제를 검토하십시오. 

1.  {{site.data.keyword.Bluemix_notm}}에 로그인하십시오. 그래픽 사용자 인터페이스에서 취약성 어드바이저를 보려면 로그인해야 합니다. 
2.  **카탈로그**를 클릭하십시오.
3.  **인프라**에서 **컨테이너**를 클릭하십시오. 
4.  **컨테이너 레지스트리** 타일을 클릭하십시오. 
5.  **취약성 어드바이저**를 펼치고 **스캔된 저장소**를 클릭하십시오. 
6.  `최신`으로 태그가 지정된 이미지에 대한 보고서를 보려면 저장소의 행을 클릭하십시오. 보고서는 총 문제 수를 표시하고 취약한 패키지 또는 구성 문제가 있는지 여부를 표시합니다. 저장소에 `최신` 태그가 존재하지 않으면 최신 이미지가 사용됩니다. 
7.  각 취약한 패키지에 대한 정보를 보려면 **발견된 취약한 패키지** 표에서 **취약성** 열의 링크를 클릭하여 보고서를 여십시오. 
    1.  자세한 정보를 보려면 요약을 펼치십시오. 
    2.  운영 체제 배포자의 공지사항을 보려면 **공식 공지사항** 열의 링크를 클릭하십시오. 
8.  각 구성 문제에 대한 정보를 보려면 **발견된 구성 문제** 표에서 문제에 대한 행을 클릭하십시오.  
9.  보고서에 표시된 각 문제에 대한 정정 조치를 수행하고 이미지를 다시 빌드하십시오. Dockerfile의 일부 문제는 [이미지의 문제점 해결](#va_report)에 제공된 코드를 사용하여 해결할 수 있습니다. 

취약성이 존재하지만 수정하지 않은 경우 이 문제는 컨테이너에 대한 이미지의 사용에 영향을 줄 수 있습니다. 컨테이너에 보안 및 구성 문제가 있는 이미지를 계속 사용할 수 있습니다. 

 




## CLI를 사용하여 네임스페이스에 저장되는 Docker 이미지의 이미지 보안 검토 
{: #va_registry_cli}

잠재적 취약성에 대한 정보를 찾기 위해 CLI를 사용하여 {{site.data.keyword.registrylong_notm}}의 네임스페이스에 저장된 Docker 이미지의 보안을 검토할 수 있습니다.
{:shortdesc}

레지스트리에 이미지를 추가하면 취약성 어드바이저에서 보안 문제와 잠재적 취약성을 발견하기 위해 자동으로 이미지를 스캔합니다. 취약한 이미지에서 배치되는 컨테이너는 공격의 대상이 되고 손상될 수 있습니다. 이미지는 취약성 어드바이저에서 지원하는 운영 체제를 기반으로 하는 경우에만 스캔됩니다.

보안 문제가 발견되면 보고된 취약성을 수정하는 데 유용한 지시사항이 제공됩니다.

{{site.data.keyword.Bluemix_notm}} 계정에서 이미지의 취약성 상태를 확인하려면 다음 단계를 완료하십시오.

1.  {{site.data.keyword.Bluemix_notm}} 계정의 모든 이미지를 나열하십시오. 다음 명령을 실행하면 저장된 네임스페이스와 상관없이 모든 이미지의 목록을 리턴합니다.

    ```
    bx cr image-list
    ```
    {: pre}

2.  **VULNERABILITY STATUS** 열의 상태를 확인하십시오. 다음 상태 중 하나가 표시됩니다.
    -   `확인` 이 상태는 보안 문제가 없음을 나타냅니다.
    -   `취약` 이 상태는 잠재적 보안 문제 또는 취약성이 있음을 나타냅니다.
    -   `알 수 없음` 이미지를 스캔하는 동안 최종 취약성 상태를 판별할 수 있을 때까지 이 상태가 표시됩니다.
    -   `지원되지 않는 OS` 이 상태는 취약성 어드바이저에서 이미지를 스캔하도록 지원되지 않는 경우 표시됩니다.
4.  상태에 대한 자세한 정보는 취약성 어드바이저 보고서를 검토하십시오.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    CLI 출력에는 취약성 패키지 목록, 발견된 취약성에 대한 설명 및 수정 방법에 대한 지시사항이 연결된 링크가 있습니다.


## 이미지의 공통 문제점 해결 
{: #va_report}

취약성 어드바이저에서 보고한 공통 문제점의 수정사항 예제.
{:shortdesc}

취약성 어드바이저에서는 보고된 보안 또는 구성 문제와 함께 정정 조치를 제공합니다. 보고된 일부 문제는 Dockerfile을 업데이트하여 수정할 수 있습니다. 공통 문제점을 더 신속하게 해결하려면 다음 코드 예제를 사용하여 Dockerfile에서 솔루션을 구현하십시오.

### 최대 비밀번호 사용 기간, 최소 비밀번호 사용 기간(일) 및 최소 비밀번호 길이
{: #va_password}

**문제점**: 다음 오류 중 하나 또는 모두가 표시됩니다.

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

**수정사항**: Dockerfile에 다음 코드를 추가하여 비밀번호 규제 준수를 설정하십시오.

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### SSH 취약성 
{: #ssh}

**문제점**: 다음 오류가 리턴됩니다.

```
SSH server should not be installed.
```
{: screen}

**수정사항**: SSH를 사용하지 않고 `docker attach` 또는 `docker exec`를 사용하여 컨테이너에 액세스하십시오. Dockerfile에 SSH Server를 설치하는 단계가 포함되어 있지 않도록 하십시오.

