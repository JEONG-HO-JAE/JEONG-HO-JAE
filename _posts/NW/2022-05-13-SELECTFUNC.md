---
title: Usage of select function
excerpt: IO multiplexing

toc: true
toc_label: About
toc_sticky: true

categories: nw

date: 2022-05-13
last_modified_at: 2022-05-18
---
# IO Multiplexing
멀티플랙싱이란 하나의 통신 채널을 동해서 다수의 사용자들에게 데이터를 전송하는 기술이다.
![Header](/assets/images/multiplex.jpg)<br>
그림에서 볼 수 있듯이 서버에 select 함수 관련 프로세스가 구현되어있다면, 하나의 서버버를 통해 다수의 클라이언트들과 통신할 수 있는 장점이 있다. 이번 포스트에서는 이를 가가능케 하는 select 함수에 대해 알아보자.<br><br>

# Select function
select 함수는 file descriptor를 이용하여 multiplexing을 가능케 한다. 0, 1, 2 (기본 file descriptor)를 제외한 3부터 file descriptor를 연결된 소켓들에게 차례로 부여하고, 해당 file descriptor에 변화가 있는지를 관찰한다. 예를 들어 4번 소켓이 서버에게 데이터를 송신하면 서버의 4번 소켓의 file descriptor는 변화가 생기고 select 함수는 이를 감지할 수 있게 되는 것이다. 코더는 select 함수가 file descriptor를 관찰하는 시간 주기와 file descriptor의 범위를 지정해 줄 수 있다.<br><br>

## fd_set mecro
- void FD_ZERO(fd_set *fdset);<br>
  fdset의 모든 비트를 0으로 설정하는 함수이다. fdset의 변수를 초기화해주는 함수로 select함수를 사용하기 위한 첫 단계라고 할 수 있다. <br>
  **관련 코드**
  fdset rFDS; //변수 설정
  FD_ZERO(&rFDS); //변수 초기화
  <br><br>

- void FD_SET(int fd, fd_set *fdset);<br>
fdset 중에서 fd의 비트를 1로 설정하는 함수이다. 만약 서버에 접속하고자하는 클라이언트가 있다면 해당 함수를 통해서 클라이언트에게 file descriptor를 할당하면 된다.<br>
**관련 코드**
FD_SET(socket_client, &rFDS);
<br><br>

- void FD_ISSET(int fd, fd_set *fdset);<br>
fdset에서 fd에 해당하는 비트가 1인지 아닌지를 판별해주는 함수이다. fd가 1로 설정되어있으면 양수를 반환하는 함수인데, 이는 select함수가 호출되고 나서 어떤 file descriptor에 변화가 생겼는지를 확인하기 위한 함수로 사용된다.<br>
**관련 코드**
// select()<br>
for (int i= 1; i<= max_socket; i++){<br>
  if(FD_ISSET(i, &reads))<br>
}<br>
<br><br>

- void FD_CLR(int fd, fd_set *fdset);<br>
fd로 전달된 file descriptor를 fdset에서 삭제하는 함수로, 서버에서 클라이언트의 접속이 끊기면 해당 함수로 통해서 끊긴 클라이언트의 file descriptor를 소멸하는 용도로 사용된다.<br><br>

## timeval structure
select함수가 사용자가 정한 시간동안 file descriptor를 모니터링하는데 사용되는 구조체이다. <br>
```c
struct timeval timeout;
timeout.tv_sec = 0; //seconds
teimout.tv_usec = 100000;//microseconds
```
# socket programming with select()  - FLOW in server code
![Header](/assets/images/selectflow.jpg)<br>

