---
title: "Lowendspirit - 정말 싸게 쓸 수 있는 저사양 VPS 서버"
category: "IT"
tags: [IT, vps, 서버, Shell ]
---

VPS 관련 정보를 너무 많이 올리는게 아닌가 싶지만,, 또 **쉘** 하면 **리눅스 서버** 아니겠습니까, 하하

그래서 정말 싸게( 일년에 3,4천원 ) 쓸 수 있는 저사양 VPS 서버 Lowendspirit 을 소개합니다. ( 이건 리퍼럴이나 추천인  같은 건 없습니다. :) )

정말 저사양이라서 ( 램이 128M :sweat: ) 뭐에 쓰겠냐 싶지만, 의외로 이걸로 웹서버를 돌리는 분들도 있고 ( 그야말로 삽질), VPN 으로 사용하거나, ssh 게이트웨이 로 쓰시는 분들도 있습니다.

또 리눅스 쉘을 공부할 목적으로도 쓸 수 있겠죠.

어쨌건 1년에 $3 정도에 불과한 가격이라, 저도 예전에 미국에 하나 두고 VPN 용으로 썼습니다. Static 파일 배포용으로 일본에도 하나 만들었는데, 지금은 쓰지는 않고 있네요, ( 혹시 관심있으신 분 있으면 빌려드릴 수도 있어요. )

전 세계에 프로바이더들이 있고 ( 각각 다른 회사입니다. ), 지역과 사양은 다음과 같습니다.

* http://lowendspirit.com/locations.html

![lowendspirit 가격표](http://i.imgur.com/8kBvyPM.jpg)

주의하실점은,, 

* Support 없음( 대신 커뮤니티 서포트 )
* IPv4 공인 아이피 없음 ( IPv4 NAT 을 위한 ssh 접속과 IPv6 를 지원합니다. )
  * IPv6 로 웹서버를 만들고 싶다면, Cloudflare 를 이용하면 IPv4 로 서비스 할 수 있습니다. (ssh,ftp 같은건 여전히 nat 이나 IPv6를 이용해야합니다. )
  * 그 밖의 주의사항을 미리 숙지할 필요가 있습니다. http://lowendspirit.com/faq.html
