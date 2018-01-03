---

copyright:
  years: 2015, 2017

lastupdated: "2017-03-07"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# IBM MQ Bluemix Container 이미지
{: #ibm_mq}

ibm-mq 이미지는 {{site.data.keyword.containershort}}를 위해 제공됩니다. 여기에는 {{site.data.keyword.IBM}} MQ Advanced for Developers(보증되지 않음)가 포함됩니다. 

{:shortdesc}


## 작동 방법
{: #ibm_mq_how}

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers에는 고유한 메시징 솔루션 및 {{site.data.keyword.IBM_notm}} MQ 애플리케이션의 개발을 시작하는 데 필요한 모든 것이 포함되어 있습니다.  

{{site.data.keyword.Bluemix_notm}}의 메시징 솔루션 및 {{site.data.keyword.IBM_notm}} MQ 애플리케이션을 개발하기 위해 {{site.data.keyword.containershort}}에 ibm-mq Docker 이미지를 배치할 수 있습니다. 그런 다음, 큐 관리자를 작성 및 실행하고 웹 UI 또는 터미널을 통해 큐 관리자를 구성하여 메시징 솔루션 또는 애플리케이션 요구사항을 충족시킬 수 있습니다. 

**참고:** 개발자 및 단위 테스트용으로만 ibm-mq 이미지를 사용할 수 있습니다. 또한 이미지를 사용하여 제품을 탐색하고, 튜토리얼을 통해 학습하며, 사용자의 조직에 대한 {{site.data.keyword.IBM_notm}} MQ의 기여도를 평가할 수도 있습니다. 

## 포함된 항목
{: #ibm_mq_what}

이 ibm-mq 이미지에는 개발 및 단위 테스트용으로 사용할 수 있는 제품의 전체 기능 버전인 {{site.data.keyword.IBM_notm}} MQ Advanced for Developers 버전 9.0.x를 위한 소프트웨어 패키지가 포함됩니다. 무료로 이 버전을 다운로드할 수 있으며 라이센스의 조항에 따라 자유롭게 사용할 수 있습니다. 

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers는 Windows 64비트 운영 체제 및 Linux on x86-64 운영 체제에서 사용 가능합니다. 모든 제품 필수 소프트웨어는 다운로드 패키지에 포함되어 있습니다. 자세한 정보는 [IBM MQ 다운로드 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www-03.ibm.com/software/products/lv/ibm-mq-advanced-for-developers){: new_window}를 참조하십시오.

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers에 포함된 기능에 대한 자세한 정보는 [IBM MQ ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}를 참조하십시오. 



## 사용 제한사항
{: #ibm_mq_usage}

다음 표는 {{site.data.keyword.Bluemix_notm}}에서 ibm-mq 이미지의 무료 사용에 적용되는 제한사항을 표시합니다. 

| 환경 | 무료 사용 제한사항|
|-------------|-------------------------|
| 개발 | 개발용 단일 컨테이너를 작성하기 위한 ibm-mq 이미지의 무제한 무료 사용|
| 테스트 | 단위 테스트용 단일 컨테이너를 작성하기 위한 ibm-mq 이미지의 무제한 무료 사용. 허용되는 테스트에 대한 자세한 정보는 [라이센스 정보](getstarted.html#ibm_mq_license )를 확인하십시오.|
| 프로덕션 | {{site.data.keyword.IBM_notm}} MQ Advanced for Developer 라이센스는 프로덕션용 사용을 허용하지 않습니다. |

**참고:** ibm-mq 이미지의 가격 책정은 {{site.data.keyword.Bluemix_notm}}에 사용하는 컨테이너의 가격 책정과 관련이 없습니다. 


## 라이센스 정보
{: #ibm_mq_license}

라이센스에 대한 정보:

*	Dockerfiles 및 관련 스크립트는 [Apache License 2.0 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.apache.org/licenses/LICENSE-2.0.html){: new_window}에 의거해서 라이센스가 부여됩니다.
* [{{site.data.keyword.IBM_notm}} MQ Advanced for Developers에 대한 라이센스 정보 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/973FA648A5DE42948525806E004CC757?OpenDocument){: new_window}를 참조하십시오.

**참고:** 라이센스에서는 더 이상의 추가 배포는 허용하지 않습니다. 이미지의 {{site.data.keyword.IBM_notm}} MQ에 대한 조항에서는 개발 및 단위 테스트를 위한 사용을 제한합니다. 


## 시작하기
{: #ibm_mq_get_started}

{{site.data.keyword.Bluemix_notm}}에서 실행되는 컨테이너의 {{site.data.keyword.IBM_notm}} MQ를 실행하고 관리하기 위한 다음 단계를 완료하십시오. 

1. [ibm-mq 이미지에 따라 컨테이너를 프로비저닝하십시오.](ibm_mq.html#ibm_mq_provision)

    **참고:** 컨테이너는 컨테이너 내부에 있는 큐 관리자의 실행 상태와 연관이 있습니다. 콘솔 또는 명령행을 사용하여 큐 관리자를 중지하는 경우 컨테이너는 곧 중지됩니다.  

2. 컨테이너에서 실행되는 {{site.data.keyword.IBM_notm}} MQ 자원을 관리하십시오. IBM MQ 웹 콘솔에서 그래픽으로 관리하거나 터미널에서 명령을 사용하여 프로그래밍 방식으로 관리할 수 있습니다.  

    1. 컨테이너에서 {{site.data.keyword.IBM_notm}} MQ 자원을 관리하려면 다음 옵션 중 하나를 선택하십시오.
    
        *	{{site.data.keyword.IBM_notm}} MQ 웹 콘솔을 실행하고 {{site.data.keyword.IBM_notm}} MQ를 그래픽으로 관리하십시오. 자세한 정보는 [IBM MQ 웹 콘솔 실행](ibm_mq.html#ibm_mq_webui)을 참조하십시오.
        *	터미널에서 Docker CLI를 사용하십시오. 자세한 정보는 [Docker CLI를 사용하도록 터미널 설정](ibm_mq.html#ibm_mq_docker_commands)을 참조하십시오.
        * 터미널에서 IBM Bluemix Container Service CLI를 사용하십시오. 자세한 정보는 [IBM Bluemix Container Service CLI를 사용하도록 터미널 설정](ibm_mq.html#ibm_mq_containers_cli)을 참조하십시오.
        
    2. 컨테이너에서 실행되는 {{site.data.keyword.IBM_notm}} MQ 큐 관리자를 구성하십시오.  

        * 명령행을 통해 큐 관리자를 관리하려면 *runmqsc* 프로그램을 사용하십시오. `runmqsc QM_Name`명령을 실행하십시오. 여기서, *QM_Name*은 큐 관리자의 이름입니다. 자세한 정보는 [명령행을 통해 컨테이너 관리](ibm_mq.html#ibm_mq_access)를 참조하십시오.
    
        * 큐 관리자를 그래픽으로 관리하려면 웹 UI를 사용하십시오. 
        
    3. 큐 관리자에 메시지 큐를 작성하고 메시지 큐에 대한 메시지를 저장하거나 가져오려면 [IBM MQ Knowledge Center ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}에 있는 지시사항을 따르십시오. 
    
        예를 들어, `runmqsc`에서 `DEFINE QLOCAL('MY.Q')` 명령을 실행하여 새 로컬 큐를 작성하십시오. 여기서, *MY.Q*는 터미널의 로컬 큐 이름입니다. 'runmqsc`에서 `DISPLAY QLOCAL('MY.Q')` 명령을 실행하여 웹 콘솔의 큐를 보거나 큐 및 해당 특성을 표시할 수 있습니다. 
    
3. [선택사항] 컨테이너 내부에 있는 {{site.data.keyword.IBM_notm}} MQ 로그를 모니터하십시오. 자세한 정보는 [명령행을 통해 컨테이너 관리](ibm_mq.html#ibm_mq_access)를 참조하십시오.

IBM MQ 시작하기 소개에 대한 자세한 정보는 [IBM MQ 버전 9.0.x 시작 페이지 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}를 참조하십시오.

이미지 및 Docker에서 자체 이미지를 로컬로 빌드하는 방법에 대한 자세한 정보는 [MQ Docker GitHub 페이지 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/mq-docker){: new_window}를 참조하십시오.
    
IBM MQ에 대한 최신 블로그 및 정보는 [MQ Developer 블로그 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/developerworks/community/blogs/messaging){: new_window}를 참조하십시오. 


### ibm-mq 이미지에 따른 컨테이너 프로비저닝
{: #ibm_mq_provision}

{{site.data.keyword.IBM_notm}}에서 제공하는 ibm-mq 이미지에 따라 {{site.data.keyword.Bluemix_notm}}에 Docker 컨테이너를 프로비저닝하기 위한 다음 단계를 완료하십시오. 

1.	{{site.data.keyword.Bluemix_notm}}에 로그인하십시오. 로그인할 때 {{site.data.keyword.Bluemix_notm}} ID를 사용하십시오. 
2.	카탈로그에서 **컨테이너**를 선택하고 *ibm-mq* 이미지를 선택하십시오. 
3.	단일 인스턴스 컨테이너를 작성하려면 **단일**을 선택하십시오. 이는 개발 및 테스트 용도로 사용될 수 있습니다. 
4.	컨테이너의 이름을 입력하십시오(예: mq).
5.	컨테이너의 크기를 선택하십시오. 
6.	공인 IP 주소 필드에서 **공인 IP 요청 및 바인드**를 선택하십시오.
7.	공용 포트 필드에서 **1414/tcp, 9443/tcp**를 지정하십시오.
8.	고급 옵션을 펼치고 **새 환경 변수 추가**를 클릭하고 환경 변수 즉, LICENSE 및 MQ_QMGR_NAME을 입력하십시오.

    * IBM MQ Advanced for Developers 라이센스 조항에 동의하려면 **라이센스**의 값을 **동의**로 설정하십시오.  
    
        동의하기 전에 라이센스를 보려면 라이센스를 “보기”로 설정하십시오. 그러면 컨테이너 로그에 라이센스가 인쇄됩니다.  
    
        영어가 모국어가 아닌 경우 **LANG**이라는 다른 환경 변수를 *zh_TW*(대만어), *zh*(중국어), *cs*(체코어), *en*(영어), *fr*(프랑스어), *de*(독일어), *el*(그리스어), *id*(인도네시아어), *it*(이탈리아어), *ja*(일본어), *ko*(한국어), *lt*(리투아니아어), *pl*(폴란드어), *pt*(포르투갈어), *ru*(러시아어), *sl*(슬로베니아어), *es*(스페인어) 또는 *tr*(터키어) 중 하나로 설정하여 다른 언어로 된 라이센스 파일을 볼 수 있습니다. 

    * *MYMQ* 값으로 **MQ_QMGR_NAME**의 값을 설정하십시오. 여기서, *MYMQ*는 선택한 큐 관리자 이름입니다. 
 
        **참고:** LICENSE 및 MQ_QMGR_NAME 환경 변수에 대한 값을 지정하지 않은 경우 컨테이너는 작성되지만 시작하지 않습니다.  
    
    * [선택사항] 기본적으로 {{site.data.keyword.IBM_notm}} MQ 웹 콘솔은 컨테이너에서 시작됩니다. 기본 작동을 변경하려면 **MQ_DISABLE_WEB_CONSOLE** 환경 변수 값을 *true*로 설정하십시오. 
    
        **참고:** 웹 콘솔을 사용하려면 *MQ_DISABLE_WEB_CONSOLE* 환경 변수를 설정하지 마십시오.

    * [선택사항] {{site.data.keyword.Bluemix_notm}} UI를 통해 MQ 로그를 보려면 **LOG_LOCATIONS** 환경 변수를 `/var/mqm/qmgr/QM_Name/errors/AMQERR01.LOG` 값으로 설정하십시오. 여기서, *QM_Name*은 *MQ_QMGR_NAME* 환경 변수에 대해 설정한 값입니다. 
    
    *	[선택사항] 기본적으로 {{site.data.keyword.IBM_notm}} MQ Developer 기본 오브젝트가 작성됩니다. 기본 작동을 변경하려면 **MQ_DEV** 환경 변수를 *false*로 설정하고 사용자 정의 값을 사용하여 IBM MQ에 대한 기본 설치 값을 수정하십시오. 자세한 정보는 [기본 설치 값 수정](ibm_mq.html#ibm_mq_dev_default)을 참조하십시오. 
 
9. [선택사항] 고급 옵션 섹션에서 볼륨을 지정하십시오. {{site.data.keyword.IBM_notm}} MQ 데이터를 저장하고 컨테이너 마이그레이션을 통해 영구적으로 구성하기 위해 {{site.data.keyword.Bluemix_notm}} 볼륨을 사용하십시오. 컨테이너가 종료되는 경우 볼륨에 저장되지 않은 {{site.data.keyword.IBM_notm}} MQ 데이터가 유실됩니다. 

    ibm-mq 이미지는 {{site.data.keyword.Bluemix_notm}} 볼륨에서 사용할 수 있으나 볼륨은 발견할 **/mnt/mqm**에 마운트되어야 하고 {{site.data.keyword.IBM_notm}} MQ로 활용되어야 합니다. 

    기본적으로 컨테이너가 중지될 때 볼륨 없이 작성된 컨테이너에는 IBM MQ 데이터가 유실됩니다. 이를 방지하려면 지속적 메시징 및 {{site.data.keyword.Bluemix_notm}} 볼륨을 사용해야 합니다. 

    1. **볼륨 작성**을 클릭하고 볼륨 이름을 입력한 후 파일 공유를 선택하십시오. 
    2. **기존 볼륨 지정**을 클릭하고 작성한 볼륨을 선택하십시오. 경로 상자에 **/mnt/mqm**을 입력하십시오. **읽기 전용** 상자가 선택되지 않았는지 확인하십시오. 

    **참고:** IBM MQ 데이터 디렉토리로 사용하기 위해 볼륨을 마운트하는 경우 */mnt/mqm* 위치에 볼륨을 마운트하십시오. */mnt/mqm* ibm-mq 스크립트에 볼륨을 마운트하면 볼륨이 발견되고 사용할 IBM MQ 파일 시스템이 구성됩니다. 
     
10.	**작성**을 클릭한 후 컨테이너의 실행이 시작될 때까지 기다리십시오. 

    컨테이터가 작성된 후 컨테이너 대시보드가 열립니다. 컨테이너의 상태가 **실행 중**으로 설정되었는지 확인하십시오.  

    컨테이너의 로그를 확인하려면 **모니터링 및 로그**를 클릭하고 **로깅** 탭을 선택하십시오. 로그가 표시됩니다. 로그에 대한 고급 분석을 보려면 **고급 보기**를 클릭하여 Kibana로 된 로그를 표시하십시오. 

큐 관리자를 작성하고 시작했습니다. 이제 다음 연결 세부사항을 사용하여 MQ 애플리케이션에 연결할 수 있습니다. 

| 연결 특성 | 값 |
|-------|-------|
| IP 주소 | 컨테이너의 공인 IP 주소 |
| 포트 | 1414 |
| 채널 | DEV.APP.SVRCONN |


### 명령행을 통한 컨테이너 관리
{: #ibm_mq_access}

{{site.data.keyword.IBM_notm}} MQ를 실행하는 컨테이너가 {{site.data.keyword.Bluemix_notm}}에 배치된 후 컨테이너 및 컨테이너의 상태에 액세스하고 {{site.data.keyword.IBM_notm}} MQ 설치를 유효성 검증할 수 있습니다. 

컨테이너에서 {{site.data.keyword.IBM_notm}} MQ의 설정 및 구성을 확인하려면 다음 단계를 완료하십시오. 

1.	IBM Containers CLI를 설정하십시오. 자세한 정보는 [Bluemix CLI 페이지](https://plugins.ng.bluemix.net/ui/home.html)를 참조하고 지시사항을 따라 최신 Cloud Foundry Container 서비스 플러그인을 설치하십시오. 

2.	터미널에서 {{site.data.keyword.Bluemix_notm}} 조직 및 영역에 로그인하십시오. 여기서, 컨테이너는 {{site.data.keyword.Bluemix_notm}} ID를 사용하여 실행됩니다. 다음 명령을 실행하십시오. 

    1. `bx login`
    2. `bx ic init`
    
3.	컨테이너의 상태를 확인하십시오. `bx ic ps` 명령을 실행하십시오. 

4.	Bash 세션을 컨테이너에 연결하십시오. 

    `bx ic exec -it container_name /bin/bash`
    
    여기서, container_name은 컨테이너의 이름입니다. 
    
5.	MQ 설정을 관리하기 위한 명령을 실행하십시오. 예를 들면, 다음과 같습니다. 

| 명령 | 조치 |
|---------|---------|
| dspmqver | IBM MQ 버전 및 빌드 정보를 확인하십시오. |
| dspmq | 컨테이너에서 실행되는 큐 관리자의 상태를 확인하십시오. | 
| runmqsc | MQ 자원을 관리하십시오. |

MQ 로그를 모니터하려면 다음 옵션 중 하나를 선택하십시오. 

* 터미널이 {{site.data.keyword.containershort}} CLI 명령을 실행하도록 구성된 경우 `bx ic exec container_id tail -f /var/mqm/qmgr/QM_ Name/errors/AMQERR01.LOG` 명령을 실행하십시오. 여기서, *container_id*는 컨테이너의 이름이고 *QM_ Name*은 큐 관리자의 이름입니다. 
    
* 터미널이 Docker CLI 명령을 실행하도록 구성된 경우 `docker exec container_id tail -f /var/mqm/qmgr/QM_Name/errors/AMQERR01.LOG` 명령을 실행하십시오. 여기서, *container_id*는 컨테이너의 이름이고 *QM_Name*은 큐 관리자의 이름입니다. 
    
    

### IBM MQ 웹 콘솔 실행
{: #ibm_mq_webui}

{{site.data.keyword.IBM_notm}} MQ 웹 콘솔을 실행하려면 다음 단계를 완료하십시오. 

1.	{{site.data.keyword.Bluemix_notm}} 대시보드에서 컨테이너를 선택하고 컨테이너의 상태가 *실행 중*으로 설정되었는지 확인하십시오.
2.	컨테이너가 공용 포트 9443으로 구성되었는지 확인하십시오. 
3.	공인 IP를 찾거나 공인 IP를 요청 및 바인드하기 위해(아직 수행되지 않은 경우) 컨테이너 세부사항을 확인하십시오. 
4.	웹 브라우저에서 `https://DockerContainerPublicIP:9443/ibmmq/console/` URL을 실행하십시오. 여기서, DockerContainerPublicIP는 컨테이너의 공인 IP입니다.

    MQ 콘솔을 호스팅할 때 웹 서버가 사용하는 TLS 인증서가 생성되고 자체 서명되므로 보안 경고가 열립니다. **예외 추가**를 클릭하고 확인하십시오. 

    **참고:** 프로덕션 환경에서 브라우저로 인식되고 신뢰되는 자체 인증서를 사용하십시오. 이 인증서는 인증 기관에서 발행되어야 합니다.  
    
    브라우저에서 웹 사용자 인터페이스가 열립니다. 
    
5. 신임 정보를 사용하여 {{site.data.keyword.IBM_notm}} MQ 콘솔에 로그인하십시오. 

    1.	사용자: admin
    2.	비밀번호: passw0rd
    
이제 IBM MQ를 관리할 수 있습니다. 

### IBM Bluemix Container Service CLI를 사용하도록 터미널 설정
{: #ibm_mq_containers_cli}

{{site.data.keyword.containershort}} CLI를 사용하여 컨테이너에 직접 {{site.data.keyword.IBM_notm}} MQ 관리 명령을 실행하십시오. 

IBM MQ를 관리할 bx ic 명령을 실행하기 위해 터미널을 설정하려면 다음 명령을 완료하십시오. 

1.	{{site.data.keyword.containershort}} CLI를 설정하십시오. 자세한 정보는 [Bluemix CLI 페이지](https://plugins.ng.bluemix.net/ui/home.html)를 참조하고 지시사항을 따라 최신 Cloud Foundry Container 서비스 플러그인을 설치하십시오. 

2.	터미널에서 {{site.data.keyword.Bluemix_notm}}에 로그인하십시오. 다음 명령을 실행하십시오. 

    1. `bx login`
    2. `bx ic init`
    
3.	Bash 세션을 컨테이너에 연결하십시오. 

    `bx ic exec -it container_name /bin/bash`
    
    여기서, container_name은 컨테이너의 이름입니다. 
    


### Docker CLI를 사용하도록 터미널 설정
{: #ibm_mq_docker_commands}

Docker CLI를 사용하여 컨테이너에 직접 {{site.data.keyword.IBM_notm}} MQ 관리 명령을 실행하십시오. 

IBM MQ를 관리할 Docker 명령을 실행하기 위해 터미널을 설정하려면 다음 명령을 완료하십시오. 

1.	{{site.data.keyword.containershort}} CLI를 설정하십시오. 자세한 정보는 [Bluemix CLI 페이지](https://plugins.ng.bluemix.net/ui/home.html)를 참조하고 지시사항을 따라 최신 Cloud Foundry Container 서비스 플러그인을 설치하십시오. 

2.	터미널에서 {{site.data.keyword.Bluemix_notm}}에 로그인하십시오. 다음 명령을 실행하십시오. 

    1. `bx login`
    2. `bx ic init`
    3. 환경 변수 즉, DOCKER_HOST, DOCKER_CERT_PATH 및 DOCKER_TLS_VERIFY에 제공된 값을 복사하십시오. 
    4. {{site.data.keyword.containershort}}에 연결하기 위해 다음 변수를 설정하여 로컬 Docker 환경을 대체하십시오.
        
        `export DOCKER_HOST=<your_host_value>`
        
        `export DOCKER_CERT_PATH=<your_cert_path_value>`
        
        `export DOCKER_TLS_VERIFY=1`

3.	Bash 세션을 컨테이너에 연결하십시오. 

    `bx ic exec -it container_name /bin/bash`
    
    여기서, container_name은 컨테이너의 이름입니다. 
    
**참고:** 일부 Docker 명령만 이 옵션에 지원됩니다. 




### 기본 설치 값 수정
{: #ibm_mq_dev_default}

큐 관리자가 작성되면 {{site.data.keyword.IBM_notm}} MQ Developer 기본값으로 작성됩니다. 이 기본 오브젝트는 애플리케이션 개발 또는 {{site.data.keyword.IBM_notm}} MQ 평가까지도 빨리 시작할 수 있도록 설계되었습니다.  

다음 기본 오브젝트를 사용하여 큐 관리자가 작성됩니다.

* *passw0rd*의 비밀번호를 사용한 MQ 관리자 **admin**
*	비밀번호가 없는 MQ 애플리케이션 사용자 **app**
*	포트 1414/TCP에 바인드하도록 리스너 **DEV.LISTENER.TCP** 구성
*	MQ 클라이언트 애플리케이션 연결을 처리하도록 채널 **DEV.APP.SVRCONN** 구성
*	MQ 관리 연결을 처리하도록 채널 **DEV.ADMIN.SVRCONN** 구성
*	메시지를 배치할 수 있는 세 개의 큐 **DEV.QUEUE.1**, **DEV.QUEUE.2**, **DEV.QUEUE.3**
*	전달되지 않는 메시지가 배치될 데드 레터 큐 **DEV.DEAD.LETTER.QUEUE**
*	*/dev*의 토픽 문자열을 사용한 기본 토픽 **DEV.BASE.TOPIC**
*	모든 클라이언트 연결을 차단하기 위한 채널 인증 레코드(모든 클라이언트 연결이 채널 **DEV.APP.SVRCONN** 또는 **DEV.ADMIN.SVRCONN**을 통해 연결되지 않는 경우)
*	연결하는 모든 관리 사용자를 차단하기 위한 채널 인증 레코드(**DEV.ADMIN.SVRCONN** 채널을 통하지 않는 경우)
*	사용자 관리만 **DEV.ADMIN.SVRCONN** 채널에 연결해야 하는 채널 인증 레코드
*	관리 애플리케이션에서 연결 시 유효한 신임 정보를 전달하고 권한 검사를 위해 사용자 ID를 채택해야 하는 연결 인증 세트

이 기본 오브젝트를 사용하여 큐에 대한 메시지를 저장하고 가져오는 클라이언트 애플리케이션을 연결하거나 토픽을 구독하고 공개할 수 있습니다. 

작성한 개발자 기본값을 사용자 정의하려면 ibm-mq 이미지에서 컨테이너를 작성할 때 다음 환경 변수를 추가할 수 있습니다. 

| 환경 변수 | 용도 | 기본값 |
|----------------------|---------|---------|
| MQ_ADMIN_PASSWORD | 기본값에서 작성된 MQ 관리자의 비밀번호를 원하는 비밀번호로 변경하도록 이 변수를 설정하십시오. 비밀번호는 최소 8자여야 합니다. 관리 사용자의 사용자 이름은 admin으로 수정됩니다. | passw0rd |
| MQ_APP_PASSWORD | 애플리케이션 사용자의 비밀번호를 설정하도록 이 변수를 설정하십시오. 애플리케이션 사용자의 사용자 이름은 app로 수정됩니다. 이 환경 변수를 설정한 경우 MQ 클라이언트를 연결할 때 사용자 ID 및 비밀번호를 제공해야 합니다. <br> 비밀번호는 최소 8자여야 합니다. |  |
| MQ_TLS_KEYSTORE | TLS 조작을 위해 MQ 웹 콘솔 및 IBM MQ Queue Manager를 사용할 인증서가 포함된 PKCS#12 키 저장소의 위치에 이 환경 변수를 설정하십시오. <br> 이 환경 변수가 설정되면 CipherSpec ‘TLS_RSA_WITH_AES_256_GCM_SHA384’를 사용하여 작성된 개발자 채널이 TLS에 대해 사용으로 설정됩니다. <br> **참고:** {{site.data.keyword.Bluemix_notm}} 컨테이너 내부에 사용 가능한 키 저장소 파일을 작성해야 합니다. 이를 수행하기 위해 컨테이너를 작성할 때 키 저장소를 포함한 볼륨을 마운트할 수 있습니다. |  |
| MQ_TLS_PASSPHRASE | MQ_TLS_KEYSTORE 환경 변수에 지정된 키 저장소의 비밀번호를 설정하도록 이 환경 변수를 설정하십시오. |  |
| MQ_DEV | {{site.data.keyword.IBM_notm}} MQ Developer 기본 오브젝트가 작성되는지 여부를 제어하도록 이 환경 변수를 설정하십시오. | true |
| MQ_DISABLE_WEB_CONSOLE | 컨테이너의 CPU/RAM 사용량을 보존하려는 경우 웹 콘솔의 구성 및 시작을 사용 안함으로 설정하도록 이 환경 변수를 설정하십시오. |  |



