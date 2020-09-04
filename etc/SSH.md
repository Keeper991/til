# SSH



## Update History

* 20.09.05 최초 작성.





## Definition

**S**ecure **SH**ell.

Public Network를 통해 컴퓨터 간 서로 통신할 때 보안적으로 안전하게 진행하기 위해 사용되는 프로토콜, 혹은 클라이언트 프로그램.

컴퓨터 간 통신의 주요 목적으로, <u>데이터 전송</u>과 <u>원격제어</u>를 들 수 있는데 이를 위한 프로토콜로 FTP나 Telnet도 사용할 수 있다. 하지만 ssh를 사용하는 이유는, 이름에서도 알 수 있다시피 **"보안"**에 있다.



## Feature

* ssh는 기본적으로 한 쌍의 비대칭Key(Private, Public)를 통해, 인증과정을 거친다.

* **Private Key**로 암호화한 것은 <u>Public Key</u>로 복호화할 수 있고,
  <u>Public Key</u>로 암호화한 것은 **Private Key**로 복호화할 수 있다.

* Private Key는 이름 그대로, 외부로 노출되어선 안된다.
* Private Key는 Passphrase 옵션을 통해 로컬 저장소에 저장할 수 있다.



## Process

Key 생성의 주체가 어디인 지에 따라 다를 수 있지만, Client에서 생성하는 것으로 간주하고 설명한다.

1. Client에서 Key 쌍을 생성한 후, Server에 Public Key를 저장.
2. Server에서 난수를 생성, 저장된 Public Key로 난수를 암호화한 후 Client에 전달.
3. Client는 Private Key로 복호화 한 난수를 Server에 전달.
4. Server는 생성한 난수와 Client로부터 전달받은 난수를 비교.



## etc

블로그마다 설명에 조금씩 차이가 있다.

생성한 난수로 해시값을 만들어 보관했다가 비교하는 경우도 있고,
암호화하지 않은 난수를 먼저 전달하고 나중에 비교하는 경우도 있었다.

큰 줄기는 비대칭키를 통한 방식이라는 것이다.

추후에 자세히 알아볼 필요가 있다.



## 참고

* https://baked-corn.tistory.com/52

* https://darksoulstory.tistory.com/318
* https://arsviator.blogspot.com/2015/04/ssh-ssh-key.html
* https://medium.com/@labcloud/ssh-암호화-원리-및-aws-ssh-접속-실습-33a08fa76596