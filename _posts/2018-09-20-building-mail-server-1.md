---
layout: post
title: Building mail server - 1 (setting)
description: postfix, dovecot, letsencypt를 이용한 메일서버 구축 첫번째 단계입니다. 
category: MailServer
date: 2018-09-20

---

안녕하세요 여러분~
이번에는 라즈베리파이와 도메인을 이용해서 나만의 메일버서를 구축해보려고 합니다. 
구축전에 준비물은, `라즈베리파이`,`자신이 등록한 도메인`,`DDNS와 포트포워딩을 지원하는 공유기`,입니다. (저는 RPI3에 Raspbian, 도메인은 mast3r.com, 공유기는 IPtime A3004NS로 진행합니다) 

생각보다 준비물이 많네요...;;;

포스팅은 총 4단계로 나누어서 진행하구요, 이 글애는 메일서버구축을 위한 사전준비를 포스팅 하려고 합니다.
최대한 쉽게 따라오실 수 있게 노력했으니 도움이 됐으면 좋겠습니다.!

자 이제, 시작해보겠습니다. 

공유기에 라즈베리파이를 물리고 전원을 켜줍니다. 

켜준 뒤 첫번째로 세팅할 곳은 공유기입니다. 


![1](/assets/images/2018-09-20-building-mail-server-1/1.png)

제일 처음 공유기에 로그인을 해주시고!
고급설정 -> 네트워크 관리 -> 내부 네트워크 설정에 들어갑니다. 
들어가시면 현재 라즈베리파이가 잘 연결되어 있으며  할당되어 있는 IP는 192.168.0.6인걸 확인 할 수 있습니다. 


![2](/assets/images/2018-09-20-building-mail-server-1/2.png)

다음으로는 포트포워딩 설정입니다. 
고급 설정 -> NAT/라우터 관리 -> 포트포워드 설정에 들어갑니다. 
안에 들어가시면 위 사진과같이 설정이 되어있어야 합니다.
설정 하는 방법은 간단한데 밑에 보이는 `규칙이름`에 임의의 이름을 넣고 `내부 IP주소`, 곧 아까 확인한 `192.168.0.6`를 입력하면 됩니다. 그리고 `외부포트`와 `내부포트`는 각각 사진의 있는 값을 따라서 적으시면 됩니다.

2번의 ssh2룰은 외부에서 내부로 도메인을 사용해 ssh로 접속할때 쓰기위한 룰입니다
3번룰은 2단계에서 Letsencrypt를 사용해서 인증서를 만들때 웹서버 인증이 필요하기 때문에 필요한 룰입니다. 
4,5번룰은 SMTP에서 사용하는 포트를 포워딩 해주는 룰입니다.
6~9번까지의 룰은 IMAP/POP3에서 사용하는 포트들을 포워딩 해주는 룰입니다. 


![3](/assets/images/2018-09-20-building-mail-server-1/3.png)

포워딩을 마친 뒤에 고급 설정 -> 특수기능 -> DDNS설정이라고 써진 메뉴를 들어갑니다. 
이 페이지 붉은색으로 네모쳐진 곳에 두가지 값을 입력해야하는데, 
호스트 이름은 DDNS의 이름을 입력하는 칸이고 사용자 ID는 사용하시는 이메일 주소를 넣으시면 됩니다. 
호스트이름에 넣으시는 이름이 곧 외부에서 내부로 접속할때 사용되는 DDNS이름이 됩니다!
2칸을 모두 채워주시고 생성버튼을 누르면 위 사진과 같이 DDNS가 등록되게 됩니다. 

이렇게 따라와 주셨으면 공유기 설정은 모두 마쳤습니다! 

* * *

이제 도메인을 세팅할 차례입니다. 


![4](/assets/images/2018-09-20-building-mail-server-1/4.png)

저는 *hosting.kr*에서 도메인을 구입했습니다.
*hosting.kr*의 서브도메인 설정화면은 위 사진과 같으며,
총 3개의 서브도메인이 보이지만... `www`도메인은 제 웹서버 도메인이므로..생략해도 됩니다 ㅋㅋ
Apex도메인에는 A레코드로 아까 만든 DDNS에 대한 IP, (cmd에서 ping [ddns 주소]하면 실제 IP를 확인할 수 있습니다.)
`mail`서브도메인에서는 `CNAME`으로 아까 생성했던 DDNS 주소를 입력하고 적용합니다. 

여기까지 오셨으면 도메인 세팅도 끝났습니다!.

***

이제 라즈베리파이를 세팅할 차례입니다. 


