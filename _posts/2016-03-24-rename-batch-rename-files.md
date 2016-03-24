---
layout: post
title: "rename : 커맨드라인에서 정규표현식으로 여러 파일의 이름을 바꾸기"
date: "2016-03-24 07:45:09 +0200"
category: "IT"
tags: [ IT, Shell, Command, Tip, 정규표현식, Perl ]
---
아는 사람은 알고 모르는 사람은 모르는 명령어 `rename` 을 소개합니다.

## 소개
`rename` 은 대부분의 리눅스 배포판에 이미 설치되어 있는 여러 파일의 이름을 정규표현식으로 관리할 수 있는 간단하고 오래된(낡은 것이 아니라 믿을 수 있는 :) ) `perl`로 만들어진 명령어 입니다. 텍스트 스크립트라서 열어보면 `perl` 코드를 직접 읽을 수도 있죠.

## 간단한 사용법
우선 `rename` 이 설치되어 있나 확인해 봅시다.
```
$ rename
Usage: rename [-v] [-n] [-f] perlexpr [filenames]
```
`man rename` 으로 메뉴얼을 읽어보는 것도 좋겠죠.

위처럼 `rename` 이 설치되어 있다면 간단한 작업을 먼저 해보겠습니다.. `-n` 옵션은 `dry run` 즉 실제로 실행하지는 않고, 어떻게 실행할 것인지만 보여주기 때문에 실제 파일이름을 바꾸기 전에, 확인/연습용으로 좋습니다. `-v` 는 `Vervose`, 실행 과정을 자세히 보여준다는 뜻입니다.

먼저 연습용 디렉토리에 연습용 파일을 만들어보도록 하겠습니다.
```
mkdir try_rename && cd try_rename
seq -f"1%02.0f" 1 3 | xargs -I% touch "my_%_abc-abc.txt"
```
이제 `try_rename` 디렉토리에 `my_101-abc-abc.txt`, `my_102-abc-abc.txt`, `my_103-abc-abc.txt`의 파일이 생겼습니다.

### abc 를 def 로
우선 abc 를 def 로 바꾸어 볼까요? 연습을 위해서 `-vn` 옵션을 쓰겠습니다.
정규표현식 치환의 문법은 `s/찾을문자열/바꿀문자열/` 입니다.
```
$ rename -vn 's/abc/def/' *
my_101-abc-abc.txt renamed as my_101-def-abc.txt
my_102-abc-abc.txt renamed as my_102-def-abc.txt
my_103-abc-abc.txt renamed as my_103-def-abc.txt
```
잘 바뀌었는데, 뒷부분은 안바뀌었습니다. 이럴때는 반복지시자 `g`를 쓰면 됩니다.
```
$ rename -vn 's/abc/def/g' *
my_101-abc-abc.txt renamed as my_101-def-def.txt
my_102-abc-abc.txt renamed as my_102-def-def.txt
my_103-abc-abc.txt renamed as my_103-def-def.txt
```
이제 `-n` 만 빼면 파일 이름이 실제로 바뀌게 됩니다.

### 확장자 바꾸기
확장자를 바꾸려면 문장의 끝을 의미하는 `$` 를 쓰면 됩니다.
```
$rename -vn 's/\.txt$/.text/' *
my_101-abc-abc.txt renamed as my_101-abc-abc.text
my_102-abc-abc.txt renamed as my_102-abc-abc.text
my_103-abc-abc.txt renamed as my_103-abc-abc.text
```
정규표현식을 이용한 검색에서 `.` 은 "아무 문자/숫자/기호/빈칸" 이라는 특수한 의미를 가지고 있기 때문에 역슬래쉬 `\`를 붙여야 문자그대로의 `.` 을 찾을 수 있습니다.

t 로 시작하는 모든 확장자를 바꾸려면 알파벳,숫자,`_` 를 의미하는 `\w` 과 앞문자의 0개이상의 반복을 의미하는 `*`를 사용합니다.
```
$ rename -vn 's/\.t\w*$/.text/' *
my_101-abc-abc.txt renamed as my_101-abc-abc.text
my_102-abc-abc.txt renamed as my_102-abc-abc.text
my_103-abc-abc.txt renamed as my_103-abc-abc.text
```

### 숫자를 모두 없애려면?
* `\d` : 임의의 한글자의 숫자
* `+`  : 앞 검색어의 1회 이상의 반복
```
$ rename -vn 's/\d+//g' *
my_101-abc-abc.txt renamed as my_-abc-abc.txt
...
```

### 숫자에 2를 곱하려면?
* `/e` : `s/찾을문자열/바꿀문자열코드/e` 에서 `바꿀문자열코드` perl 코드로 실행한 뒤 결과값으로 바꾼다
* `(문자열)` : 여러번 사용할 수 있고 찾은 문자열을   `$1`,`$2`,... 에 저장한다.
```
$ rename -vn 's/(\d+)/$1*2/ge' *
my_101-abc-abc.txt renamed as my_202-abc-abc.txt
...
```
### 다른 사용법이 궁금하면?
댓글 남겨주시면 추가하겠습니다. :)
