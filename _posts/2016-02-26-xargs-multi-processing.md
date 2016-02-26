---
title: "xargs 와 간단한 쉘 기반 멀티프로세싱"
category: "IT"
tags: [IT, Shell, xargs, 배치, multi]
---

xargs 는 다양한 용도로 사용할 수 있는데 특히 멀티 프로세싱 큐 기능에 대해서는 의외로 모르는 분들이 많으셔서, 간단히 소개해보도록 하겠습니다. 

xargs 를 이용한 멀티 프로세싱의 핵심은 "-P" 옵션입니다. 
먼저 간단하고 실용적인  예제를 만들어 보겠습니다.


## 웹페이지 다운받기 1

다음처럼 `input.txt` 파일에 다음처럼 다운 받을 URL을 한줄에 하나씩 넣습니다.


```
cat << ENDL > input.txt
google.com
http://qgp9.github.io/blog/2015/07/11/Free-Servers
http://qgp9.github.io/blog/2015/07/09/John-8-7
http://qgp9.github.io/blog/2015/05/30/Windstruck
http://qgp9.github.io/blog/2015/05/06/Craig-Remark-01
ENDL

```

그리고 `xargs` 에 옵션 `-P4` 를 주어서 4개의 멀티 프로세싱 큐를 만들어줍니다.

```
cat input.txt | xargs -I% -P4 wget -s "%" 
```

짜잔~ , 이제 `input.txt` 의 URL 들은 동시에 4개씩 다운받아집니다. 아마 원래의 순서와 출력 순서가 봐뀌어서 출력된 것이 보일겁니다.

이게 끝입니다. 참 간단하죠?


## 웹페이지 다운받기 2

이제 약간만 응용해 볼까요? 이번엔 제가 실제로 사용했던 예를 들어보죠.
창조과학회(창조과학소설회)의 글을 반박하기 위한 데이터베이스를 만들기 위해 사전작업으로 웹페이지를 다운받아봅시다.
다행히 글들이 일련번호 순으로 되어있어 간단하네요

다운로드는 한 10개 정도 큐를 만들어봅시다. ( 어차피 네트웤이 병목이라 늘려도 한계가 있고, 또 너무 늘리면 방화벽이 차단할 가능성도 있습니다. )

```
seq 1 100 | xargs -I% -P10 wget -q "http://www.kacr.or.kr/library/itemview.asp?no=%" -O %.html
```

오~ 28초, 큐가 하나인 경우( `-P1` ) 보다 딱 10배 빠르군요.

하지만 진행상황이 나오지 않으니 답답하죠? `wget` 에 진행중인 URL 을 출력하는 옵션이 있겠지만 간단히 `echo` 로 해결해봅시다. `xargs`에 간단한 명령어 조합을 사용할땐 `bash -c` 가 유용합니다.

    seq 10 20 | xargs -I% -P1 bash -c "echo %;wget -q 'http://www.kacr.or.kr/library/itemview.asp?no=%' -O %.html"

물론 제가 진짜로 사용했던 스크립트는 더 복잡하지만, 이 정도도 자주 씁니다.


## 핑 테스트
이번엔 관리하는 서버들의 상태를 알아보기 위해 `192.168.1.100 ~ 192.168.1.199` 까지 ping 을 보낸다고 생각해봅시다.
일단 `ping`의 결과를 아이피별 파일로 만들어보겠습니다. 큐는 10개, 아이피별로 5번씩 핑을 보냅니다. 사실 `ping` 은 한 100개 짜리 큐를 만들어서 동시에 보내도 됩니다. 자신의 머신과 네트웤 상황에 맞게 테스트해보시고 조절하세요.

    seq 100 199 | xargs -I% -P10 bash -c 'ping -c5 192.168.1.% > %.txt'

일일이 파일로 만들고 다시 처리하려면 귀찮으니까 한번에 처리해봅시다. perl 을 이용하면 편한데, 여기선 좀더 간단한 명령어를 사용해 보겠습니다.

일반적인 ping 의 결과는 다음과 같습니다.