![5](/assets/images/2018-09-20-building-mail-server-1/5.png)

저는 putty를 사용하여 제 rpi에 접속하였습니다. 
접속시 실제 도메인을 사용하여 로그인하였습니다. `Host Name`에는 도메인이름을 `Port`에는 아까 공유기에서 포트포워딩해준 포트 60001번을 넣고 Open을 눌러 접속합니다.


![6](/assets/images/2018-09-20-building-mail-server-1/6.png)

그리고 pi/raspberry로 로그인을 합니다. (이 계정과 비번은 기본 세팅이니 꼮 변경해주시기 바랍니다!!!!변경필쑤!!)


![7](/assets/images/2018-09-20-building-mail-server-1/7.png)

로그인을 한 뒤 아래 명령어를 입력 해 루트계정으로 작업을 실행합니다(매번 sudo를 치기 귀찮기 때문이죠...후훗)

```bash
sudo su
```
참고로 현재 상태에서는 `root`계정에 대해 비밀번호가 걸려있지 않기 때문에 반드시 비밀번호를 설정해주시기 바랍니다!!! 제발요.. 꼭 해주세요...


![8](/assets/images/2018-09-20-building-mail-server-1/8.png)

`root`계정으로 접근한 뒤, 아래의 명령어를 입력하여 시스템을 최신버전으로 업데이트 해줍니다. 
```bash
apt update; apt upgrade -y
```


![9](/assets/images/2018-09-20-building-mail-server-1/9.png)

이 단계는 옵션입니다. 저는 개인적으로 VIM을 선호하기때문에 라즈베리파이에 설치해 줍니다. 

```bash
apt install vim 
```


![10](/assets/images/2018-09-20-building-mail-server-1/10.png)
![11](/assets/images/2018-09-20-building-mail-server-1/11.png)

다음으로는 host이름을 변경합니다.
```bash
vim /etc/hostname
#이 명령어로 hostname 파일을 열고

dd
#raspberrypi 를 지워줍니다

i
#입력모드로 전환합니다. 

mast3r.com
#자신의 도메인주소를 적어줍니다

esc
#esc버튼을 눌러 입력모드를 나갑니다

:wq
#위 명령을 입력하여 저장 후 닫습니다.
```


다음은 `/etc/hosts`파일을 변경해야합니다.


![12](/assets/images/2018-09-20-building-mail-server-1/12.png)
![13](/assets/images/2018-09-20-building-mail-server-1/13.png)

위에서 했던 것 처럼 값을 변경/입력해줍니다.
변경할 값은 `127.0.1.1` 을 도메인 이름으로 변경하고
새 줄에는 DDNS대한 IP주소를 삽입 후 도메인을 입력하면 됩니다!
(제 IP는 가렸습니다. 소중하니까요..ㅋㅋ~~어차피 ping때리면 나오는데.. 왜가렸을까~~)

어쨋든! 이렇게 하시면 기본설정은 끝났습니다.

이제 실제 이메일에 사용될 계정을 만들어보도록 하겠습니다. 


![14](/assets/images/2018-09-20-building-mail-server-1/14.png)
![15](/assets/images/2018-09-20-building-mail-server-1/15.png)

이렇게 간단할 수가....
```bash
useradd linux
usermod -G mail linux
usermod -s /usr/sbin/nologin linux
passwd linux
```
이 4문장만 입력하시면 나중에 실제로 사용될 이메일 계정이 만들어지는 겁니다. 
```bash
vim /etc/passwd
vim /etc/group
```
계정을 만든 뒤에 위의 파일을 열어서 실제로 계정이 추가됐는지 확인할 수 있습니다. 

![16](/assets/images/2018-09-20-building-mail-server-1/16.png)
마지막으로 
```bash
chown :mail /home
chmod 775 /home
```
이 `/home`의 그룹을 `mail`로 변경하고 775권한을 주는 이유는 postfix에서 Maildir형식으로 메일 디렉토리를 구성할 것이기 때문입니다.(즉 이메일 계정이 생성될 때 마다 `/home/[user]/Maildir`이 생성됩니다)

모든 명령을 마친 이후에
![16](/assets/images/2018-09-20-building-mail-server-1/16.png)

```bash
reboot
```
리부팅을 해주면 모든 사전준비가 끝나게 됩니다.

따라오시느라 고생하셨습니다!
두번째 글에서는 Postfix설치 및 설정, letsencrypt를 이용한 SSL/TLS 설정을 포스팅 하도록하겠습니다. 
