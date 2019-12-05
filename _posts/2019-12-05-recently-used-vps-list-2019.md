---
layout: post
title: "최근 사용중인 VPS 리스트와 벤치마크"
category: "IT"
tags: [ VPS, Server, Cloud, Free ]
---

어제 유명 VPS 제공업체들에 대한 벤치마크를 하나 찾아서 공유합니다.
가격이 저렴한 VPS 들은 자체 성능도 중요하지만 모두 어느정도라도 OverSell 을 하기 때문에 실제 사용에는 운에 따른 편차가 있습니다.
같은 가격에 성능이 좋을 수록, 이웃에 따라 그 성능이 보장되기는 힘들겠죠.

## 벤치마크 
* [벤치마크 링크](https://community.centminmod.com/threads/13-way-vps-server-benchmark-comparison-tests-upcloud-vs-digitalocean-vs-linode-vs-vultr-vs-hetzner.17742/)

위 링크는 centminmod 라는 유명한 Centos 기반 LEMP(Linux, Nginx, MySQL, PHP) 관리 툴 커뮤니티에서 테스트한 내용이네요.
[Upcloud], [DigitalOcean], [Linode], [Vultr], [Hetzner] 5개 업체를 비교했습니다.

자세한 내용은 링크에 있으니 하이라이트와 결론만 얘기하자면

![]({{ site.baseurl }}/images/8108_vps-benchmarks-05-geekbench.png)

[Upcloud] / [Vultr] 이 단연 선두이고 [Hetzner] 은 저렴한 가격에도 불구하고 빠르네요

하지만 앞서 말했듯이, 벤치는 보통 짧은 시간의 테스트 결과라 안정성은 다른 문제이기는 합니다.


## 최근 사용중인 VPS

한국과 관련되어서 VPS 를 사용할땐 주로 지역 문제가 걸리다 보니 [Vultr]이나 [Lightsail], [Conoha] 등을 사용했었는데, 
요즘은 개발/취미용으로만 사용하다보니 유럽에 있는 서버를 사용중입니다.

주로 사용하는 순위로 소개하자면 (장단점은 개인적인 이유가 포함되었습니다.)

### [Upcloud]
* 장점: 핀란드에 서버가 있음!!!!(주로 미국/유럽지역 서버). 속도가 빠름
* 단점: 아시아쪽 서버가 별로 없음.

### [Oracle Cloud Compute]
* 장점1: 무려 2개의 1G 메모리 VPS 를 무료로 제공합니다. (Free tier 가 아니라 쭉~)
* 장점2: AWS 와 다르게 기본 네트웍 용량이 좋아서(2TiB 인가..), 요금 폭탄 걱정이 적습니다. 
* 단점: AWS Free-tier 도 그렇지만, CPU 는 약합니다.

### [Vultr]
* 장점: 지금은 큰 상관 없지만 일본에 서버가 있어서 한국 서비스용으로 가끔 씁니다. 속도는 다른 VPS 에 비해 빠른편
* 뭣보다 크레딧이 남아있어서 가끔 씁니다. ㅎㅎㅎ

### [DigitalOcean]
* 장점: DO 의 가장 큰 장점은, 제일 유명하기도하지만 무엇보다 Droplet과 풍부한 문서입니다. 뭘 하나 테스트하고 싶은데 노력을 기울이기 귀찮을때 주로 사용합니다.
* 단점: 위 벤치마크에 있듯이 개중에 느린 편.

### [Hetzner]
* 여긴 아직 사용해 보지 않았는데 저렴한 가격과 빠른 속도, 핀란드 서버 때문에 조만간 사용해볼 계획입니다. 가격은 많이 저렴합니다.
* 단점은 서버가 독일과 핀란드밖에 없어요.

## 끝.
아, 이 글의 링크 중 상당수는 제 리퍼럴 링크입니다. :-)

[Vultr]: https://www.vultr.com/?ref=7858657-4F
[Lightsail]: https://aws.amazon.com/ko/lightsail/
[Upcloud]: https://upcloud.com/signup/?promo=3M58BB
[Oracle Cloud Compute]: https://www.oracle.com/cloud/free/#always-free
[DigitalOcean]: https://m.do.co/c/39d8cf51683d
[Hetzner]: https://www.hetzner.com/
[Linode]: https://www.linode.com/?r=6f7cb83593e0f6e3e090915d7966e5bebbb5b5b6
[Conoha]: https://www.conoha.jp/referral/?token=WRozgp0nWTIpRITFDvMw9Ziy7335arMLOWzEkhIEEvI6i3DewrU-1CR
