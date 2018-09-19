---
layout: post
title: HTTP Injection(Get)
description: bWAPP의 HTTP Injection(Get)문제의 low, medium 문제 풀이입니다. 
Category: webhacking
date: 2018-08-13
---

이번에 풀어볼 문제는 bWAPP의 A1-Injection Section의 제일 첫번째 문제인 HTML Injection -REflected(GET) 문제입니다!!


![image1](/assets/images/2018-08-13-html_injection_get/1.png)
문제풀이를 위해 기본페이지에서 위 사진과 같이 로그인을 한다.
(ID:bee PW:bug)

밑에 보이는 난이도는 기본으로 low로 설정되어 있으니 바꾸지 말고 로그인한다. 

![image2](/assets/images/2018-08-13-html_injection_get/2.png)
로그인 후에는 위와같이 어떤 취약점을 공략할지 리스트가 나오게 되는데, 제일 처음에 보이는 `HTML Injection - REflected(GET)`을 선택하고 밑의 HACK버튼을 클릭한다. 

![image3](/assets/images/2018-08-13-html_injection_get/3.png)
①에서 `First name:`에 hello, `Last name:`에 world를 입력하고 `GO`버튼을 누르면 아래에 **Welcome hello world**, 즉 사용자가 입력한 결과를 출력한다. 

②를 확인해보면 사용자가 입력했던 값들이 URL로 넘어가는 것으로 GO를 누르면 HTTP GET방식으로 서버에 요청을 보낸다는 것을 확인 할 수 있다. 


만약 `GO` 버튼이 사용자가 입력한 값을 모두 출력한다면, 입력칸에 HTML TAG를 사용한다면 어떻게 될까? 

![image4](/assets/images/2018-08-13-html_injection_get/4.png)

위 사진같이과 같이 현재 LOW레벨에서는 사용자 입력값에 대한 sanitizing이 적용되지 않아서 사용자가 입력한 값을 모두 그대로 출력하게 된다!

사용자 입력값이 완벽한 HTML TAG이기 때문에 출력되는 값은 HTML TAG가 적용된 TEXT가 나오게 된다!

![image5](/assets/images/2018-08-13-html_injection_get/5.png)

우측 상단의 `security level`을 Medium으로 바꾸고 똑같이 LOW에서 했던 것 같이 HTML TAG를 입력해보면 이번에는 HTML TAG가 적용되지 않고 사용자 입력 String이 그대로 출력되게 된다.

왜 그런것일까...?

Medium 레벨에서는 입력 값에 대해서 HTML관련 Character가 있으면 해당 문자를 Escaping처리를 하는 함수가 있다는 것을 추측 할 수 있다.
> 예를들어 `<` 가 입력되면 sanitizing기능은 이 문자를 `%3C`로 변환 시킨다.

그렇다면 어떻게 해야 이 Sanitizing기능을 우회할 수 있을까?

에초에 Sanitizing에 걸리지 않는 문자들로 HTML TAG를 Encoding하게된다면? 예를들어 원래는 `<`를 입력해야하지만 `%3C`로 변환하여 입력한다면? `%3C`는 HTML Character가 포함되어있지 않기 때문에 그 문자 그대로 출력 되게 된다. 

![image6](/assets/images/2018-08-13-html_injection_get/6.png)
위 사진은 *Fiddler*라는 툴을 이용하여 `<h1>hello</h1>`를 Encoding 한 사진이다. 위의 결과 값을 복사하여 `First name`칸에 복사를 하게되면 

아래 사진과 같이 sanitizing을 우회하여 HTML TAG를 적용 시켰음을 확인할 수 있다. 
![image7](/assets/images/2018-08-13-html_injection_get/7.png)
