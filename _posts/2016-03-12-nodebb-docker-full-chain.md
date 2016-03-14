---
layout: post
title: "NodeBB 를 위한 도커 체인 ( Nginx, Redis, Nodejs, Mongodb)."
description: "NodeBB 를 위한 도커 체인 ( Nginx, Redis, Nodejs)"
category: IT
tags: [IT, shell, docker, utils, jq]
---

하루 종일 공부해서 NodeBB 를 위한 도커체인을 만들었습니다. 이거 생각보다 재밌네요.

가볍기로 소문난 Alpine 리눅스르 기반으로 docker-compose 를 이용해서 각각 빌드 했고,

개발/가벼운 서비스를 목적으로 하기때문에 대부분의 설정파일과 NodeBB 폴더는 호스트 폴더에 집중시킨후 도커로 볼륨 마운트를 했습니다. 

첨엔, docker-compose 가 맘에 안들어서 직접 스크립트를  한참 짜다 보니,,, 왠만해선 docker-compose 보다 잘하려면 노력이 너무 많이 필요하고, 거꾸로 생각해서 docker-compose 에 wraper 나 추가 스크립트를 추가하는게 훨씬 이익이라는 걸 깨달았어요.

Vi가 맘에 안들어서 Vi로 새 에디터를 만들다가 Vi 를 그냥 쓰게 되었다는 옛 교훈이 새삼스레 떠오릅니다. :)

https://github.com/qgp9/nodebb-docker-dev 