```
PING 192.168.1.103 (192.168.1.103): 56 data bytes
64 bytes from 192.168.1.103: icmp_seq=0 ttl=64 time=0.103 ms
64 bytes from 192.168.1.103: icmp_seq=1 ttl=64 time=0.119 ms
64 bytes from 192.168.1.103: icmp_seq=2 ttl=64 time=0.109 ms
64 bytes from 192.168.1.103: icmp_seq=3 ttl=64 time=0.080 ms
64 bytes from 192.168.1.103: icmp_seq=4 ttl=64 time=0.128 ms

--- 192.168.1.103 ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 0.080/0.108/0.128/0.016 ms
```

사실 우린 끝의 3줄만 알면 됩니다. `grep -A2 statistics` 를 이용해서 `--- 192.168.1.103 ping statistics ---` 이후 2줄만 출력한후 `xargs` 를 이용해서 한줄로 합치겠습니다. ( `xargs` 가 또 나오네요. 이것도 유용한 트릭입니다. )

    seq 100 199 | xargs -I% -P10 bash -c 'ping -c5 192.168.1.% | grep -A2 statistics | xargs ' | tee result.txt

결과는 다음처럼 나옵니다. 

```
--- 192.168.1.103 ping statistics --- 5 packets transmitted, 5 packets received, 0.0% packet loss round-trip min/avg/max/stddev = 0.085/0.282/0.436/0.130 ms
--- 192.168.1.100 ping statistics --- 5 packets transmitted, 5 packets received, 0.0% packet loss round-trip min/avg/max/stddev = 6.855/56.180/109.259/37.225 ms
--- 192.168.1.104 ping statistics --- 5 packets transmitted, 0 packets received, 100.0% packet loss
--- 192.168.1.105 ping statistics --- 5 packets transmitted, 0 packets received, 100.0% packet loss
--- 192.168.1.106 ping statistics --- 5 packets transmitted, 0 packets received, 100.0% packet loss
--- 192.168.1.101 ping statistics --- 5 packets transmitted, 0 packets received, 100.0% packet loss
```

살아있는 머신이 2개 밖에 없군요. -_-;;;; 정확히 말하자면 원래 머신이  2개입니다. :)

## 웹페이지 다운받기 3

다시 웹페이지 다운받기 2로 돌아가봅시다. 창조소설회의 글들을 다운받아야 하는데,, 6000 개를 받다가 실수로 중간에 멈춰버렸습니다. 다시 첨부터 받아야 할까요?

간단한 확인 코드를 넣어봅시다.

    seq 10 200 | xargs -I% -P10 bash -c "test -f %.html && exit; echo %; wget -q 'http://www.kacr.or.kr/library/itemview.asp?no=%' -O %.html"

이제 한번 받은 파일은 다시 받지 않고 넘어가니, 언제라도 중지했다가 다시 시작할 수 있습니다.
받을 파일이 6000 개라는 점을 생각해 보면 perl 등을 이용해 더 빠르게 처리할 수도 있습니다.

    seq 1 6000 | perl -nle' -f "$_.html" or print' | xargs -I% -P10 bash -c "echo %; wget -q 'http://www.kacr.or.kr/library/itemview.asp?no=%' -O %.html"

하지만 명령문이 점점 길어지니 관리하기가 힘들군요. 더군다나 `bash -c` 를 쓰면 명령문을 인용부호 안에 넣어야 한다는 제한이 있습니다.

이럴 경우, 해결책은 간단하죠. 스크립트 파일을 만드는 겁니다.

```
cat << 'ENDL' > down_one_page.sh
#!/bin/bash
if [[ -f "$1.html" ]];then
  exit;
fi
echo $1;
wget -q "http://www.kacr.or.kr/library/itemview.asp?no=$1" -O "$1.html"
ENDL

chmod +x down_one_page.sh

seq 1001 2000 | xargs -I% -P10 ./down_one_page.sh %
seq 2001 3000 | xargs -I% -P10 ./down_one_page.sh %
```


끝~ 차암 쉽죠잉~


주의사항 : 이 문서의 코드와 명령어를 사용할 경우 컴퓨터의 폭발이나 서버의 다운 등에 대한 책임은 사용자에게 있음을 공지합니다. :)

