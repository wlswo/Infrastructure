# OpenStack Service : Neutron

상태: Done
태그: network

---

# Neutron

> 가상 네트워킹 인프라(Virtual Network Infra)에 대한 모든 네트워킹 측면과 OpenStack 환경에서 물리 네트워킹 인프라(Pysical Network Infra)의 접근 레이어 측면에서 관리합니다. 

뉴트론은 네트워크 가상화 기술을 지원합니다. 이를 통해서 사용자는 서로 다른 가상화 기술을 사용하여 네트워크를 구성할 수 있습니다.
> 

주요 기능 

- VM과 같은 인스턴드들 간의 네트워크 구성
- 라우팅
- IP주소 할당
- 로드 밸런싱과 같은 네트워크 기능을 제공

주요 네트워크 가상화 기술

- Linux bridge
- Open vSwitch
- Linux netfiliter/iptables
- VLAN
- GRE
- VXLAN

# Newtron Server

![스크린샷 2023-05-09 오후 3.38.54.png](OpenStack%20Service%20Neutron%20e57a7b7c4b1d4e04a594f36fc040484a/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-05-09_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.38.54.png)

- 사용자는 Neutron API를 이용하여 Neutron 서버로 IP할당을 요청합니다.
- Neutron 서버는 들어온 요청을 Queue로 다시 요청합니다
- Queue는 Newtron 에이전트와 플러그인으로 IP 할당 지시를 내립니다.
- Neutron 에이전트와 플러그인은 지시받은 작업을 데이터베이스에 저장합니다.
- Neutron 에이전트는 네트워크 프로바이더에게 작업을 지시합니다.
    - 상태를 데이터베이스에 업데이트합니다.
- 할당되 IP를 인스턴스에서 사용할 수 있습니다.