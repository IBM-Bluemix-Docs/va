---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-23"

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

취약성 어드바이저는 {{site.data.keyword.IBM}} 또는 써드파티에서 제공하거나 조직의 레지스트리 네임스페이스에 추가된 컨테이너 이미지의 보안 상태를 검사합니다. 각 클러스터에 컨테이너 스캐너가 설치된 경우 취약성 어드바이저에서는 실행 중인 컨테이너 상태도 확인합니다.
{:shortdesc}

네임스페이스에 이미지를 추가하면 취약성 어드바이저에서 보안 문제와 잠재적 취약성을 발견하기 위해 자동으로 이미지를 스캔합니다. 보안 문제가 발견되면 보고된 취약성을 수정하는 데 유용한 지시사항이 제공됩니다. 

취약성 어드바이저는 제안되는 수정사항 및 우수 사례가 포함된 심각도 상태 보고서를 생성하여 {{site.data.keyword.registrylong_notm}}를 위한 보안 관리를 제공합니다. 

취약성 어드바이저에서 발견된 문제는 이 이미지의 배치는 권장되지 않음을 표시하는 결과를 나타냅니다. 이미지를 배치하도록 선택한 경우 이미지에서 배치된 컨테이너는 컨테이너를 공격하거나 손상시키는 데 사용될 수 있는 문제를 알고 있습니다. 결과는 지정한 면제 정책을 기반으로 조정됩니다. 이 결과는 {{site.data.keyword.containerlong_notm}}에서 비보안 이미지의 배치를 방지하기 위해 컨테이너 이미지 보안 적용으로 사용될 수 있습니다. 

취약성 어드바이저에서 보고한 보안 및 구성 문제를 수정하면 {{site.data.keyword.cloud_notm}} 인프라를 보호할 수 있습니다.


