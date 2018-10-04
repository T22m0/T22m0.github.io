---
layout: post
title: Building mail server - 3 (Dovecot)
description: postfix, dovecot, letsencypt를 이용한 메일서버 구축 세번째 단계입니다. 
category: MailServer
date: 2018-09-22
---



이번 포스팅에서는 Dovecot을 설치하고 설정하도록 하겠습니다. 



![1](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/1.png)

```bash
apt install dovecot-core dovecot-imapd dovecot-pop3d
```

제일 먼저 apt명령어로 Dovecot과 imap, pop3 데몬을 다운받아 줍니다. 





![2](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/2.png)

```bash
vim /etc/dovecot/conf.d/10-mast3r.conf
```

다운로드가 완료되었으면 위의 파일을 수정할 것입니다. 



![3](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/3.png)

`unix_listener`라는 항목을 몇번 지나다 보면 `/var/spool/postfix/private/auth`라는 섹션이 나오는데 그곳에서

```yml
user = postfix
group = postfix
```

이 두 항목을 추가시켜줍니다. 





![4](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/4.png)

```bash
vim /etc/dovecot/conf.d/10-auth.conf
```

다음은 10-auth.conf 파일입니다. 인증방법을 설정하는 파일입니다. 



![5](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/5.png)

`disable_plaintext_auth`는 평문인증을 비활성화 하는 옵션입니다. 



![6](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/6.png)

`auth_mechanisms` 옵션을 찾아 뒤에 `login`옵션을 추가해 줍니다. 이를 추가해줌으로써 IMAP에서 로그인을 할 수 있게 됩니다. 



![7-1](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/7-1.png)

```bash
vim /etc/dovecot/dovecot.conf
```

Dovecot의 메인 설정파일입니다. 



![7-2](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/7-2.png)

```bash
protocols = pop3 imap
```

위 문장을 파일 제일 아래쪽에 적어주도록 합시다. 



![8](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/8.png)

```bash
vim /etc/dovecot/conf.d/10-mail.conf
```

다음은 메일을 설정하는 파일입니다. 



![9](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/9.png)

```bash
mail_location = maildir:~/Maildir
```

Postfix설정에서도 설정 해 줬듯이, Dovecot에서도 mail저장에 mbox대신에 /Maildir을 사용합니다. 



![10](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/10.png)

```bash
vim /etc/dovecot/conf.d/20-pop3.conf
```

다음은 pop3를 설정하는 파일입니다. 



![11](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/11.png)

```bash
pop3_uidl_format = %08Xu%08Xv
```

주석처리되어있는 위 라인의 주석을 해제 시켜줍니다. 



![12](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/12.png)

```bash
vim /etc/dovecot/conf.d/10-ssl.conf
```

마지막으로 SSL 설정파일입니다. 

![13](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/13.png)

위 사진과같이 `ssl`옵션에 yes를 주시고, `ssl_cert`와`ssl_key`의 경로에 저번에 Letsencrypt에서 생성되었던 파일들의 경로를 지정합니다. 



이렇게 지정하면 모든 설정이 완료되었습니다.!

![14](/Users/teemo/Desktop/T22m0.github.io/assets/images/2018-09-21-building-mail-server-3/14.png)

```bash
systemctl restart dovecot
```

마지막으로 dovecot을 재구동 해주면 dovecot설정이 완료됩니다. 



다음 마지막 포스팅에서는 실제로 썬더버드를 사용해서 메일을 주고 받아보겠습니다!

