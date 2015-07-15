---
title: 무료로 사용할 수 있는 서버 ( IaaS, VPS )
date: 2015-07-11 18:37:30
category: IT
tags: [ 무료, free, 서버, 클라우드, IaaS, VPS]
#thumbnailImage: WICC_bannervideoimage.png
---
<center>![AWS banner]({{ site.baseurl }}/images/WICC_bannervideoimage.png)</center>
IT 관련 공부를 하다보면 테스트용 서버가 필요해지는데, 무료로 장기간 혹은 단기간 사용할 수 있고 OS 레벨에서 풀 컨트롤이 가능한 서버들(IaaS,VPS)들을 정리해 보았습니다.

* [6 개월 이상](./#6_개월_이상)
* [2 개월 이상](./#2_개월_이상)
* [1 개월 이상](./#1_개월_이상)


## 6 개월 이상
### <img style="display:inline;" src="http://aws.amazon.com/favicon.ico"> [Amazon Web Service - AWS](http://aws.amazon.com/ko/free/)
  * 12 개월간 
  * t2.micro 인트턴스<span style="color:red">만 가능</span> 월 750시간. Window 가능 
    메모리 1G, 1CPU(느림), Disk:최대30GB, Bandwidth:월15GB( CDN 25GB )
  * 데이터 송수신 가격과 인스턴스 가격이 비싸서, 요금 폭탄을 맞을 수 있으니 조심해야합니다.

### <img style="display:inline;" src="https://www.digitalocean.com/favicon.ico">[Digital Ocean ( DO )](https://www.digitalocean.com/?refcode=39d8cf51683d)
  * 가입시 10$ 제공, 신용카드 없이 페이팔만으로 가능
  * 필리핀 서버 가능 ( 미국.유럽.아시아 )
  * [깃헙 학생팩](https://education.github.com/pack) 에서 학생인증 받으면 100$ 쿠폰을 줍니다. 
    * 소멸기간 없음 ( 10개월간 1GB VPS 사용가능 ), DO 에 먼저 가입한 후에도 쿠폰을 쓸 수 있습니다.


  | RAM | CPU | Disk | Bandwidth | per month |
  |-----|-----|------|-----------|-|
  | 512M | 1 | 20GB SSD | 1TB | $5 |
  | 1GB | 1 | 30GB SSD | 2TB | $10 |
  | 2GB | 2 | 40GB SSD | 3TB | $20 |
  | 4GB | 2 | 60GB SSD | 4TB | $40 |

### [Hyper-V Mart](http://www.hyper-v-mart.com/)
  * 1GB 1CPU 6 개월간 무료 ( Window Hyper-V, 네트웤이 느림 5Mbps ). 한국에서 안될 수도...

## 2 개월 이상
### <img style="display:inline;" src="https://console.developers.google.com/favicon.ico">[Google Cloud Platform](https://console.developers.google.com/freetrial?hl=ko)
  * 60일간 300$, 대충 한달에 CPU 6~7 개 정도를 쓸 수 있는 양


### <img style="display:inline;" src="http://www.hpcloud.com/sites/all/themes/hpcloud/img/hp-logo-print.png"> [HP Cloud](http://www.hpcloud.com/cloud-credit)
  * 3개월간 300$ CPU 4개정도 사용할 수 있는 양 

### <img style="display:inline;" src="https://www.vultr.com/favicon.ico"> [Vultr](https://www.vultr.com/freetrial/?ref=6839789)
  * <span class="red">60 일간 50$ 무료제공</span>
  * 프로모션이 계속 있는 것이 아니라서, 제일 먼저 신청한다면 Vultr을 권합니다. 
  * 신용카드나 직불카드를 연결 필요
  * 일본 서버 가능( 미국.유럽.아시아 )


  | RAM | CPU | Disk | Bandwidth | per month |
  |-----|-----|------|-----------|-|
  | 768M | 1 | 15GB SSD | 1TB | $5 |
  | 1GB | 1 | 20GB SSD | 2TB | $8 |
  | 2GB | 2 | 45GB SSD | 3TB | $16 |
  | 4GB | 2 | 90GB SSD | 4TB | $32 |

## 1 개월 이상
### [Microsoft Azure](https://azure.microsoft.com/ko-kr/pricing/free-trial/)
  * 1 개월 150$


## 결론
AWS 와 DO 은 안정적이면서 장기간 솔루션이 될 수 있고, Google 은 짧은 시간동안 고사양 머신을 쓸 수 있다는 장점이 있습니다.
당장 필요하고 뭘로 시작해야 할지 모르겠다면 프로모션 중인 Vultr 로 일단 시작한 후에 필요에 따라 다른 것들을 고려해보는 것이 가장 효율적일 것 같습니다.

사실 free 서버를 찾아 헤메거나 관리하는 것도 무척 소모적인 일이라서 Vultr 이나 DO 같은 중저가 솔루션에 적당한 가격을 지불하는게 더 좋습니다.
다음엔 무료 PaaS 들과, 저가형 VPS 도 소개해 보겠습니다.
