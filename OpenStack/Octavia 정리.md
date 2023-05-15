# OpenStack : Octavia

상태: Done
태그: load Balance

---

# Octavia 개요

> OpenStack 생태계 에서 사용하는 로드 밸런싱 솔루션입니다.
> 

# Octavia History

> 오픈스택에서 제공하는 로드밸런서는 초기 Neutron 로드밸런서 v1으로 시작하여 Neutron 로드밸런서 v2로 업데이트 된 뒤에 일정 기간의 업데이트를 거쳐 Queens부터 지원이 종료되어 Train버전부터는 완전히 종료됩니다.

이는 Neutron의 규모가 커졌을 뿐만 아니라 별도의 프로젝트인 Octavia가 존재하기 때문입니다. Train이 릴리즈 된 현재 시점에서는 `오픈스택 = Octavia`라고 생각해도 무방 할 것 같습니다.
> 

# Octavia Architectures

## 로드밸런싱 구조

![스크린샷 2023-05-15 오전 10.34.19.png](OpenStack%20Octavia%20f1203ea055024efba55cd0da4eeb4b27/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-05-15_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_10.34.19.png)

- 기존 Neutron 로드밸런서는 Neutron 자체의 프로세스로 존재하였습니다.
- Octavia에서는 Amphora라고 불리는 로드밸러서를 인스턴스 형태로 배포합니다.
    - 해당 인스턴스를 생성하는데 필요한 이미지와 네트워크 등은 기본으로 생성되어 있습니다.

### 동작방식

로드밸런서는 Listener를 통해 특정 포트를 수신합니다.

Pool에서 사용하는 로드밸런싱 방식(ex. 라운드로빈)에 따라 Pool에 속한 Member들에게 데이터를 전달합니다.

Health Monitor는 이름 그대로 Member의 상태 및 동작을 확인합니다. 

Pool에 속한 2개의 Member중 하나가 죽어 있다면 데이터는 살아있는 Member에게 전달됩니다.

## Octavia 구조

![스크린샷 2023-05-15 오전 10.39.46.png](OpenStack%20Octavia%20f1203ea055024efba55cd0da4eeb4b27/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-05-15_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_10.39.46.png)

Octavia의 구조는 크게 **Amphora**, **Controller(Octavia Server)**, **Network**로 나뉘게 됩니다.

**Amphora** 

- Amphora는 가상 머신, 컨테이너 등으로 구성된 로드밸런서입니다. 인스턴스이기 때문에 운영체제를 가지고 있습니다.(Ubuntu) 이들 내부에는 HAProxy가 동작하면서 Listener를 통해 적합한 Pool에 데이터를 전달합니다. Amphora는 여러 개로 구성될 수 있습니다.

**Controller** 

- Octavia 동작의 전반적인 관리를 담당합니다. 특히 Driver의 경우 Nova에게 요청하여 Amphora 인스턴스를 생성하고,  Amphora에 접근하기 위한 네트워크를 요청하는 등 여러 프로젝트 및 Amphora에게 명령을 내리기 위한 Driver들이 구성 포함 되어 있습니다.
    - Health Manager : Amphora의 상태를 점검
    - Controller Worker : API처리를 담당
    - Housekeeping Manager : 데이터베이스 및 인증서 관리를 담당

**Network**

- Amphora는 Octavia의 설정 파일에 등록 된 네트워크 서브넷을 통해 연결됩니다. 
이에 따라 Amphora에 명령을 내릴 수 있는 접근 가능한 네트워크 서브넷이 필요합니다. Octavia의 구성 파일에서 이를 변경해주는 것이 필요합니다.