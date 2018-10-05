---
layout: post
title: 메일 서버 구축 - 2 (Postfix & letsencrypt)
description: postfix, dovecot, letsencypt를 이용한 메일서버 구축 두번째 단계입니다. 
category: MailServer
date: 2018-09-21
---

이번 포스팅에서는 리눅스 머신에 postfix설치 설정 및 letsencrypt를 사용한 SSL 설정을 하겠습니다.

 postfix를 설치하고 설정까지 마치면 SMTP프로토콜(메일발송 프로토콜)을 이용할 수 있습니다.



앞으로의 작업은 모두 라즈베리파이에서 이루어 지는 작업입니다. 

자 그럼 시작하도록 하겠습니다ㅋㅋ



제일 먼저 라즈베리파이에 로그인을 합니다, 

![1](/assets/images/2018-09-21-building-mail-server-2/1.png)

```bash
apt install postfix
```

그 다음 위와 같은 명령어를 입력하여 postfix를 설치해 줍니다. 



![2](/assets/images/2018-09-21-building-mail-server-2/2.png)

![3](/assets/images/2018-09-21-building-mail-server-2/3.png)

최초 설치시에 위와 같은 화면을 보게 되는데, `System mail name:`을 입력하는 곳에 메일을 보낼 도메인(저는 당연히 `mast3r.com`이겠죠?)를 입력해 줍니다. 



![4](/assets/images/2018-09-21-building-mail-server-2/4.png)

```bash
dpkg-reconfigure postfix
```

설치가 완료된 이후에 위의 명령어로 postfix에 대한 설정을 다시 구성하도록 합시다. 



![5](/assets/images/2018-09-21-building-mail-server-2/5.png)

`dpkg-reconfigure postfix`명령어를 입력 하게되면 3번째 옵션부터 설정해야합니다. 

제일먼저 시스템관리자의 계정을 입력하는 옵션입니다. 

저는 `pi`계정으로 시스템을 관리하기 때문에 `pi`를 입력하였습니다. 



![6](/assets/images/2018-09-21-building-mail-server-2/6.png)

다음으로는 도착지 설정입니다. postfix가 최종 목적지로 사용할 도메인명을 적어주시면 됩니다.

저는 `mast3r.com`과	`mail.mast3r.com`을 최종 목적지로 추가 하였습니다. 



![7](/assets/images/2018-09-21-building-mail-server-2/7.png)

![8](/assets/images/2018-09-21-building-mail-server-2/8.png)

![9](/assets/images/2018-09-21-building-mail-server-2/9.png)

![10](/assets/images/2018-09-21-building-mail-server-2/10.png)

![11](/assets/images/2018-09-21-building-mail-server-2/11.png)

나머지 위의 옵션 부분에서는 똑같이 따라하시면 됩니다. 



![13](/assets/images/2018-09-21-building-mail-server-2/13.png)

```bash
postconf -e 'home_mailbox = Maildir/'
```

위의 명령어는 기본 홈 메일박스를 Maildir/로 설정하는 명령어입니다.

(기본적으로 mbox와 Maildir방식이 있는데 Maildir로 설정합니다. 간단하게 설명하면 사용자마다 다른 폴더를 생성한다) 

![14](/assets/images/2018-09-21-building-mail-server-2/14.png)

이후 SMTP에서 SASL을 사용하기 위한 설정들입니다

(SASL은 Simple Authentication and Security Layer의 약자로 SMTP를 사용할 때 인증사용을 위한 설정이라고 간단하게 알아두면 될듯하다.[틀리면 알려주세요!!])



![15](/assets/images/2018-09-21-building-mail-server-2/15.png)

![16](/assets/images/2018-09-21-building-mail-server-2/16.png)

다음은 `master.cf`에서의 주석해제이다. 붉은 네모에 있는 줄들의 주석을 해제해준다. 



![17](/assets/images/2018-09-21-building-mail-server-2/17.png)

다음은 SSL인증을 위한 Letsenctpt설치이다! apache2와 letsencrypt를 설치한다. 

apache2는 letsencrypt에서 인증서를 발급할 때 인증을 위하여 필요하기 때문에 같이 설치 한다. 

두 패키지가 설치 된 이후에 인증을 위해서  `systemctl stop apache2`로 아파치를 중지시켜줘야 한다 





![18](/assets/images/2018-09-21-building-mail-server-2/18.png)

```bash
certbot certonly --agree-tos -email [email] -d [mail.[domail]]
```

위 명령어를 입력하고 나오는 옵션의 2번째 옵션을 선택하여 인증서를 발급받는다.

성공적으로 발급받게 되면 위와같이 `Congratultations!`이라는 메세지가 나오게 된다. 



![19](/assets/images/2018-09-21-building-mail-server-2/19.png)

다음은 인증서 자동 갱신 방법이다. 

```bash
crontab -e 
```

위 명령어로 root에 대한 crontab을 변경해주도록하자. 



![20](/assets/images/2018-09-21-building-mail-server-2/20.png)

crontab의 맨 아래로 가서 위와같이 해당 줄을 추가해주도록 하자. 

간단하게 설명 하자면 매월 1일 01시 00분에 apache2를 종료시키고 03분에 인증서를 갱신시킨다라는 명령어 2줄이다. 



![21](/assets/images/2018-09-21-building-mail-server-2/21.png)

위는 생성된 인증서를 이용해서 SMTP를 설정하는 명령어들이다. 



![22](/assets/images/2018-09-21-building-mail-server-2/22.png)

모든 설정이 완료되었으면 `systemctl restart postfix`라는 명령어를 입력하여 postfix를 재시작하도록 한다. 

