---
title: HyperVisor
excerpt: Basic Concept

toc: true
toc_label: About
toc_sticky: true

categories: network

date: 2023-03-03
last_modified_at: 2023-03-03
---
<br>

# Hyper Visor
- Concept<br>
Hyper Visor는 virtual machine을 생성하고 실행하는 소프트웨어이다.<br>
리소스를 가상으로 공유하여 여러 개의 가상머신들의 메모리 관리나 프로세스 처리를 가능케 한다.<br>
<br>

- Host & Guest<br>
하이퍼바이저로 사용되는 물리 하드웨어를 **Host**라고 하며, 리소스를 사용하는 VM을 **Guest**라고 한다.<br>
<br>

- Overview<br>
Hyper Visor에서 VM을 실행하려면 메모리 관리, 스케줄러, I/O 처리, 드라이버 관리, 보안 등과 같은 운영체제 수준의 요건이 요구된다.<br>
Hyper Visor는 VM 구성에 따라 리소스를 제공하고, VM들의 요청에 따라 프로세스를 처리한다.<br>
즉, 가상화를 통해 서로 다른 운영체제를 동시에 구동할 수 있게 된다.<br>
<br>

## Type
### Bare Metal Server
- Concept<br>
![image](\assets\images\type1.png)<br><br>
호스트의 하드웨어에서 하이퍼 바이저가 직접 구동되어 게스트 운영체제를 관리한다.<br>
하이퍼바이저가 OS위에서 구동되는 것이 아니라 하드웨어 위에서 구동되기 때문에 성능이 뛰어나고 보안이 좋다.<br>
<br>

  - 전가상화<br>
  ![image](\assets\images\전가상화.png)<br><br>
  하드웨어 전체를 가상화하는 방식<br>
  Guest들은 자신들이 가상화 위에서 작동되고 있는지 알 수 없음<br>
  Guest - 물리 자원 접근 불가<br>
  <br>

  - 반가상화<br>
  ![image](\assets\images\반가상화.png)<br><br>
  Guest들의 프로세스를 하드웨어에 전달하는 가상화 방식<br>
  Guest들은 자신들이 하이퍼 바이저 위에 가상화되어있다는 것을 인지하고 있음<br>
  <br>
  <br>

### Host Hyper Visor
- Concept<br>
![image](\assets\images\type2.png)<br><br>
일방적으로 개인이나 가정에서 쓰는 가상화 방식<br>
Host OS위에 하이퍼 바이저가 실행되고 그 위에 Guest가 작동되는 방식<br>
<br>