# Basic Chatting app using SELECT function
## tcp_server.c (server code)
```c
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <netdb.h>
#include <unistd.h>
#include <errno.h>
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define ISVALIDSOCKET(s) ((s) >= 0)
#define CLOSESOCKET(s) close(s)
#define SOCKET int
#define GETSOCKETERRNO() (errno)


int main() {

    struct addrinfo hints;
    memset(&hints, 0, sizeof(hints));
    hints.ai_family = AF_INET;
    hints.ai_socktype = SOCK_STREAM;
    hints.ai_flags = AI_PASSIVE;

    struct addrinfo *bind_address;
    getaddrinfo(0, "8080", &hints, &bind_address);


    SOCKET socket_listen; // define the server socket
    socket_listen = socket(bind_address->ai_family,
            bind_address->ai_socktype, bind_address->ai_protocol);  //assigne the server socket
    if (!ISVALIDSOCKET(socket_listen)) {
        fprintf(stderr, "socket() failed. (%d)\n", GETSOCKETERRNO());
        return 1;
    }

    if (bind(socket_listen,
                bind_address->ai_addr, bind_address->ai_addrlen)) {
        fprintf(stderr, "bind() failed. (%d)\n", GETSOCKETERRNO());
        return 1;
    }  // bind with the clients (IP: 127.0.0.1 Port number: 8080)
       // if not bind with clients -> return 1
    freeaddrinfo(bind_address);


    if (listen(socket_listen, 10) < 0) {
        fprintf(stderr, "listen() failed. (%d)\n", GETSOCKETERRNO());
        return 1;
    }

    fd_set master;  // file descriptor set for monitoring
    FD_ZERO(&master);  // set all of file descriptors 0
    FD_SET(socket_listen, &master); // addt the socket_listen file descriptor
    SOCKET max_socket = socket_listen;

    printf("Waiting for connections...\n");


    while(1) {
        fd_set reads;
        reads = master;

        if (select(max_socket+1, &reads, 0, 0, 0) < 0) {
            fprintf(stderr, "select() failed. (%d)\n", GETSOCKETERRNO());
            return 1;
        }

        SOCKET i;
        for(i = 1; i <= max_socket; ++i) {
            if (FD_ISSET(i, &reads)) {
                if (i == socket_listen) {
                    struct sockaddr_storage client_address;
                    socklen_t client_len = sizeof(client_address);
                    SOCKET socket_client = accept(socket_listen,
                            (struct sockaddr*) &client_address,
                            &client_len);

                    if (!ISVALIDSOCKET(socket_client)) {
                        fprintf(stderr, "accept() failed. (%d)\n",
                                GETSOCKETERRNO());
                        return 1;
                    }

                    FD_SET(socket_client, &master);

                    if (socket_client > max_socket)
                        max_socket = socket_client;

		    printf("Succesfully connected with client %d \n", socket_client);

		    char temp [2];
		    sprintf(temp, "%d", socket_client);
		    send(socket_client, temp, 2, 0);  //send client number to client
		    // number is client's file descriptor in server

		    for (int j = 4; j <= max_socket; j++) {
          if ( j == socket_client ) continue;
          else{
                        char sys[10] = "server";
                        send(j, sys, 10, 0);

                        char text [4096] = "client";
                        char temp [2];
                        sprintf (temp, "%d", socket_client);
                        strcat(text, temp);
                        char temp2 [50] = " has successfully entered the chat room\n";
                        strcat(text, temp2);
                        send(j, text, 4096, 0);
                      }
                   } //send success messsages to other clients.
		}

                 else {
                    char read[4096];
                    int bytes_received = recv(i, read, 4096, 0);

                    if (bytes_received < 1) {
			printf("Client%d disconnected!\n", i);

                        FD_CLR(i, &master);
                        CLOSESOCKET(i);

			for (int j = 1; j <= max_socket; j++) {
				if( j == socket_listen || j == i) continue;
				else {
					char sys[10] = "server";
					char text[4096] = "client";
					char temp [2];
					sprintf(temp, "%d", i);
					strcat(text, temp);
					char text1[30] = " disconnected!\n";
					strcat(text, text1);
					send(j , sys, 10, 0);
					send(j, text, 4096, 0);
				}
			}
			continue;
                      } // send disconnected messages to others clients

		    for(int j = 1; j <= max_socket; j++) {
			if(FD_ISSET(j, &master)) {
			   if(j == socket_listen || j == i) continue;
			   else {
				char name [10] = "client";
				char temp [2];
				sprintf(temp, "%d", i);
				strcat(name, temp);
				send(j, name, 10, 0);
				send(j, read, bytes_received, 0);
			   }
			}
                     }// send messages to other client what the server recieved.
		}// else
            } //if FD_ISSET
        } //for i to max_socket
    } //while(1)

    printf("Closing listening socket...\n");
    CLOSESOCKET(socket_listen);

    printf("Finished.\n");

    return 0;
}
```

