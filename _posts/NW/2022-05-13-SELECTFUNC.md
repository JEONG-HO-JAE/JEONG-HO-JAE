---
title: Usage of select function
excerpt: IO multiplexing

toc: true
toc_label: About
toc_sticky: true

categories: nw

date: 2022-05-13
last_modified_at: 2022-05-13
---
# IO Multiplexing
멀티플랙싱이란 하나의 통신 채널을 동해서 다수의 사용자들에게 데이터를 전송하는 기술이다.
![Header](/assets/images/multiplex.jpg)<br>
그림에서 볼 수 있듯이 서버에 select 함수 관련 프로세스가 구현되어있다면, 하나의 서버버를 통해 다수의 클라이언트들과 통신할 수 있는 장점이 있다. 이번 포스트에서는 이를 가가능케 하는 select 함수에 대해 알아보자.<br><br>

# Select function
select 함수는 file descriptor를 이용하여 multiplexing을 가능케 한다. 0, 1, 2 (기본 file descriptor)를 제외한 3부터 file descriptor를 연결된 소켓들에게 차례로 부여하고, 해당 file descriptor에 변화가 있는지를 관찰한다. 예를 들어 4번 소켓이 서버에게 데이터를 송신하면 서버의 4번 소켓의 file descriptor는 변화가 생기고 select 함수는 이를 감지할 수 있게 되는 것이다. 코더는 select 함수가 file descriptor를 관찰하는 시간 주기와 file descriptor의 범위를 지정해 줄 수 있다.<br><br>

## fd_set mecro
- void FD_ZERO(fd_set *fdset);
  fdset의 모든 비트를 0으로 설정하는 함수이다. fdset의 변수를 초기화해주는 함수로 select함수를 사용하기 위한 첫 단계라고 할 수 있다. <br>
  **관련 코드**
  fdset rFDS; //변수 설정
  FD_ZERO(&rFDS); //변수 초기화
  <br><br>

- void FD_SET(int fd, fd_set *fdset);
fdset 중에서 fd의 비트를 1로 설정하는 함수이다. 만약 서버에 접속하고자하는 클라이언트가 있다면 해당 함수를 통해서 클라이언트에게 file descriptor를 할당하면 된다.<br>
**관련 코드**
FD_SET(socket_client, &rFDS);
<br><br>

- void FD_ISSET(int fd, fd_set *fdset);
fdset에서 fd에 해당하는 비트가 1인지 아닌지를 판별해주는 함수이다. fd가 1로 설정되어있으면 양수를 반환하는 함수인데, 이는 select함수가 호출되고 나서 어떤 file descriptor에 변화가 생겼는지를 확인하기 위한 함수로 사용된다.<br>
**관련 코드**
// select()<br>
for (int i= 1; i<= max_socket; i++){<br>
  if(FD_ISSET(i, &reads))<br>
}<br>
<br><br>

- void FD_CLR(int fd, fd_set *fdset);
fd로 전달된 file descriptor를 fdset에서 삭제하는 함수로, 서버에서 클라이언트의 접속이 끊기면 해당 함수로 통해서 끊긴 클라이언트의 file descriptor를 소멸하는 용도로 사용된다.<br><br>

## timeval structure
