# Triple O 아키텍처

상태: Done

---

# Triple O 아키텍처

“**O**penStack-**O**n-**O**penStack”을 의미합니다. 이는 오픈스택을 통해 오픈스택을 배포하는 것입니다.
TripleO는 OpenStack 자체를 배포 도구로 활용하여 OpenStack 클라우드 배포를 단순화 하고 자동화하도록 설계  되었습니다.

Triple O 아키텍처는 운영을 효율적으로 하기 위해서 “`관리의 영역`”과 “`서비스 영역`”을 분리한 것으로 물리 장치를 제어하고 **관리하기 위한 언더클라우드(UnderCloud)**와 고객에게 클라우드 **서비스를 제공하기 위한 오버클라우드(OverCloud)**로 분리되어 있습니다.

## 오버클라우드와 언더클라우드

![스크린샷 2023-05-11 오후 1.58.14.png](Triple%20O%20%E1%84%8B%E1%85%A1%E1%84%8F%E1%85%B5%E1%84%90%E1%85%A6%E1%86%A8%E1%84%8E%E1%85%A5%20ce45d4580813486aa6a8ece69a09d6b2/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-05-11_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_1.58.14.png)

### 언더클라우드 (Undercloud)

언더클라우드는 오픈스택 클라우드 환경의 기본 구성 요소로, 오픈스택 컨트롤 플레인을 관리하는 데 사용됩니다. 주로 오픈스택의 **설치, 배포, 관리를 담당**하며, **인프라스트럭처의 기본 설정과 네트워크 설정 등을 수행**합니다. 언더클라우드는 오픈스택 설치 중심의 서버로서, 컨트롤러 노드, 네트워크 노드, 스토리지 노드 등으로 구성될 수 있습니다.

### 오버클라우드 (Overcloud)

오버클라우드는 실제 사용자들이 가상 머신 인스턴스를 생성하고 관리하는 오픈스택 클라우드 환경입니다. 오버클라우드는 **실제 사용자들에게 인프라스트럭처 리소스를 제공하는 역할을 수행**합니다. 가상 머신 인스턴스, 가상 네트워크, 스토리지 등이 오버클라우드의 주요 구성 요소입니다. 오버클라우드는 언더클라우드 위에 구축되며, 사용자들은 오버클라우드를 통해 자원을 요청하고, 가상 환경을 생성하고 관리할 수 있습니다.

## 오픈스택을 2-tier 로 분리하는 이유

가장큰 이유로는 6개월 마다 릴리즈되는 버전에 맞추어 주기적으로 업데이트와 유지보수를 수행함에 있습니다.

TripleO를 사용하여 관리영역, 서비스영역으로 나누었을떄 다음과 같은 장점을 가질 수 있습니다.

1. 롤링 업그레이드(Rolling Upgrade): TripleO는 롤링 업그레이드를 지원하여 오픈스택 클라우드를 업데이트할 때 중단 없이 서비스의 지속성을 유지할 수 있습니다. 이는 업데이트 과정에서 사용자의 가상 머신 인스턴스와 서비스 중단을 최소화하는 데 도움을 줍니다. (무중단)
2. 개별 구성 요소 업데이트(Individual Component Updates): TripleO는 오픈스택의 개별 구성 요소를 독립적으로 업데이트할 수 있습니다. 이는 필요한 경우 특정 구성 요소에 대한 업그레이드 또는 수정을 수행할 수 있다는 것을 의미합니다. 
3. 자동화된 유지보수(Automated Maintenance): TripleO는 자동화된 방식으로 유지보수 작업을 수행할 수 있습니다. 이를 통해 예방적인 유지보수 작업, 패치 적용, 자원 모니터링 등을 자동으로 처리할 수 있습니다.
4. 컨테이너 기반 배포(Containerized Deployment): TripleO는 컨테이너 기반의 배포를 지원하여 오픈스택 서비스를 격리하고 관리할 수 있습니다. 이를 통해 업데이트 및 확장이 용이하며, 서비스 간의 의존성 충돌을 줄일 수 있습니다.

### TripleO 를 이용한 Upgrade

TripleO 배포를 다음 주요 릴리즈버전으로 업그레이드 하기 위해선 “언더클라우드”를 업그레이드한 다음 “오버클라우드”를 업그레이드 합니다.

### 언더클라우드 업그레이드

*ex) Mitaka → Newton 버전예시* 

