# OpenStack Service : Glance

상태: Done
태그: image

---

# Glance 개요

> Glance는 하이퍼바이저에서 사용할 수 있는 다양한 가상머신 이미지를 관리하고 설치된 운영체제를 보관 및 관리합니다.
> 

# Glance Logical Architecture

- 사용자들은 glance-api를 통해 이미지를 등록 삭제 관리할 수 있습니다.
- glacne-api는 glance-registry와 glance database에서 이미지가 관리됩니다.
- 이미지를 등록할 때는 glance-registry를 통해 glance-database에 저장이 됩니다.
- 등록된 이미지를 사용할 땐 glance-database에 사용요청을 합니다.
    
    

![스크린샷 2023-05-09 오후 1.48.40.png](OpenStack%20Service%20Glance%205209afdda78c4cc89627c1b7d3d3044f/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-05-09_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_1.48.40.png)

- Glance DB에는 다양한 OS 이미지들을 보관 및 관리합니다.
- glance-API 와 glance-registry 를 이용해서 glance DB에 있는 이미지를 사용합니다.

![스크린샷 2023-05-09 오후 1.54.10.png](OpenStack%20Service%20Glance%205209afdda78c4cc89627c1b7d3d3044f/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-05-09_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_1.54.10.png)