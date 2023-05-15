# OpenStack Service : Heat

상태: Done
태그: orchestration

---

# Heat 개요

> 인프라스트럭쳐를 자동화하고, 각 오픈스택 서비스들을 조율하는 오케스트레이션 컴포넌트입니다. Heat은 인프라스트럭쳐의 프로비저닝, 구성 및 배포를 자동화하기 위해 사용됩니다.
> 

⇒ Heat는 코드로 작성가능한 텍스트 파일 형식인 템플릿을 기반으로 여러 클라우드 애플리케이션을 실행하는 오케스트레이션 엔진입니다.

⇒ 네트워크, 인스턴스, 저장 장치 등과 같은 클라우드 구성 요소 생성을 자동화하는 방법을 제공합니다.

# Heat 구성 요소

![스크린샷 2023-05-15 오전 9.36.25.png](OpenStack%20Service%20Heat%20bae6f78da24e47f2a569d237230ed1ac/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-05-15_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_9.36.25.png)

- heat
    - heat-api와 통신하는 CLI
- heat-API
    - heat-engine과 RPC통신을 통한 사용자의 HOT에 대한 API요청을 보내 그 요청을 처리하도록 하는 REST API
    - *HOT : `Hybrid Orchestration Template`의 약자로 OpenStack Heat에서 사용되는 템플릿 형식입니다. HOT는 YAML형식으로 작성되며, OpenStack 인프라스트럭처의 프로비저닝 및 구성을 자동화하는 데 사용됩니다.
    
    ```yaml
    heat_template_version: 2018-08-31
    
    parameters:
      server_name:
        type: string
        description: Name of the server
    
    resources:
      server:
        type: OS::Nova::Server
        properties:
          name: { get_param: server_name } 
          flavor: m1.small
          image: cirros
          networks:
            - network: private
    
    outputs:
      server_ip:
        value: { get_attr: [server, networks, private] }
    ```
    
- heat-API-cfn
    - Heat CloudFormation API 서비스입니다. AWS CloudFormation 호환 API를 제공하며 AWS CloudFormation과 호환되는 템플릿을 사용하여 인프라를 관리할 수 있습니다.
- heat-api-cloudwatch
    - CloudWatch API로 오케스트레이션 서비스에 대한 모니터링 기능을 제공
- heat-engince
    - 사용자가 정의한 HOT을 기반으로 nova, neutron과 같은 오픈스택 서비스를 통해 시작하고 그로 인해 발생하는 이벤트를 API요청자에게 제공하는 핵심 운영 요소
    

# Heat의 작동 방식

1. 사용자는 작성한 HOT를 Heat API에게 전달
2. API는 AMPQ를 통해 Heat Engine에게 전달
3. Heat Engine은 RPC를 이용하여 오픈스택 서비스에 전달
4. 전달받은 요청들을 각 오픈스택의 서비스에 수행

***AMQP**

- `Advanced Message Queuing Protocol`(고급 메세지 큐잉 프로토콜)의 약자로 분산 시스템 간에 메시지를 효율적으로 교환하기 위한 개방형 표준 프로토콜입니다.
- Heat 에서의 AMQP는 메시지 브로커를 의미하며 주로 컴포넌트 간의 통신을 위해 사용됩니다.