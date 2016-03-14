---
layout: post
title: "NodeBB 플러그인 nodebb-plugin-magicblock"
description: "NodeBB 포럼 플러그인 nodebb-plugin-magicblock 을 개발 중인데 조언부탁드립니다."
category: IT
tags: [IT, NodeBB, Plugin, Nodejs]
---

https://rm-rf.work 이 nodeBB 기반인데, 이런 저런 불편한점을 해결하려다 보니, 생전 처음으로 node.js 를 만져보게 되었습니다. ( 갓 24시간이 안된 따끈따끈한 신참입니다. :) )
html 태그를 허용하지 않는 마크다운 편집이 아무래도 불편해서 워드프레스나 미디어위키처럼 {{ .. }} 매크로 블럭을 쓰는 방법을 도입해보려고 nodebb 플러긴을 하나 만들었는데요, 자바스크립트는 거의 써본적이 없어서 ( jquery 로 뭘 만들어 본 적은 이건 뭐 자바스크립트 경험이라고 하기에는... ) 어제 처음 여기저기 뒤져서 만들기는 했는태 태생이 C++, Perl 이라, 언어만 자바스크립트지 코드는 C,Perl 스타일이네요.

여튼, 그런 이유로, 제대로 만든 건지, 어떤 보안문제가 있는지, 더 쉽게 만들 수 있는 방법, 자바스크립트/nodejs 스타일, 좋은 라이브러리가 있는지는 영 모르겠습니다.

관심 있으신 분이 계시면, 보시고 조언해 주시면 감사하겠습니다. :) 

플러그인에 대한 아이디어나 문법등은 Readme 파일에 있는 것 처럼 NodeBB 포럼 에서 논의중입니다.( 라고 말하지만 사실 별 관심 없는 듯 합니다. )

https://community.nodebb.org/topic/8098

아랫 부분은 NodeBB 커뮤니티 포럼에 올린 글입니다.

-----------------------------------------------

Hi everyone, 

After I felt that it's so difficult to write formatted text ( color, css class ...) with  a sanitized markdown, I have started writing a new plugin called __Magic Block__.  :)

A basic idea is a using of `{{` `}}` block ( like mediawiki or wordpress ) with parameters like
* `{{.classname#color#BGcolor text of body }}`
  * `{{.myClass body }}` will be `<span class="myClass">body<span>`
  * `{{#red body}}` will be `<span style="color:red;"> body <span>`
  * `{{#red#000 body}}` will be `<span style="color:red;background-color:#000">body</span>`
  * and also all toghter `{{.classA.classB.classC#red#yello body}}` will act in same manner.
* If you put a link as a ``body``, everything will be same as above but
  * `{{.myclass http://example.com}}` will be `<a href="http://example.com" class="myclass">http://example.com</a>`
  * Also the magic block supports `{{#red [link](http://example.com)}}` in same way.
* if you just put just a link to the magic block, then real magic will be there so 
  * `{{ http://example.com/any.jpg }}` will display a image ( currently with iFramely which means all iframely supported link will work )

Actually I have done until here, and further plan is  **Macros** like
* `{{macroName(parameter)#red text of body}}` can do any special.
* Also this can provide a simple but configurable macro action as a text expander via an admin interface

Right now, codes are messy and no document, no configuration interface but some basic functionalities have been done.

What do you think about these functionality and syntax? 
Are those too messy or conflicting with Markdown philosophy?  
Or do you have any better idea or any comments?

* This is screenshot what current version of MagicBlock can do. ( iframely objects are hided by clicks )
![MagicBlock](http://i.imgur.com/omoH6VM.png)
