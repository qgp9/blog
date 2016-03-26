---
layout: post
title: "SSH Proxy 와 점프호스트 ( 경유서버를 이용한 목적서버 연결 )"
category: "IT"
tags: [ IT, Shell, ssh, proxy, jump host ]
---
## 오늘의 TIP~  `SSH Proxy` 를 이용한 점프 호스트~

SSH 프록시/터널링은 여러가지 이유로 사용합니다.

1. 간이 VPN 이 필요할 때
2. 여러 안전하지 않은 통신 프로토콜을 안전하게 연결하고 싶을때
3. 서버가 접근 IP 를 제한하거나 방화벽/공유기 뒤에 있어서 직접 연결이 불가능 할때
4. 사용하는 데스크탑/노트북이 퍼블릭 아이피를 가지고 있지 않을 때 임시로 서비스를 운용하고 싶을때.
5. 다른 서버를 이용해서 최종 서버에 ssh 연결하고 싶을때
6. 다른 프록시나 tor 망을 경유해서 접속하고 싶을때 ( 이건 엄밀히 말하면 ssh 프록시는 아니지만, ssh 가 프록시를 이용하는거죠 )
7. 기타등등

이 글에서는 5번에 해당하는 방법들을 알아보겠습니다. 다른 것들은 요청이 있으면 나중에 하나씩 살펴보기로 하고요.

보통 5번을 점프호스트(Jump Host) 라고 부릅니다.( 검색용 힌트 :) ) 이 경우에도 다양한 세부 경우가 있을 수 있는데, 예를 들어
1. 목적 서버가 접근 IP 를 제한할때
2. 목적 서버가 방화벽/공유기 뒤에 있을때
3. IPv4 전용 네트웤/머신에서 IPv6 전용 서버로 접속하고 싶을때

이 경우들은 종류가 다르지만 결국 경유서버를 이용한다면 같은 문제로 분류할 수 있어 해결책도 같습니다.

경유서버는 `server1`, 목적서버는 `server2` 라고 부르겠습니다. 물론 `server1` 은 클라이언트에서 ssh 로 접속할 수 있고 `server1` 은 `server2`에 네트웤 접근이 가능해야 합니다.

## 가장 쉬운 방법 : Pseudo Terminal

가장 쉬운 방법은 Pesudo Terminal 을 이용하는 겁니다.

```
ssh -tt server1 ssh server2
```
간단하죠?

이 명령은 ssh 로 `server1` 에 접속한 후 `server1` 에서 로긴쉘이 아니라 `ssh server2` 를 실행하는 겁니다. 따라서 사실 다음의 두 명령을 한번에 내린 것과 비슷합니다.

```
[client]$ ssh server1
[server1]$ ssh server2
```

`-tt` ssh로 로긴쉘을 실행하지 않고 명시적으로 명령어를 실행할 때 명령어가 터미널을 필요로 할 경우 Pseudo terminal 을 사용할 것을 알려줘야하기 때문에 필요한 옵션입니다.

Pseudo Terminal 을 이용한 이 방법의 가장 큰 단점은 server2 에 대한 인증키등의 설정이 `server1` 의 설정에 의존한다는 것입니다. 인증키 문제는 `ssh-agent`로 해결할 수도 있기는 하지만 여전히 로컬의 .ssh/config 를 사용할 수 없다는 단점이 있습니다.

## 복잡하지만 범용적인 방법
```
ssh -fqN -L2222:server2:22 server1
ssh localhost -p 2222
```
이건 전형적인 로컬 포트 터널링을 이용하는 방법입니다.
첫번째 ssh 프로세스를 직접 관리해야 하고 명령이 지저분한 단점이 있지만, 대부분의 경우에 가장 잘 통하는 장점이 있죠.

하지만, 제가 가장 좋아하는 다른 아름다운 방법이 있기 때문에 꼭 필요한 경우에만 쓰면 됩니다.

## 추천하는 방법 - ProxyCommand
ssh 의 프록시 옵션인 `ProxyCommand` 과 ssh 내장 `netcat proxy` 를 사용하는 방법입니다.

```
ssh -o ProxyCommand="ssh -W %h:%p server1" server2
```

이 명령을 이용하면 server1 이 server2 의 ssh 포트로 netcat 링크를 만들어주고 그것을 이용해 server2 에 접속하게 됩니다. 여기서 `%h` 는 `server2`, `%p`는 `server2` 의 ssh 포트( 지정하지 않을 경우 22번 ) 을 의미합니다.

이 명령 자체로도 깔끔하고 훌륭하지만 진정한 장점은 로컬 `~/.ssh/config` 파일로 설정이 가능하다는 것입니다. 예를 들어 다음과 같이 설정하면

```
Host s2
HostName server2
User user2
IdentityFile ~/.ssh/server2_id_rsa
ProxyCommand ssh -W server2:22 server1
```

이제 간단히 s2 에 접속할 수 있게 됩니다.

```
ssh s2
```

혹은

```
ssh user2@s2
```

아름답죠?

아 물론, scp나 rsync 등 ssh 를 이용하는 대부분의 명령어에서 잘 작동합니다.

오늘은 여기까지!!

## 부록 - netcat 을 이용한 ProxyCommand

ssh 에 `-W` 옵션이 생긴 것이 그리 오래되지 않아서, 그 전에는 `netcat` 의 `nc` 명령을 이용했습니다. 이것도 알아두면 여러모로 응용이 가능합니다.

```
ssh -o ProxyCommand="ssh server1 nc server2 22" server2
```

진짜 끝!
