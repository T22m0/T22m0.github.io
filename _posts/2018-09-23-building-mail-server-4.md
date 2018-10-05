---
layout: post
title: Building mail server - 4 (Using Email in gmail)
description: postfix, dovecot, letsencypt를 이용한 메일서버 구축 마지막 단계입니다. 
category: MailServer
date: 2018-09-23
---

드디어 마지막 단계입니다!

구축한 메일서버를 이용해서 실제로 메일을 보내보도록 하겠습니다@@@



현재 메일서버는 웹 메일은 구현되어있지 않고 오직 SMTP와 IMAP&POP3 로 구성되어있어 

이메일을 사용하기 위해서는 별도의 이메일 클라이언트가 필요합니다. 

저는 메일 클라이언트로 썬더버드라는 메일 클라이언트를 다운로드 받았습니다



![1](/assets/images/2018-09-21-building-mail-server-4/1.png)



[Thundirbird]: https://www.thunderbird.net	"Thundirbird"

위의 링크에서 클라이언트를 받고 설치해 줍니다. 



![2](/assets/images/2018-09-21-building-mail-server-4/2.png)

보통 처음 썬더버를 설치한 이후에는 메일 계정을 추가하는 창이 뜨지만,

혹시 창이 뜨지않거나 취소하신 분들은 위의 사진처럼 `계정 설정` -> `메일` 을 눌러서 메일을 추가해 줍니다





![4](/assets/images/2018-09-21-building-mail-server-4/4.png)

![5](/assets/images/2018-09-21-building-mail-server-4/5.png)

메일 구축 1단계에서 생성하였던 계정 정보를 입력하고 계속을 누르면 썬더버드가 메일 서버를 찾습니다.

계속해서 완료 버튼을 누르면 메일 계정 추가가 완료 됩니다. 



![6](/assets/images/2018-09-21-building-mail-server-4/6.png)

짜잔~ 위 사진처럼 성공적으로 메일 계정이 추가되면 쓰기버튼을 눌러서 메일을 작성해 보겠습니다



![7](/assets/images/2018-09-21-building-mail-server-4/7.png)

간단하게 메일 작성후 메일을 보냅니다. 





![8](/assets/images/2018-09-21-building-mail-server-4/8.png)

잠시후 메일이 도착함을 확인할 수 있습니다! 물론 답장을 하게되면 썬더버드를 통해서 확인 하실 수 있습니다. 



이상으로 메일 구축을 완료하였습니다!

따라오시느라 고생 많으셨고 질문이나 수정할 부분있으면

 linux@mast3r.com이나 pawn@teem0.com으로 메일 보내 주시면 감사하겠습니다!

감사합니다~~~~~~!!!!!