## tcp_client.c (client code)
```c
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <netdb.h>
#include <unistd.h>
#include <errno.h>
#include <stdio.h>
#include <string.h>
#include <time.h>

#define ISVALIDSOCKET(s) ((s) >= 0)
#define CLOSESOCKET(s) close(s)
#define SOCKET int
#define GETSOCKETERRNO() (errno)

int main(int argc, char *argv[]) {
    char name [10] = "client";
    int isFirstRecv = 1;

    // variable for print date and time.
    time_t timer;
    struct tm *t;
    timer = time(NULL);
    t = localtime(&timer);


    if (argc < 3) {
        fprintf(stderr, "usage: tcp_client hostname port\n");
        return 1;
    }

    struct addrinfo hints;
    memset(&hints, 0, sizeof(hints));
    hints.ai_socktype = SOCK_STREAM;
    struct addrinfo *peer_address;
    if (getaddrinfo(argv[1], argv[2], &hints, &peer_address)) {
        fprintf(stderr, "getaddrinfo() failed. (%d)\n", GETSOCKETERRNO());
        return 1;
    }


    SOCKET socket_peer;
    socket_peer = socket(peer_address->ai_family,
            peer_address->ai_socktype, peer_address->ai_protocol);
    if (!ISVALIDSOCKET(socket_peer)) {
        fprintf(stderr, "socket() failed. (%d)\n", GETSOCKETERRNO());
        return 1;
    }


    if (connect(socket_peer,
                peer_address->ai_addr, peer_address->ai_addrlen)) {
        fprintf(stderr, "connect() failed. (%d)\n", GETSOCKETERRNO());
        return 1;
    }
    freeaddrinfo(peer_address);

    while(1) {
	fd_set reads;
        FD_ZERO(&reads);
        FD_SET(socket_peer, &reads);
        FD_SET(0, &reads);


        struct timeval timeout;
        timeout.tv_sec = 0;
        timeout.tv_usec = 100000;


        if (select(socket_peer+1, &reads, 0, 0, &timeout) < 0) {
            fprintf(stderr, "select() failed. (%d)\n", GETSOCKETERRNO());
            return 1;
        }

        if (FD_ISSET(socket_peer, &reads)) {

	    if( isFirstRecv ) {
		char num [2];
		recv(socket_peer, num, 2, 0);
		printf("Connected with server! \n");
		strcat(name, num);
   	 	printf("Your name is %s \n", name);
    		printf("*****To send data, enter text followed by enter.*****\n");
		isFirstRecv = 0;
	    }
	    else {
		printf("[ %d.%d.%d. %d:%d:%d ] ", t->tm_year, t->tm_mon, t->tm_mday, t->tm_hour, t->tm_min, t->tm_sec);
		char others[10];
		recv(socket_peer, others, 10, 0);
		printf("%s : ", others);
		char read[4096];
            	int bytes_received = recv(socket_peer, read, 4096, 0);
            	if (bytes_received < 1) {
                    printf("Connection closed by peer.\n");
                    break;
                }
		printf("%.*s", bytes_received, read);
	    }		
        }

        if(FD_ISSET(0, &reads)) {
            char read[4096];
            if (!fgets(read, 4096, stdin)) break;
	    if (strcmp(read, "END\n") == 0) {
		printf("Closing socket...\n");
		CLOSESOCKET(socket_peer);
		break;
	    }

	    printf("[ %d.%d.%d. %d:%d:%d ] %s : %s", t->tm_year, t->tm_mon, t->tm_mday, t->tm_hour, t->tm_min, t->tm_sec, name, read);
            send(socket_peer, read, strlen(read), 0);
        }

    } //end while(1)
}

```