1. 이전 OpenStack 릴리즈 레포지토리를 비활성화합니다.
2. 언더 클라우드에서 새 릴리즈 레포지토리를 활성화합니다.

```bash
#이전 OpenStack 릴리스 리포지토리를 비활성화하고 언더클라우드에서 새 릴리스 리포지토리를 활성화합니다.
export CURRENT_VERSION=mitaka
export NEW_VERSION=newton

#현재 레포지토리를 백업하고 비활성화합니다. 레포지토리 파일의 이름은 설치에 따라 다르게 지정될 수 있습니다.
mkdir /home/stack/REPOBACKUP
sudo mv /etc/yum.repos.d/delorean* /home/stack/REPOBACKUP/

#새로운 버전의 레포지토리를 가져온후 활성화 합니다.
#모든 패키지에 대해 최신 RDO Newton Delorean 레포지토리 활성화 
sudo curl -L -o /etc/yum.repos.d/delorean-newton.repo https://trunk.rdoproject.org/centos7-newton/current/delorean.repo

#Newton Delorean Deps 저장소 활성화 
sudo curl -L -o /etc/yum.repos.d/delorean-deps-newton.repo https://trunk.rdoproject.org/centos7-newton/delorean-deps.repo

#핵심 openstack 패키지에 대해 마지막으로 알려진 양호한 RDO Trunk Delorean 리포지토리 활성화
sudo curl -L -o /etc/yum.repos.d/delorean.repo https://trunk.rdoproject.org/centos7-master/current-passed-ci/delorean.repo

#TripleO 패키지에 대해서만 최신 RDO Trunk Delorean 리포지토리 활성화
sudo curl -L -o /etc/yum.repos.d/delorean-current.repo https://trunk.rdoproject.org/centos7/current/delorean.repo
sudo sed -i 's/\[delorean\]/\[delorean-current\]/' /etc/yum.repos.d/delorean-current.repo
sudo /bin/bash -c "cat <<EOF>>/etc/yum.repos.d/delorean-current.repo

includepkgs=diskimage-builder,instack,instack-undercloud,os-apply-config,os-collect-config,os-net-config,os-refresh-config,python-tripleoclient,openstack-tripleo-common*,openstack-tripleo-heat-templates,openstack-tripleo-image-elements,openstack-tripleo,openstack-tripleo-puppet-elements,openstack-puppet-modules,openstack-tripleo-ui,puppet-*
EOF"

#Delorean Deps 저장소 활성화
sudo curl -L -o /etc/yum.repos.d/delorean-deps.repo https://trunk.rdoproject.org/centos7/delorean-deps.repo
```

*Delorean Deps

- 오픈스택 패키지 의존성 관리 도구 / 오픈스택은 복잡한 의존성을 가지고 있으며 이러한 의존성을 관리하여 개발 및 배포 프로세스를 단순화 합니다.

1. 언더클라우드 업그레이드 실행
- 업그레이드 작업을 시작하기 전에 언더클라우드의 상태를 확인
- tripleo [-validations 저장소에는](https://github.com/openstack/tripleo-validations/tree/master/validations) [validations](https://slagle.fedorapeople.org/tripleo-docs/validations/validations.html#running-a-group-of-validations) 의 지침에 따라 "사전 업그레이드" 그룹을 실행하여 실행할 수 있는 몇 가지 '`사전 업그레이드`' 유효성 검사가 있습니다.
    
    ```bash
    openstack workflow execution create tripleo.validations.v1.run_groups '{"group_names": ["pre-upgrade"]}'
    mistral execution-get-output $id_returned_above
    ```
    

```bash
#newton(5.0.0)의 첫 번째 릴리스에서는 undercloud telemetry 서비스가 기본적으로 비활성화되어 있습니다.
#mitaka에서 newton으로 업그레이드하는 동안 텔레메트리 서비스를 유지하려면 운영자가 언더클라우드 업그레이드를 실행하기 전에 이를 명시적으로 활성화해야 합니다.
#undercloud.conf 구성 파일의 [DEFAULT] 섹션에 있습니다.
enable_telemetry  =  true
```

다음 명령은 언더클라우드를 업그레이드 합니다.

```bash
sudo systemctl stop openstack-*
sudo systemctl stop neutron-*
sudo systemctl stop httpd
sudo yum -y update instack-undercloud openstack-puppet-modules openstack-tripleo-common python-tripleoclient
openstack undercloud upgrade
```