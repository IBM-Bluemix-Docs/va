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


# 취약성 어드바이저로 이미지 보안 관리
{: #va_index}

취약성 어드바이저는 IBM 또는 써드파티에서 제공하거나 조직의 레지스트리 네임스페이스에 추가된 컨테이너 이미지의 보안 상태를 검사합니다.
{:shortdesc}


네임스페이스에 이미지를 추가하면 취약성 어드바이저에서 보안 문제와 잠재적 취약성을 발견하기 위해 자동으로 이미지를 스캔합니다. 보안 문제가 발견되면 보고된 취약성을 수정하는 데 유용한 지시사항이 제공됩니다. 여전히 취약한 이미지에서 컨테이너를 배치할 수 있지만 이 컨테이너가 공격받거나 손상될 수 있습니다.


## 취약성 어드바이저 정보
{: #about}

취약성 어드바이저에서는 {{site.data.keyword.containerlong}}의 보안 관리를 제공합니다. 취약성 어드바이저에서는 보안 상태 보고서를 생성하고 수정사항과 우수 사례를 제안하며 안전하지 않은 이미지가 실행되지 않게 제한하는 관리 기능을 제공합니다. 취약성 어드바이저에서 보고한 보안 및 구성 문제를 수정하면 {{site.data.keyword.cloud_notm}} 인프라를 보호할 수 있습니다.
{:shortdesc}

취약성 어드바이저에는 다음 기능이 포함되어 있습니다.

-   취약성을 확인하기 위해 이미지 스캔
-   {{site.data.keyword.containerlong_notm}}에 대한 보안 사례를 기반으로 한 평가 보고서 제공
-   애플리케이션 유형의 서브세트에 대한 구성 파일을 보호하기 위한 권장사항 제공
-   보고된 [취약한 패키지](#packages) 또는 이 보고서의 [구성 문제](#app_configurations)를 수정하는 방법에 대한 지시사항 제공

레지스트리 대시보드의 **보안 보고서** 열에는 저장소의 상태가 표시됩니다. 보고서는 사용자의 이미지에 대한 양호한 클라우드 보안 사례를 식별합니다. 

취약성 어드바이저 대시보드는 이미지에 대한 보안의 개요 및 평가를 제공합니다. 취약성 어드바이저 대시보드에 대한 자세한 정보는 [취약성 보고서 검토](#va_reviewing)를 참조하십시오.
	
	
**데이터 보호**

보안 문제에 대한 계정에서 이미지 및 컨테이너를 스캔하려면 취약성 어드바이저가 다음 정보를 수집하고 저장하고 처리합니다.
- ID, 설명 및 이미지 이름을 포함한 자유 양식 텍스트 필드(레지스트리, 네임스페이스, 저장소 이름 및 이미지 태그)
- 팟(Pod), 복제 세트 및 배치 이름과 같은 Kubernetes 리소스의 이름이 포함된 Kubernetes 메타데이터
- 구성 파일의 작성 시간소인 및 파일 모드에 대한 메타데이터
- 이미지 및 컨테이너에 있는 시스템 및 애플리케이션 구성 파일의 컨텐츠
- 설치된 패키지 및 라이브러리(해당 버전 포함)

이전 목록에서 식별된 대로, 취약성 어드바이저가 처리하는 필드 또는 위치에 개인 정보를 입력하지 마십시오.

데이터 센터 레벨에서 수집된 스캔 결과가 익명의 메트릭을 생성하여 서비스를 운영하고 개선하도록 처리됩니다.

스캔 결과는 생성되고 30일 후에 삭제됩니다.



## 취약성 유형
{: #types}

### 취약한 패키지
{: #packages}

취약성 어드바이저에서는 지원되는 운영 체제를 기반으로 하는 이미지에서 취약한 패키지를 확인하고 취약성에 관한 보안 알림과 연결된 링크를 제공합니다.
{:shortdesc}

알려진 취약성 문제가 있는 패키지가 스캔 결과에 표시됩니다. 가능한 취약성은 다음 표에 나열된 Docker 이미지 유형에 대해 공개된 보안 공지사항을 통해 매일 업데이트됩니다. 일반적으로 취약한 패키지의 스캔을 통과하려면 취약성에 대한 수정사항을 포함한 이후 버전의 패키지가 필요합니다. 동일한 패키지에는 다중 취약성이 나열될 수 있습니다. 이 경우 단일 패키지 업그레이드 시 다중 취약성이 해결될 수 있습니다.


  |Docker 기본 이미지|보안 알림 소스|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://git.alpinelinux.org/) 및 [CIRCL CVE Search ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.circl.lu/)|
  |CentOS|[CentOS 발표 아카이브 ![외부 링크 아이콘n](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.centos.org/pipermail/centos-announce/) 및 [CentOS CR 발표 아카이브 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian 보안 공지사항 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux(RHEL)|[Red Hat Product Errata ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu 보안 공지사항 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://usn.ubuntu.com/)|
  {: caption="표 1. 취약성 어드바이저에서 취약한 패키지를 검사하는 지원되는 Docker 기본 이미지" caption-side="top"}




### 구성 문제
{: #app_configurations}

구성 문제는 앱 설정 방법과 관련된 잠재적 보안 문제입니다. 보고된 많은 문제점은 Dockerfile을 업데이트하여 수정할 수 있습니다.
{:shortdesc}

이미지는 취약성 어드바이저에서 지원하는 운영 체제를 기반으로 하는 경우에만 스캔됩니다. 취약성 어드바이저는 다음 앱 유형에 대한 구성 설정을 검사합니다.
-   MySQL
-   NGINX
-   Apache



## 컨테이너 스캐너 설치
{: #va_install_livescan}

시작하기 전에 다음을 수행하십시오.

1.  {{site.data.keyword.Bluemix_notm}} CLI 클라이언트에 로그인하십시오. 연합 계정이 있는 경우 `--sso`를 사용하십시오.
2.  Helm 차트를 사용할 대상 클러스터에 [`kubectl` CLI를 지정](../../containers/cs_cli_install.html#cs_cli_configure)하십시오.
3.  컨테이너 스캐너의 서비스 ID와 API 키를 작성하고 이름을 제공하십시오.
    1.  다음 명령을 실행하여 서비스 ID를 작성하십시오. `<scanner_serviceID>`는 서비스 ID에 대해 선택한 이름으로 바꾸십시오. 해당 **CRN**을 기록하십시오.
    
        ```
    	bx iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  서비스 API 키를 작성하십시오. 여기서 `<scanner_serviceID>`는 이전 단계에서 작성한 서비스 ID이며 `<scanner_APIkey_name>`을 스캐너 API 키에 대해 선택한 이름으로 바꾸십시오. 
    
        ```
    	bx iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	    스캐너 API 키가 리턴됩니다.
	
	    스캐너 API 키를 나중에 검색할 수 없으므로 이를 안전하게 저장하십시오.
	    {: tip}
	
    3.  `작성자` 역할을 부여하는 서비스 정책을 작성하십시오.
    		
        ```
    	bx iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

Helm 차트를 구성하려면 다음을 수행하십시오.

1.  [클러스터에서 Helm을 설정](../../containers/cs_integrations.html#helm)하십시오. RBAC 정책을 사용하여 Helm 틸러에 액세스 권한을 부여하는 경우 스캐너가 모든 네임스페이스에서 컨테이너를 감시할 수 있도록 틸러 역할에 모든 네임스페이스에 대한 액세스 권한이 있는지 확인하십시오.

2.  Helm에 IBM 차트 저장소를 추가하십시오(예: `ibm-incubator`).

    ```
    helm repo add ibm-incubator https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

3.  로컬 YAML 파일에 컨테이너 스캐너 Helm 차트에 대한 기본 구성 설정을 저장하십시오. Helm 차트 경로에 차트 저장소(예: `ibm-incubator`)를 포함하십시오.

    ```
    helm inspect values ibm-incubator/ibmcloud-container-scanner > config.yaml
    ```
    {: pre}

4.  `config.yaml` 파일을 편집하십시오.

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
    <caption>YAML 파일 컴포넌트 이해</caption>
    <thead>
    <th>필드</th>
    <th>값</th>
    </thead>
    <tbody>
    <tr>
    <td><code>EmitURL</code></td>
    <td>취약성 어드바이저 지역 엔드포인트 URL을 입력하십시오. URL을 확보하려면 <code>bx cr info</code>를 실행하고 <strong>컨테이너 레지스트리</strong> 주소를 검색하십시오. <code>registry</code>를 <code>va</code>로 바꾸십시오. 예: <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>클러스터가 있는 {{site.data.keyword.Bluemix_notm}} 계정 ID로 바꾸십시오. 계정 ID를 확보하려면 <code>bx account list</code>를 실행하십시오.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>컨테이너 스캐너를 설치할 Kubernetes 클러스터로 바꾸십시오. 클러스터 ID를 나열하려면 <code>bx cs clusters</code>를 실행하십시오.</td>
    </tr>
    <tr>
    <td><code>APIKey</code></td>
    <td>이전에 작성한 스캐너 API 키로 바꾸십시오.</td>
    </tr>
    </tbody></table>

5.  업데이트한 `config.yaml` 파일과 함께 Helm 차트를 클러스터에 설치하십시오. 업데이트한 특성은 차트의 구성 맵에 저장됩니다. `<myscanner>`를 Helm 차트에 대해 선택한 이름으로 바꾸십시오. Helm 차트 경로에 차트 저장소(예: `ibm-incubator`)를 포함하십시오.

    ```
    helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    **참고**: 컨테이너 스캐너가 `kube-system` 네임스페이스에 설치되지만 모든 네임스페이스에서 컨테이너를 스캔합니다.

6.  차트 배치 상태를 확인하십시오. 차트가 준비되면 출력의 상단에 있는 **STATUS** 필드는 `DEPLOYED` 값을 가집니다.

    ```
    helm status <myscanner>
    ```
    {: pre}

7.  차트가 배치되면 `config.yaml` 파일의 업데이트된 설정이 사용되었는지 확인하십시오.

    ```
    helm get values <myscanner>
    ```
    {: pre}


이제 IBM Container Scanner가 설치되었으며 livescan 에이전트가 [DaemonSet ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)로 클러스터에 배치됩니다. 스캐너가 `kube-system` 네임스페이스에 배치되지만, 모든 Kubernetes 네임스페이스에서 팟(Pod)에 지정된 모든 컨테이너를 스캔합니다(예: `default`). 



## 취약성 보고서 검토
{: #va_reviewing}

이미지를 배치하기 전에 취약한 패키지 및 비보안 앱 설정의 세부사항에 대해 취약성 어드바이저 보고서를 검토할 수 있습니다.
{:shortdesc}



1.  {{site.data.keyword.Bluemix_notm}}에 로그인하십시오.
2.  **카탈로그**를 클릭하십시오.
3.  **인프라**에서 **컨테이너**를 클릭하십시오.
4.  **컨테이너 레지스트리** 타일을 클릭하십시오.
5.  **취약성 어드바이저**를 펼치고 **스캔된 저장소**를 클릭하십시오.
6.  `최신`으로 태그가 지정된 이미지에 대한 보고서를 보려면 저장소의 행을 클릭하십시오. 보고서는 총 문제 수를 표시하고 취약한 패키지 또는 구성 문제가 있는지 여부를 표시합니다. 저장소에 `최신` 태그가 존재하지 않으면 최신 이미지가 사용됩니다.
7.  선택한 이미지의 각 취약한 패키지에 대한 정보를 보려면 **발견된 취약한 패키지** 표에서 **취약성** 열의 링크를 클릭하여 보고서를 여십시오.
    1.  자세한 정보를 보려면 요약을 펼치십시오.
    2.  운영 체제 배포자의 공지사항이 제공되면 **공식 공지사항** 열의 링크를 클릭하십시오.
8.  각 구성 문제에 대한 정보를 보려면 **발견된 구성 문제** 표에서 문제에 대한 행을 클릭하십시오.
9.  보고서에 표시된 각 문제에 대한 정정 조치를 수행하고 이미지를 다시 빌드하십시오. Dockerfile의 일부 문제는 [이미지의 문제점 해결](#va_report)에 제공된 코드를 사용하여 해결할 수 있습니다.

취약성이 있지만 수정하지 않은 경우 이러한 문제는 이 이미지로 빌드된 컨테이너의 보안에 영향을 줄 수 있습니다. 하지만 컨테이너에 보안 및 구성 문제가 있는 이미지를 계속 사용할 수 있습니다.

 



## CLI를 사용하여 취약성 보고서 검토
{: #va_registry_cli}

CLI를 사용하여 {{site.data.keyword.registrylong_notm}}의 네임스페이스에 저장된 Docker 이미지의 보안을 검토할 수 있습니다.
{:shortdesc}

1.  {{site.data.keyword.Bluemix_notm}} 계정에 이미지를 나열하십시오. 저장된 네임스페이스와 관계없이 모든 이미지의 목록이 리턴됩니다.

    ```
        bx cr image-list
    ```
    {: pre}

2.  **SECURITY STATUS** 열에서 상태를 확인하십시오.
    -   `No Issues`: 보안 문제가 없습니다.
    -   `X Issues`: 잠재적 보안 문제 또는 취약성이 있습니다.
    -   `Scanning`: 이미지를 스캔 중이며 최종 취약성 상태가 아직 판별되지 않았습니다.
4.  상태의 세부사항을 보려면 취약성 어드바이저 보고서를 검토하십시오.

    ```
        bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    CLI 출력에서 구성 문제에 대해 다음 정보를 볼 수 있습니다.
      - 보안 사례: 발견된 취약성에 대한 설명
      - 정정 조치: 취약성 수정 방법에 대한 세부사항


## 이미지의 공통 문제점 해결
{: #va_report}

취약성 어드바이저가 보고할 수 있는 공통 문제점에 대한 수정사항 예를 검토하십시오. Dockerfile을 업데이트하여 일부 문제점을 수정할 수 있습니다.
{:shortdesc}

### 최대 비밀번호 사용 기간, 최소 비밀번호 사용 기간(일) 및 최소 비밀번호 길이
{: #va_password}

**문제점**: 다음 취약성 중 하나 이상을 수신합니다.

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
{: codeblock}

### SSH 취약성
{: #ssh}

**문제점**: 다음 취약성이 리턴됩니다.

```
SSH server should not be installed.
```
{: screen}

**수정사항**: SSH를 사용하지 않고 `docker attach` 또는 `docker exec`를 사용하여 컨테이너에 액세스하십시오. Dockerfile에 SSH Server를 설치하는 단계가 포함되어 있지 않도록 하십시오.