## 취약성 어드바이저 정보
{: #about}

취약성 어드바이저는 이미지를 보호할 수 있는 기능을 제공합니다.

{:shortdesc}

 사용할 수 있는 기능은 다음과 같습니다.

-   문제를 확인하기 위한 이미지 스캔
-   각 클러스터에 [컨테이너 스캐너](#va_install_container_scanner)가 설치된 경우 실행 중인 컨테이너에 문제가 있는지 스캔
-   {{site.data.keyword.containerlong_notm}}에 대한 보안 사례를 기반으로 한 평가 보고서 제공
-   애플리케이션 유형의 서브세트에 대한 구성 파일을 보호하기 위한 권장사항 제공
-   보고된 [취약한 패키지](#packages) 또는 이 보고서의 [구성 문제](#app_configurations)를 수정하는 방법에 대한 지시사항 제공
-   결과를 [컨테이너 이미지 보안 적용](/docs/services/Registry/registry_security_enforce.html#security_enforce)에 제공
-   플래그 지정된 문제가 유스 케이스에 적용되지 않을 때 표시할 계정, 네임스페이스, 저장소 또는 태그 레벨에서 면제를 보고서에 적용
-   {{site.data.keyword.registrylong_notm}} 그래픽 사용자 인터페이스의 **태그** 보기에서 연관된 컨테이너에 대한 링크 제공. 컨테이너 스캐너가 설치된 클러스터에서 해당 이미지를 실행 중이고 사용 중인 컨테이너를 나열할 수 있습니다.


레지스트리 대시보드의 **정책 상태** 열에는 저장소의 상태가 표시됩니다. 연결된 보고서는 사용자의 이미지에 대해 양호한 클라우드 보안 사례를 식별합니다. 

취약성 어드바이저 대시보드는 이미지에 대한 보안의 개요 및 평가를 제공하고 컨테이너 스캐너가 설치된 경우 실행 중인 컨테이너에 연결합니다. 취약성 어드바이저 대시보드에 대한 자세한 정보는 [취약성 보고서 검토](#va_reviewing)를 참조하십시오.
	
	
**데이터 보호**

보안 문제에 대한 계정에서 이미지 및 컨테이너를 스캔하면 취약성 어드바이저가 다음 정보를 수집하고 저장하고 처리합니다.
- ID, 설명 및 이미지 이름을 포함한 자유 양식 필드(레지스트리, 네임스페이스, 저장소 이름 및 이미지 태그)
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

취약성 어드바이저에서는 지원되는 운영 체제를 사용 중인 이미지에서 취약한 패키지를 확인하고 취약성에 관한 보안 알림과 연결된 링크를 제공합니다.
{:shortdesc}

알려진 취약성 문제가 있는 패키지가 스캔 결과에 표시됩니다. 가능한 취약성은 다음 표에 나열된 Docker 이미지 유형에 대해 공개된 보안 공지사항을 사용하여 매일 업데이트됩니다. 일반적으로 취약한 패키지의 스캔을 통과하려면 취약성에 대한 수정사항을 포함한 이후 버전의 패키지가 필요합니다. 동일한 패키지에 다중 취약성이 나열될 수 있습니다. 이 경우 단일 패키지 업그레이드 시 다중 취약성이 해결될 수 있습니다.


  |Docker 기본 이미지|보안 알림 소스|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://git.alpinelinux.org/) 및 [CIRCL CVE Search ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.centos.org/pipermail/centos-announce/) 및 [CentOS CR announce archives ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux(RHEL)|[Red Hat Product Errata ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://usn.ubuntu.com/)|
  {: caption="표 1. 취약성 어드바이저에서 취약한 패키지를 검사하는 지원되는 Docker 기본 이미지" caption-side="top"}


### 구성 문제
{: #app_configurations}

구성 문제는 앱 설정 방법과 관련된 잠재적 보안 문제입니다. 보고된 많은 문제점은 Dockerfile을 업데이트하여 수정할 수 있습니다.
{:shortdesc}

이미지는 취약성 어드바이저에서 지원하는 운영 체제를 사용 중인 경우에만 스캔됩니다. 취약성 어드바이저는 다음 앱 유형에 대한 구성 설정을 검사합니다.
-   MySQL
-   NGINX
-   Apache


## 컨테이너 스캐너 설치
{: #va_install_container_scanner}

**시작하기 전에**

1.  {{site.data.keyword.Bluemix_notm}} CLI 클라이언트에 로그인하십시오. 연합 계정이 있는 경우 `--sso`를 사용하십시오.
2.  [Helm 차트를 사용할 대상 클러스터로 `kubectl` CLI](/docs/containers/cs_cli_install.html#cs_cli_configure)를 지정하십시오.
3.  컨테이너 스캐너의 서비스 ID와 API 키를 작성하고 이름을 제공하십시오.
    1.  서비스 ID를 작성하려면 다음 명령을 실행하십시오. 여기서 `<scanner_serviceID>`는 서비스 ID에 대해 선택한 이름입니다. 해당 **CRN**을 기록하십시오.
    
        ```
    	ibmcloud iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  서비스 API 키를 작성하십시오. 여기서 `<scanner_serviceID>`는 이전 단계에서 작성한 서비스 ID이며 `<scanner_APIkey_name>`은 스캐너 API 키에 대해 선택한 이름입니다. 
    
        ```
    	ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	스캐너 API 키가 리턴됩니다.
	
	스캐너 API 키를 나중에 검색할 수 없으므로 이를 안전하게 저장하십시오.
	{: tip}
	
    3.  `작성자` 역할을 부여하는 서비스 정책을 작성하십시오.
    		
        ```
    	ibmcloud iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

다음 단계를 완료하여 Helm 차트를 구성하십시오.

1.  [클러스터에서 Helm을 설정](/docs/containers/cs_integrations.html#helm)하십시오. RBAC 정책을 사용하여 Helm 틸러에 액세스 권한을 부여하는 경우 틸러 역할에 모든 네임스페이스에 대한 액세스 권한이 있는지 확인하십시오. 틸러 역할에 액세스 권한을 부여하면 컨테이너 스캐너가 모든 네임스페이스에서 컨테이너를 감시할 수 있습니다.

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
    <td>취약성 어드바이저 지역 엔드포인트 URL을 입력하십시오. URL을 가져오려면 <code>ibmcloud cr info</code>를 실행하고 <strong>컨테이너 레지스트리</strong> 주소를 검색하십시오. <code>registry</code>를 <code>va</code>로 바꾸십시오. 예: <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td><code>AccountID</code>를 클러스터가 있는 {{site.data.keyword.Bluemix_notm}} 계정 ID로 바꾸십시오. 계정 ID를 가져오려면 <code>ibmcloud account list</code>를 실행하십시오.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td><code>ClusterID</code>를 컨테이너 스캐너를 설치할 Kubernetes 클러스터로 바꾸십시오. 클러스터 ID를 나열하려면 <code>ibmcloud ks clusters</code>를 실행하십시오. <br> **팁**: 이름이 아닌 클러스터의 ID를 사용하십시오.
    </td>
    </tr>
    <tr>
    <td><code>APIKey</code></td>
    <td><code>APIKey</code>를 이전에 작성한 스캐너 API 키로 바꾸십시오.</td>
    </tr>
    </tbody></table>

5.  업데이트한 `config.yaml` 파일과 함께 Helm 차트를 클러스터에 설치하십시오. 업데이트한 특성은 차트의 구성 맵에 저장됩니다. `<myscanner>`를 Helm 차트에 대해 선택한 이름으로 바꾸십시오. Helm 차트 경로에 차트 저장소(예: `ibm-incubator`)를 포함하십시오.

    ```
helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    컨테이너 스캐너가 `kube-system` 네임스페이스에 설치되지만 모든 네임스페이스에서 컨테이너를 스캔합니다.
    {:tip}

6.  차트 배치 상태를 확인하십시오. 차트가 준비되면 **STATUS** 필드의 값은 `DEPLOYED`입니다.

    ```
helm status <myscanner>
    ```
    {: pre}

7.  차트가 배치되면 `config.yaml` 파일의 업데이트된 설정이 사용되었는지 확인하십시오.

    ```
helm get values <myscanner>
    ```
    {: pre}


이제 Container Scanner가 설치되었으며 에이전트가 [DaemonSet ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)으로 클러스터에 배치됩니다. 컨테이너 스캐너가 `kube-system` 네임스페이스에 배치되지만, 모든 Kubernetes 네임스페이스에서 팟(Pod)에 지정된 모든 컨테이너를 스캔합니다(예: `default`).


## 방화벽 뒤에서 컨테이너 스캐너 실행
{: #va_firewall}

방화벽이 발신 연결을 차단하는 경우 작업자 노드가 다음 표에 있는 IP 주소의 TCP 포트 <code>443</code>에서 컨테이너 스캐너에 액세스할 수 있도록 방화벽을 구성해야 합니다.
{:shortdesc}

 

<p>
  <table summary=" 행은 왼쪽에서 오른쪽으로 읽어야 하며 첫 번째 열에는 서버 구역, 두 번째 열에는 일치하는 IP 주소가 표시됩니다.">
  <caption>발신 트래픽을 위해 열린 IP 주소</caption>
      <thead>
      <th>지역</th>
      <th>IP 주소</th>
      </thead>
    <tbody>
      <tr>
         <td>아시아 태평양 남부</td>
         <td><code>168.1.40.158</code><br><code>130.198.65.182</code></td>
      </tr>
      <tr>
         <td>유럽 중앙</td>
         <td><code>159.8.220.182</code><br><code>158.177.74.102</code></td>
        </tr>
      <tr>
        <td>영국 남부</td>
        <td><code>158.175.71.134</code><br><code>5.10.111.190</code></td>
      </tr>
      <tr>
        <td>미국 동부</td>
         <td><code>169.60.73.158</code><br><code>169.61.84.102</code></td>
      </tr>
      <tr>
        <td>미국 남부</td>
        <td><code>169.47.103.118</code><br><code>169.48.165.6</code></td>
      </tr>
      </tbody>
    </table>
</p>


## 조직 면제 정책 설정
{: #va_managing_policy}

{{site.data.keyword.Bluemix_notm}} 조직의 보안을 관리하려면 정책 설정을 사용하여 문제 면제 여부를 판별할 수 있습니다. 사용자 정책에 따라 면제된 문제를 처리한 후 보안 문제가 없는 이미지에서만 배치가 허용되도록 컨테이너 이미지 보안 적용을 사용하게 선택할 수도 있습니다.
{:shortdesc}

클러스터에 컨테이너 이미지 보안 적용이 배치되지 않은 경우 보안 상태와 상관없이 이미지에서 컨테이너를 배치할 수 있습니다. 컨테이너 이미지 보안 적용을 배치하는 방법을 알아보려면 [보안 적용 설치](/docs/services/Registry/registry_security_enforce.html#security_enforce)를 참조하십시오.

컨테이너 이미지 보안 적용을 사용하는 경우 취약성 어드바이저에서 발견한 보안 문제 때문에 이미지에서 컨테이너가 배치되지 않습니다. 발견된 문제가 있는 이미지를 배치할 수 있으려면 정책에 면제가 추가되어야 합니다.

### GUI를 사용하여 조직 면제 정책 설정
{: #va_managing_policy_gui}

GUI를 사용하여 정책에 면제를 설정하려면 다음 단계를 완료하십시오.

1.  {{site.data.keyword.Bluemix_notm}}에 로그인하십시오. GUI에서 취약성 어드바이저에 로그인해야 합니다.
2.  **메뉴** 아이콘을 클릭하고 **컨테이너**를 클릭하십시오.
3.  **취약성 어드바이저**에서 **정책 설정**을 클릭하십시오.
4.  **면제 작성**을 클릭하십시오.
5.  문제 유형을 선택하십시오.
6.  문제 ID를 입력하십시오. 

    [취약성 보고서](#va_reviewing)에서 이 정보를 찾을 수 있습니다. **취약성 ID** 열에는 CVE 또는 보안 알림 문제에 사용할 ID가 포함되어 있습니다. **구성 문제 ID** 열에는 구성 문제에 사용할 ID가 포함되어 있습니다.
    {: tip}


7.  면제를 적용할 레지스트리 네임스페이스, 저장소 및 태그를 선택하십시오.
8.  **저장**을 클릭하십시오.

관련 행 위에 마우스를 두고 **옵션 목록 열기 및 닫기** 아이콘을 클릭하여 면제를 편집하고 제거할 수도 있습니다.

### CLI를 사용하여 조직 면제 정책 설정
{: #va_managing_policy_cli}

CLI를 사용하여 정책에 면제를 설정하려면 다음 명령을 실행할 수 있습니다.

-  보안 문제 면제를 작성하려면 [`ibmcloud cr exemption-add`](/docs/services/Registry/registry_cli.html#bx_cr_exemption_add) 명령을 실행하십시오.
-  보안 문제 면제를 나열하려면 [`ibmcloud cr exemption-list`](/docs/services/Registry/registry_cli.html#bx_cr_exemption_list) 명령을 실행하십시오.
-  면제할 수 있는 보안 문제 유형을 나열하려면 [`ibmcloud cr exemption-types`](/docs/services/Registry/registry_cli.html#bx_cr_exemption_types) 명령을 실행하십시오.
-  보안 문제 면제를 삭제하려면 [`ibmcloud cr exemption-rm`](/docs/services/Registry/registry_cli.html#bx_cr_exemption_rm) 명령을 실행하십시오.

명령에 대한 자세한 정보는 명령을 실행할 때 `--help` 플래그를 사용할 수 있습니다.


## 취약성 보고서 검토
{: #va_reviewing}

이미지를 배치하기 전에 취약성 어드바이저 보고서를 검토하여 취약한 패키지와 비보안 컨테이너 또는 앱 설정의 세부사항을 확인할 수 있습니다. 또한 이미지가 조직 정책을 준수하는지 여부를 확인할 수 있습니다.
{:shortdesc}

발견된 문제를 해결하지 않은 경우 이러한 문제는 이 이미지를 사용하여 빌드된 컨테이너의 보안에 영향을 줄 수 있습니다. 컨테이너 이미지 보안 적용이 배치되지 않은 경우 컨테이너에 보안 및 구성 문제가 있는 이미지를 계속 사용할 수 있습니다. 컨테이너 이미지 보안 적용이 배치되고 이미지용으로 활성인 경우 이 이미지에서 컨테이너를 배치할 수 있도록 정책에서 발견된 모든 문제를 면제해야 합니다. 

컨테이너 이미지 보안 적용에서 취약성 어드바이저 문제 적용 범위를 구성하려면 [정책 사용자 정의](/docs/services/Registry/registry_security_enforce.html#customize_policies)를 참조하십시오.
{:tip}

이미지가 조직 정책에 따라 설정된 요구사항을 만족하지 않으면 해당 요구사항을 충족하도록 이미지를 구성해야 배치할 수 있습니다. 조직 정책을 보고 변경하는 방법에 대한 자세한 정보는 [조직 면제 정책 설정](#va_managing_policy)을 참조하십시오.
{:tip}

컨테이너 스캐너가 배치되면 이미지를 배치한 후  취약성 어드바이저에서 컨테이너의 보안 및 구성 문제를 계속 스캔합니다. [컨테이너 보고서 검토](#va_reviewing_container)에 설명된 단계를 따라 발견된 모든 문제를 해결할 수 있습니다.

### GUI를 사용하여 취약성 보고서 검토
{: #va_reviewing_gui}

GUI를 사용하여 {{site.data.keyword.registrylong_notm}}의 네임스페이스에 저장된 Docker 이미지의 보안을 검토할 수 있습니다.
{:shortdesc}

1.  {{site.data.keyword.Bluemix_notm}}에 로그인하십시오.
2.  카탈로그에서 **인프라** 아래의 **컨테이너**를 클릭하십시오.
3.  **컨테이너 레지스트리** 타일을 클릭하십시오.
4.  **취약성 어드바이저**를 펼치고 **스캔된 저장소**를 클릭하십시오.
5.  `최신`으로 태그가 지정된 이미지에 대한 보고서를 보려면 저장소의 행을 클릭하십시오. 보고서는 총 문제 수를 표시하고 취약한 패키지 또는 구성 문제가 있는지 여부를 표시합니다. 저장소에 `최신` 태그가 존재하지 않으면 최신 이미지가 사용됩니다.
6.  선택한 이미지의 각 취약한 패키지에 대한 정보를 보려면 **발견된 취약한 패키지** 표에서 **취약성** 열의 링크를 클릭하여 보고서를 여십시오.
    1.  자세한 정보를 보려면 요약을 펼치십시오.
    2.  운영 체제 배포자의 공지사항이 제공되면 **공식 공지사항** 열의 링크를 클릭하십시오.
7.  각 구성 문제에 대한 정보를 보려면 **발견된 구성 문제** 표에서 문제에 대한 행을 클릭하십시오.
8.  보고서에 표시된 각 문제에 대한 정정 조치를 완료하고 이미지를 다시 빌드하십시오.


### CLI를 사용하여 취약성 보고서 검토
{: #va_registry_cli}

CLI를 사용하여 {{site.data.keyword.registrylong_notm}}의 네임스페이스에 저장된 Docker 이미지의 보안을 검토할 수 있습니다.
{:shortdesc}

1.  {{site.data.keyword.Bluemix_notm}} 계정에 이미지를 나열하십시오. 저장된 네임스페이스와 관계없이 모든 이미지의 목록이 리턴됩니다.

    ```
    ibmcloud cr image-list
    ```
    {: pre}

2.  **SECURITY STATUS** 열에서 상태를 확인하십시오.
    -   `No Issues`: 보안 문제가 없습니다.
    -   `<X> Issues`: `<X>` 잠재적 보안 문제 또는 취약성이 있습니다. 여기서 `<X>`는 문제의 갯수입니다.
    -   `Scanning`: 이미지를 스캔 중이며 최종 취약성 상태가 아직 판별되지 않았습니다.
    
3.  상태의 세부사항을 보려면 취약성 어드바이저 보고서를 검토하십시오.

    ```
    ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    CLI 출력에서 구성 문제에 대해 다음 정보를 볼 수 있습니다.
      - **보안 사례**: 발견된 취약성에 대한 설명
      - **정정 조치**: 취약성 수정 방법에 대한 세부사항


## 컨테이너 보고서 검토
{: #va_reviewing_container}

대시보드에서 보안이 조직의 정책을 준수하는지 여부를 판별하기 위해 컨테이너의 상태를 볼 수 있습니다. 또한 취약한 패키지와 비보안 컨테이너 또는 애플리케이션 설정에 대해 자세하게 설명되어 있으며 컨테이너가 조직의 정책을 준수하는지 여부를 표시하는 컨테이너의 보안 보고서를 검토할 수도 있습니다.
{:shortdesc}

**정책 상태** 필드를 검토하여 영역에서 실행 중인 컨테이너가 조직 정책을 계속 준수하는지 확인하십시오. 다음 조건 중 하나로 상태가 표시됩니다.

-   정책 준수 - 보안 또는 구성 문제가 없습니다.
-   정책을 준수하지 않음 - 취약성 어드바이저에서 컨테이너가 정책을 준수하지 않게 하는 잠재적 보안 또는 구성 문제를 발견했습니다. 조직 정책에서 취약한 이미지 배치를 허용하면 이미지가 `Deploy with Caution` 상태로 이미지가 배치되며 이미지를 배치한 사용자에게 경고가 전송됩니다.
-   불완전 평가 - 스캔이 완료되지 않았습니다. 스캔이 계속 실행 중이거나 해당 컨테이너 인스턴스의 운영 체제가 호환 가능하지 않을 수 있습니다.

보고된 보안 또는 구성 문제에 대한 보안 보고서 및 법령을 보고 다음 단계를 완료하여 컨테이너가 최대한 안전한지 확인하십시오.

1.  보고서를 보려는 컨테이너를 선택하십시오.
    1.  카탈로그에서 **컨테이너**를 선택하고 **컨테이너 레지스트리**를 클릭하십시오.
    2.  **개인용 저장소** 탭을 선택하고 원하는 저장소의 행을 선택하십시오.
    3.  원하는 이미지 태그의 행을 선택하십시오.
    4.  **연관된 컨테이너** 탭을 선택한 후 원하는 컨테이너의 행을 선택하십시오. 보안 보고서가 열립니다.
2.  이미지에서 각 패키지에 대해 잠재적 보안 및 구성 문제를 보기 위한 섹션을 검토하십시오.

      -   **취약성**: 알려진 취약성 문제가 있는 패키지를 나열합니다. 목록은 [취약성 유형](#types)에 나열된 Docker 이미지 유형에 대해 공개된 보안 공지사항을 사용하여 매일 업데이트됩니다. 일반적으로 취약한 패키지의 스캔을 통과하려면 취약성에 대한 수정사항을 포함한 이후 버전의 패키지가 필요합니다. 동일한 패키지에 다중 취약성이 나열될 수 있습니다. 이 경우 단일 패키지 업그레이드 시 여러 문제가 정정될 수 있습니다. 패키지에 대한 자세한 정보와 패키지를 업데이트하는 단계를 보려면 보안 공지사항 코드를 클릭하십시오.

    -   **구성 문제**: 컨테이너와 비보안 컨테이너에 대한 애플리케이션 설정의 보안을 강화하기 위해 제안할 수 있는 사항을 나열합니다. 문제를 해결하는 방법을 보려면 행을 펼치십시오.

   나열된 각 항목에 정정 조치 또는 제안이 제공됩니다.
   
3.  각 보안 문제마다 정책 상태를 검토하십시오. 정책 상태는 이 문제가 면제되는지 여부를 표시합니다.

    -  **활성**: 면제되지 않는 문제가 있으며 문제는 보안 상태에 영향을 줍니다.
    -  **면제**: 이 문제는 정책 설정으로 면제됩니다.
    -  **부분 면제**: 이 문제는 하나 이상의 보안 공지사항과 연관됩니다. 보안 공지사항은 모두 면제되지 않습니다.

4.  문제점을 해결할 수 있도록 컨테이너를 업데이트하는 방법을 결정하십시오.

    **중요:** 컨테이너 이미지에 대한 문제점을 해결하려면 이전 인스턴스를 삭제한 후 다시 배치해야 합니다. 이는 기존 컨테이너 내의 데이터가 유실됨을 의미합니다. 컨테이너를 다시 배치하는 데 적합한 방법을 선택하기 위해 컨테이너 아키텍처에 대해 잘 이해하고 있어야 합니다.

    **예**

    -   컨테이너가 컴퓨팅하는 데이터와 분리되는 경우 데이터 유실 없이 컨테이너를 중지하여 삭제하고, 이미지에 필요한 변경사항을 수행하고, 다시 배치할 수 있습니다.
    -   {{site.data.keyword.Bluemix_notm}} 서비스(예: [Delivery Pipeline](/docs/services/ContinuousDelivery/pipeline_about.html#deliverypipeline_about))를 사용하여 취약한 컨테이너 인스턴스를 업데이트하는 데 도움을 받을 수 있습니다.
    -   마이크로서비스 아키텍처에서 보안 또는 구성 문제를 수정하는 동안 트래픽을 다른 컨테이너 인스턴스로 라우트하고 빨간색/검정색으로 된 배치에 새 이미지를 푸시할 수 있습니다.

5.  지금 문제를 해결할 수 없는 경우 정책 설정에서 문제를 면제하면 문제로 인해 컨테이너 배치가 차단되지 않습니다. 문제를 면제하려면 **옵션 목록 열기 및 닫기** 아이콘을 클릭하고 **면제 작성**을 클릭하십시오. [조직 면제 정책 설정](#va_managing_policy)을 참조하십시오.

6.  **보안** 보고서에 설명된 문제를 수정하고 선택한 방법에 따라 이미지를 다시 빌드하거나 컨테이너를 다시 배치하십시오.
