---
layout: post
title: "SSH란 무엇인가?"
categories: 개발
date: 2019-11-28
lastmod : 2019-12-23 09:00:00
sitemap :
changefreq : daily
priority : 1.0
---



안녕하세요. 코딩벅스입니다. 

 네트워크를 통해서 다른 컴퓨터나 서버로 접속할 때, 보통 다음과 같은 두가지 목적이 있습니다. 

* 파일전송
* 원격 제어

 파일 전송은 **FTP(File Transfer Protocol)** 프로토콜을 보통 사용하죠. **<u>그럼 원격 제어의 경우는 어떤 프로토콜을 사용할까요?</u>** 예전에는 **Telnet** 프로토콜을 사용했었습니다. 근데 **Telnet**은 어느새 자취를 감추고, 요즘은 다들 **SSH**를 사용합니다. 저도 사용을 하지만.. 정확한 정의를 누가 물으면, 선뜻 대답하기 어려웠습니다. 

 그래서 몇 가지 정보를 찾아보고, 제가 이해한 바를 공유드리고자 합니다. **SSH**는 **S**ecure **Sh**ell Protocol의 줄임말이고, 번역하면 보안이 강화된 쉘 프로토콜 입니다.  **Telnet**도 쉘 기반의 원격 제어 프로토콜이지만,  **보안성이 떨어진다는 치명적인 단점을 가지고 있습니다.** 즉, 누군가 안좋은 마음을 품고 의도적으로 클라이언트와 서버 중간에서 오가는 메시지를 갈취하면, 이러한 메시지들이 암호화 되어있지 않아 그대로 노출되어버리는 위험이 존재하죠. 그래서 **Telnet의 단점**인 보안성을 강화한 쉘 기반의 새로운 프로토콜이 필요했고, 이 기능을 제공하는 것이 바로 **SSH**입니다. 

**SSH**는 한 쌍의 Public Key와 Private Key로 동작합니다. 아래는 **SSH**를 통한 클라이언트와 서버간 상호 인증 과정입니다. 

<br>

(1) 우선, 클라이언트에서 한 쌍의 Publick Key와 Privte Key를 생성합니다. 

(2) (1)에서 생성한 Public Key를 통신할 서버에 복사 및 추가합니다. 

(3) 서버에 로그인하는 시점에는, (2)에서 서버에 복사해 두었던 Public Key로 암호화 하여 클라이언트에게 전달. 

(4) 클라이언트는 전달받은 문자열을 Private Key로 복호화

(5) (4)에서 복호화한 메시지를, **대칭키**와 합쳐서 해쉬값으로 만들어 다시 서버로 전송

(6) (5)번의 해쉬 값을 서버에서 검증하여 일치시 클라이언트 인증 완료

(7) 이후에는 **대칭키**로 서로 통신합니다. 

<br>

위 과정을 거치면 누가 중간에서 메시지를 갈취하더라도 메시지에 대한 복호화가 어렵기 때문에, **SSH**는 보안성이 뛰어난 프로토콜이라고 볼 수 있습니다. 

<br>

### 기타 정보

* 22번 포트를 사용합니다. 

* RSH, Rlogin, Telnet 등을 대체하기 위해 설계되었습니다. 

<br>

## References

[1]: https://swalloow.github.io/ssh-tunneling	"SSH 프로토콜과 Tunneling이해하기"


