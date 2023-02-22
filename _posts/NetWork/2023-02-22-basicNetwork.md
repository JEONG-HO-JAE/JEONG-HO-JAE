---
title: Basic Concept of Network
excerpt: LAN / WAN / Switch / Router 

toc: true
toc_label: About
toc_sticky: true

categories: network

date: 2023-02-22
last_modified_at: 2023-02-22
---
<br>
# Basic Concept

## 네트워크 규모
- LAN (근거리 통신망)
  - 회사, 학교 그리고 가정과 같이 지리적으로 제한된 곳에서 컴퓨터, 프린터 등을 연결할 수 있는 네트워크
  - 외부와 차단하고 내부에서만 사용가능한 네트워크
<br>

- WAN (광대역 통신망)
  - 넓은 범위에 구축된 네트워크
  - Internet service provider (인터넷 서비스 제공자) 가 제공하는 서비스를 사용하여 구축된 네트워크
  - LAN과 LAN을 연결하는 개념
<br>

![image](\assets\images\LANWAN.png)
<br>

## Switch
Data link 계층에서 작동하는 네트워크 기기로 물리적 포트에 기기들을 꽂아 사용한다. 포트에 연결된 기기들는 스위치를 통해 통신이 가능하다. Data link계층에서 통신이 이루어지기 때문에 MAC 주소를 사용하여 패켓을 전달한다.
스위치에 연결된 기기들은 전이중 기능을 통해 네트워크 트래픽 간의 충돌을 피할 수 있다. 서로 무전기가 아닌 휴대전화처럼 통신이 가능하다.
<br>

## Hub
스위치와 마찬가지로 허브를 통해 네트워크 기기들은 통신이 가능하다. 다만 송신측이 데이터를 발송하면 모든 기기들이 해당 데이터를 다 받아야만 하는 현상이 발생한다. (브로드캐스팅) 지정된 기기에게만 전해지는 스위치와 달리 허브는 모두에게 통신을 하게되므로 효율이 떨어진다.
<br>

## Router
Network link 계층에서 작동하는 네트워크 기기로 IP주소를 기반으로 작동한다. 라우터의 주 기능은 **패킷의 위치를 추출하여, 그 위치에 대한 최적의 경로를 지정하여 패킷을 다음 장치로 보내는 것이다.** 또한 라우터의 대표적인 기능은 **NAT, Firewall, VPN** 등이 있다.