---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-05"

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


# Vulnerability Advisor로 이미지 보안 관리
{: #va_index}

Vulnerability Advisor는 {{site.data.keyword.IBM}} 또는 서드파티에서 제공하거나 조직의 레지스트리 네임스페이스에 추가된 컨테이너 이미지의 보안 상태를 검사합니다.
{:shortdesc}

네임스페이스에 이미지를 추가하면 Vulnerability Advisor에서 보안 문제와 잠재적 취약성을 발견하기 위해 자동으로 이미지를 스캔합니다. 보안 문제가 발견되면 보고된 취약성을 수정하는 데 유용한 지시사항이 제공됩니다.

Vulnerability Advisor는 제안되는 수정사항 및 우수 사례가 포함된 심각도 상태 보고서를 생성하여 [{{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started)를 위한 보안 관리를 제공합니다.

Vulnerability Advisor에서 발견된 문제는 이 이미지의 배치는 권장되지 않음을 표시하는 결과를 나타냅니다. 이미지를 배치하도록 선택한 경우 이미지에서 배치된 컨테이너는 컨테이너를 공격하거나 손상시키는 데 사용될 수 있는 문제를 알고 있습니다. 결과는 지정한 면제 정책을 기반으로 조정됩니다. 이 결과는 {{site.data.keyword.containerlong_notm}}에서 비보안 이미지의 배치를 방지하기 위해 컨테이너 이미지 보안 적용으로 사용될 수 있습니다.

Vulnerability Advisor에서 보고한 보안 및 구성 문제를 수정하면 {{site.data.keyword.cloud_notm}} 인프라를 보호할 수 있습니다.

## Vulnerability Advisor 정보
{: #about}

Vulnerability Advisor는 이미지를 보호할 수 있는 기능을 제공합니다.
{:shortdesc}

사용할 수 있는 기능은 다음과 같습니다.

- 문제를 확인하기 위한 이미지 스캔
- {{site.data.keyword.containerlong_notm}}에 대한 보안 사례를 기반으로 한 평가 보고서 제공
- 애플리케이션 유형의 서브세트에 대한 구성 파일을 보호하기 위한 권장사항 제공
- 보고된 [취약한 패키지](#packages) 또는 이 보고서의 [구성 문제](#app_configurations)를 수정하는 방법에 대한 지시사항 제공
- 결과를 [컨테이너 이미지 보안 적용](/docs/services/Registry?topic=registry-security_enforce#security_enforce)에 제공
- 플래그 지정된 문제가 유스 케이스에 적용되지 않을 때 표시할 계정, 네임스페이스, 저장소 또는 태그 레벨에서 면제를 보고서에 적용

레지스트리 대시보드의 **정책 상태** 열에는 저장소의 상태가 표시됩니다. 연결된 보고서는 사용자의 이미지에 대해 양호한 클라우드 보안 사례를 식별합니다.

취약성 어드바이저 대시보드는 이미지에 대한 보안의 개요 및 평가를 제공합니다. Vulnerability Advisor 대시보드에 대한 자세한 정보는 [취약성 보고서 검토](#va_reviewing)를 참조하십시오.

### 데이터 보호
{: #about_data_protection}

보안 문제에 대한 계정에서 이미지 및 컨테이너를 스캔하면 Vulnerability Advisor가 다음 정보를 수집하고 저장하고 처리합니다.

- ID, 설명 및 이미지 이름을 포함한 자유 양식 필드(레지스트리, 네임스페이스, 저장소 이름 및 이미지 태그)
- 구성 파일의 작성 시간소인 및 파일 모드에 대한 메타데이터
- 이미지 및 컨테이너에 있는 시스템 및 애플리케이션 구성 파일의 컨텐츠
- 설치된 패키지 및 라이브러리(해당 버전 포함)

이전 목록에서 식별된 대로, Vulnerability Advisor가 처리하는 필드 또는 위치에 개인 정보를 입력하지 마십시오.

데이터 센터 레벨에서 수집된 스캔 결과가 익명의 메트릭을 생성하여 서비스를 운영하고 개선하도록 처리됩니다.

스캔 결과는 생성되고 30일 후에 삭제됩니다.

## 취약성 유형
{: #types}

### 취약한 패키지
{: #packages}

Vulnerability Advisor에서는 지원되는 운영 체제를 사용 중인 이미지에서 취약한 패키지를 확인하고 취약성에 관한 보안 공지사항과 연결된 링크를 제공합니다.
{:shortdesc}

알려진 취약성 문제가 있는 패키지가 스캔 결과에 표시됩니다. 가능한 취약성은 다음 표에 나열된 Docker 이미지 유형에 대해 공개된 보안 공지사항을 사용하여 매일 업데이트됩니다. 일반적으로 취약한 패키지의 스캔을 통과하려면 취약성에 대한 수정사항을 포함한 이후 버전의 패키지가 필요합니다. 동일한 패키지에 다중 취약성이 나열될 수 있습니다. 이 경우 단일 패키지 업데이트 시 다중 취약성이 해결될 수 있습니다.

  |Docker 기본 이미지|보안 공지사항 소스|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://git.alpinelinux.org/) 및 [CIRCL CVE Search ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.centos.org/pipermail/centos-announce/) 및 [CentOS CR announce archives ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.centos.org/pipermail/centos-cr-announce/) 취약성에 대한 자세한 정보는 [CentOS의 패키지 취약성](#va_centos)을 참조하십시오.|
  |Debian|[Debian security announcements ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux(RHEL)|[Red Hat Security Data API ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://access.redhat.com/labsinfo/securitydataapi)|
  |Ubuntu|[Ubuntu Security Notices ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://usn.ubuntu.com/)|
  {: caption="표 1. Vulnerability Advisor에서 취약한 패키지를 검사하는 지원되는 Docker 기본 이미지" caption-side="top"}

### 구성 문제
{: #app_configurations}

구성 문제는 앱 설정 방법과 관련된 잠재적 보안 문제입니다. 보고된 많은 문제점은 Dockerfile을 업데이트하여 수정할 수 있습니다.
{:shortdesc}

이미지는 Vulnerability Advisor에서 지원하는 운영 체제를 사용 중인 경우에만 스캔됩니다. Vulnerability Advisor는 다음 앱 유형에 대한 구성 설정을 검사합니다.

- MySQL
- NGINX
- Apache

## 취약성 보고서 검토
{: #va_reviewing}

이미지를 배치하기 전에 Vulnerability Advisor 보고서를 검토하여 취약한 패키지와 비보안 컨테이너 또는 앱 설정의 세부사항을 확인할 수 있습니다. 또한 이미지가 조직 정책을 준수하는지 여부를 확인할 수 있습니다.
{:shortdesc}

발견된 문제를 해결하지 않은 경우 이러한 문제는 이 이미지를 사용하여 빌드된 컨테이너의 보안에 영향을 줄 수 있습니다. 컨테이너 이미지 보안 적용이 배치되지 않은 경우 컨테이너에 보안 및 구성 문제가 있는 이미지를 계속 사용할 수 있습니다. 컨테이너 이미지 보안 적용이 배치되고 이미지용으로 활성인 경우 이 이미지에서 컨테이너를 배치할 수 있도록 정책에서 발견된 모든 문제를 면제해야 합니다.

컨테이너 이미지 보안 적용에서 Vulnerability Advisor 문제 적용 범위를 구성하려면 [정책 사용자 정의](/docs/services/Registry?topic=registry-security_enforce#customize_policies)를 참조하십시오.
{:tip}

이미지가 조직 정책에 따라 설정된 요구사항을 만족하지 않으면 해당 요구사항을 충족하도록 이미지를 구성해야 배치할 수 있습니다. 조직 정책을 보고 변경하는 방법에 대한 자세한 정보는 [조직 면제 정책 설정](#va_managing_policy)을 참조하십시오.
{:tip}

### GUI를 사용하여 취약성 보고서 검토
{: #va_reviewing_gui}

GUI를 사용하여 {{site.data.keyword.registrylong_notm}}의 네임스페이스에 저장된 Docker 이미지의 보안을 검토할 수 있습니다.
{:shortdesc}

1. {{site.data.keyword.cloud_notm}}에 로그인하십시오.
2. **탐색 메뉴** 아이콘을 클릭하고 **Kubernetes**를 클릭하십시오.
3. **레지스트리**를 클릭하고 **이미지** 타일을 클릭하십시오. 이미지의 목록과 각 이미지의 보안 상태가 **이미지** 테이블에 표시됩니다.
4. `latest`로 태그가 지정된 이미지에 대한 보고서를 보려면, 해당 이미지의 행을 클릭하십시오. **이미지 세부사항 정보** 탭이 열리고 해당 이미지에 대한 데이터가 표시됩니다. 저장소에 `최신` 태그가 존재하지 않으면 최신 이미지가 사용됩니다.
5. 보안 상태에 문제가 표시되는 경우 문제에 대해 확인하려면, **유형별 문제** 탭을 클릭하십시오. **취약성** 및 **구성 문제** 테이블이 열립니다.

   - **취약성** 테이블. 각 문제에 대한 취약성 ID, 해당 문제에 대한 정책 상태, 영향을 받는 패키지 및 문제 해결 방법이 표시됩니다. 해당 문제에 대한 자세한 정보를 보려면 행을 펼치십시오. 문제의 요약 정보가 표시되며 해당 문제와 관련된 벤더 보안 공지사항에 대한 링크가 포함되어 있습니다. 알려진 취약성 문제가 포함된 패키지를 나열합니다.
  
     목록은 [취약성 유형](#types)에 나열된 Docker 이미지 유형에 대해 공개된 보안 공지사항을 사용하여 매일 업데이트됩니다. 일반적으로 취약한 패키지의 스캔을 통과하려면 취약성에 대한 수정사항을 포함한 이후 버전의 패키지가 필요합니다. 동일한 패키지에 다중 취약성이 나열될 수 있습니다. 이 경우 단일 패키지 업데이트 시 여러 문제가 정정될 수 있습니다. 패키지에 대한 자세한 정보와 패키지를 업데이트하는 단계를 보려면 보안 공지사항 코드를 클릭하십시오.

   - **구성 문제** 테이블. 각 문제에 대한 구성 문제 ID, 해당 문제에 대한 정책 상태 및 보안 사례가 표시됩니다. 해당 문제에 대한 자세한 정보를 보려면 행을 펼치십시오. 문제의 요약 정보가 표시되며 해당 문제와 관련된 보안 공지사항에 대한 링크가 포함되어 있습니다.
  
     목록에는 비보안 컨테이너에 대한 애플리케이션 설정 및 컨테이너의 보안을 강화하기 위해 수행할 수 있는 조치에 대한 제안사항이 포함되어 있습니다. 문제를 해결하는 방법을 보려면 행을 펼치십시오.

6. 보고서에 표시된 각 문제에 대한 정정 조치를 완료하고 이미지를 다시 빌드하십시오.

### CLI를 사용하여 취약성 보고서 검토
{: #va_registry_cli}

CLI를 사용하여 {{site.data.keyword.registrylong_notm}}의 네임스페이스에 저장된 Docker 이미지의 보안을 검토할 수 있습니다.
{:shortdesc}

1. {{site.data.keyword.cloud_notm}} 계정에 이미지를 나열하십시오. 저장된 네임스페이스와 관계없이 모든 이미지의 목록이 리턴됩니다.

   ```
    ibmcloud cr image-list
   ```
   {: pre}

2. **SECURITY STATUS** 열에서 상태를 확인하십시오.
    - `문제 없음` 보안 문제가 발견되지 않았습니다.
    - `<X>개의 문제` 발견된 잠재적 보안 문제 또는 취약성 수입니다. 여기서 `<X>`는 문제 수입니다.
    - `스캐닝` 이미지가 스캐닝되고 있으며 최종 취약성 상태는 판별되지 않았습니다.

3. 상태에 대한 세부사항을 보려면 Vulnerability Advisor 보고서를 검토하십시오.

   ```
    ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
   ```
   {: pre}

   CLI 출력에서 구성 문제에 대해 다음 정보를 볼 수 있습니다.
      - `보안 사례` 발견된 취약성에 대한 설명
      - `정정 조치` 취약성 수정 방법에 대한 세부사항

### CentOS의 패키지 취약성
{: #va_centos}

CentOS를 사용 중인 경우 보고서에 거짓 긍정(false positive)이 발생할 수 있습니다. 즉, 취약성이 없는 경우 보고서에 취약성이 보고될 수 있습니다. 이 상황은 Red Hat에서 보안 공지사항을 릴리스했지만 보안 공지사항이 CentOS에 적용될 수 없거나 수정사항이 아직 CentOS에 포팅되지 않은 경우에 발생합니다.
{:shortdesc}

패키지에 취약성이 있음을 나타내는 보고서를 수신하는 경우 다음 단계를 완료하십시오.

1. 보안 공지사항 코드를 클릭하여 패키지를 업데이트하는 단계를 보십시오.
2. 패키지를 업데이트하는 단계를 완료하여 패키지를 업데이트하십시오.
3. 패키지가 업데이트되면 결과가 거짓 긍정(false positive)이 아니며 필요한 조치가 완료됩니다.
4. 설치할 수 없는 새 버전이 없으므로 패키지가 업데이트되지 않은 경우 결과가 거짓 긍정(false positive)입니다. 이 보안 공지사항에 대한 면제 정책을 설정할 수 있습니다. [조직 면제 정책 설정](#va_managing_policy)을 참조하십시오.

## 조직 면제 정책 설정
{: #va_managing_policy}

{{site.data.keyword.cloud_notm}} 조직의 보안을 관리하려면 정책 설정을 사용하여 문제 면제 여부를 판별할 수 있습니다. 사용자 정책에 따라 면제된 문제를 처리한 후 보안 문제가 없는 이미지에서만 배치가 허용되도록 컨테이너 이미지 보안 적용을 사용할 수 있습니다.
{:shortdesc}

클러스터에 컨테이너 이미지 보안 적용이 배치되지 않은 경우 보안 상태와 상관없이 이미지에서 컨테이너를 배치할 수 있습니다. 컨테이너 이미지 보안 적용을 배치하는 방법을 알아보려면 [보안 적용 설치](/docs/services/Registry?topic=registry-security_enforce#security_enforce)를 참조하십시오.

컨테이너 이미지 보안 적용을 사용하는 경우 Vulnerability Advisor에서 발견한 보안 문제 때문에 이미지에서 컨테이너가 배치되지 않습니다. 발견된 문제가 있는 이미지를 배치할 수 있으려면 정책에 면제가 추가되어야 합니다.

### GUI를 사용하여 조직 면제 정책 설정
{: #va_managing_policy_gui}

GUI를 사용하여 정책에 면제를 설정하려면 다음 단계를 완료하십시오.

1. {{site.data.keyword.cloud_notm}}에 로그인하십시오. GUI에서 Vulnerability Advisor에 로그인해야 합니다.
2. **탐색 메뉴** 아이콘을 클릭하고 **Kubernetes**를 클릭하십시오.
3. **Vulnerability Advisor**에서 **정책 설정**을 클릭하십시오.
4. **면제 작성**을 클릭하십시오.
5. 문제 유형을 선택하십시오.
6. 문제 ID를 입력하십시오.

   [취약성 보고서](#va_reviewing)에서 이 정보를 찾을 수 있습니다. **취약성 ID** 열에는 CVE 또는 보안 공지사항 문제에 사용할 ID가 포함되어 있습니다. **구성 문제 ID** 열에는 구성 문제에 사용할 ID가 포함되어 있습니다.
   {: tip}

7. 면제를 적용할 레지스트리 네임스페이스, 저장소 및 태그를 선택하십시오.
8. **저장**을 클릭하십시오.

관련 행 위에 마우스를 두고 **옵션 목록 열기 및 닫기** 아이콘을 클릭하여 면제를 편집하고 제거할 수도 있습니다.

### CLI를 사용하여 조직 면제 정책 설정
{: #va_managing_policy_cli}

CLI를 사용하여 정책에 면제를 설정하려면 다음 명령을 실행할 수 있습니다.

- 보안 문제 면제를 작성하려면 [`ibmcloud cr exemption-add`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_add) 명령을 실행하십시오.
- 보안 문제 면제를 나열하려면 [`ibmcloud cr exemption-list`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_list) 명령을 실행하십시오.
- 면제할 수 있는 보안 문제 유형을 나열하려면 [`ibmcloud cr exemption-types`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_types) 명령을 실행하십시오.
- 보안 문제 면제를 삭제하려면 [`ibmcloud cr exemption-rm`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_rm) 명령을 실행하십시오.

명령에 대한 자세한 정보는 명령을 실행할 때 `--help` 플래그를 사용할 수 있습니다.
