---
layout: post
title: "간단하게 만들어본 유틸리티용 도커입니다."
description: "간단하게 만들어본 유틸리티용 도커입니다."
category: IT
tags: [IT, shell, docker, utils, jq]
---

# 간단하게 만들어본 유틸리티용 도커입니다.

도커 운용 시스템에 여러 유틸이 설치되어 있지 않을 가능성을 생각해서 간단한 유틸리티 도커를 이용하는 방법입니다. 

간이용이라서, 아직 복잡한 건 못합니다. 현재 디렉토리 이하의 파일을 다루는 정도죠.
NodeBB 설치단계에서 JSON 파일을 다루기 위해 만든거라 `jq` 만 설치합니다.

물론 `jq`를  NodeBB 도커에 설치하고 쓰는 방법도 있지만, . 여러 용도로 쓸수 있을 것 같아서 왠지 이렇게 해보고 싶었습니다.

다음 처럼 사용합니다.
```bash
./utils jq a.json b.json > c.json   
```

최신 코드는 이곳에  
https://github.com/qgp9/nodebb-docker-dev/blob/master/bin/utils

```bash
#!/bin/bash
CMD=${1:-"/bin/bash"} && shift
IMAGENAME=qgp9_utils

## Orignal script file if this is symbolic link
oScript=$( echo $BASH_SOURC | perl -MCwd+abs_path -nlE'$_=abs_path$n while $n=readlink $_;say abs_path $_' )
## Get Date of qgp9_utils images
imageDateForm=$(docker inspect -f '{{.Created}}' $IMAGENAME)
export imageDate=$( test -n "$imageDateForm" && date -d "$imageDateForm" +%s )
export scriptDate=$(date -r "$oScript" +%s);
if perl -e'$i=$ENV{imageDate}||0;$f=$ENV{scriptDate}||0;exit ($i<$f?0:1)' ;then
  TMPDIR=$(mktemp -d);

  cat > $TMPDIR/Dockerfile << EOFF
  FROM alpine
  MAINTAINER qgp9 

  # BASE SET
  RUN apk update && apk upgrade && apk update
  RUN apk add --update bash && rm -rf /var/cache/apk/*  

  # ADDITIONAL SET

  # UTILS
  RUN apk add --update jq perl && rm -rf /var/cache/apk/*  

  RUN mkdir /data
  WORKDIR /data
  RUN touch created-${scriptDate}.log


  CMD ["/bin/bash"]
EOFF

echo "(RE)BUILD a Docker image : $IMAGENAME"
  docker build -t qgp9_utils $TMPDIR  2>&1 > $TMPDIR/build.log || cat $TMPDIR/build.log

fi

docker run -it --rm -v $PWD:/data $IMAGENAME $CMD $@
```
