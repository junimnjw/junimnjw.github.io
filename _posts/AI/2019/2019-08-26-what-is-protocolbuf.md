---
layout: post
title: "Tensorflow Serving 무엇인가?"
categories: AI
date: 2019-08-26
lastmod : 2019-08-26 14:00:00
sitemap :
changefreq : hourly
priority : 1.0
---

안녕하세요. 코딩벅스입니다.   


## 배경설명

텐서플로우에서 학습이 끝난 모델을 저장할 때, **Protocol Buffer(이하 ProtoBuf)** 포맷으로 저장합니다. 그리고 이후에 해당 포맷 파일을 읽고, 복원해 추론(Inference)에 사용하죠. 그렇다면 **ProtoBuf**포맷은 과연 무엇일까요? 왜 이 포맷을 사용하는 것일까요? 이번 포스트는 여기에 대한 답변을 드리고자 합니다. 



## 본문

우선 **ProtoBuf**는 구글이 만든 포맷이고, 오픈소스입니다. 구조화된 데이터를 쉽게 저장하고, 이를 쉽게 사용하도록 설계되었습니다. 중요한 특징은 ProtoBuf 포맷으로 저장된 하나의 파일을, 다양한 프로그래밍 언어에서 코드 레벨로 접근이 가능하도록 해준다는 것 입니다. 즉, 중간 언어개념의 공통어인 ProtoBuf 포맷으로 저장한 데이터를, 각 언어별로 주어진 컴파일러를 사용하면, 해당 언어에서 **코드레벨**로 데이터를 접근할 수 있습니다. 

텐서플로우가 학습한 모델을 저장하는 방식으로, **ProtoBuf**를 채택한 것은 텐서플로우가 다양한 프로그래밍 언어를 지원해야는 이유와 함께, 텐서플로우와 ProtoBuf를 만든 기업이 구글이라는 같은 기업이라는 점도 크게 작용했을 거라는 생각도 드네요. 

사용 순서는 다음과 같습니다. 

* ProtoBuf 포맷 데이터 저장하기 
* 컴파일(각 언어별 지원되는 protoc 컴파일러 사용)
* 컴파일된 모듈 로드하기

### ProtoBuf 파일 생성

ProtoBuf 포맷으로 작성된 코드의 예입니다. 라인별 자세한 설명은 [공식 가이드](https://developers.google.com/protocol-buffers/docs/pythontutorial)를 참고해주세요. 대충 보면, C++ 문법과 유사해보이기도 하네요. Person이라는 개체를 중첩된 구조체로 정의한 예제입니다. 



~~~protobuf
package tutorial;

message Person{
    required string name = 1;
    required int32 id = 2;
    optional string email = 3;

    enum PhoneType{
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
    }

    message PhoneNumber{
        required string number = 1;
        optional PhoneType type = 2 [default = HOME];
    }

    repeated PhoneNumber phone = 4;
}

message AddressBook{
    repeated Person = 1;
}
~~~



### 컴파일 하기

이제 위 proto 포맷 파일을 컴파일할 차례입니다. 이번 포스트에서 타겟 언어는 파이썬으로 택하였으니, 파이썬용 protoc 컴파일러를 설치해야겠죠. 

* 설치: pip install protobuf-compiler







### 컴파일 된 모듈 다시 로드




## 결론




## 참고문헌

[1]:https://bcho.tistory.com/1182 "조대협의 블로그"
[2]:https://developers.google.com/protocol-buffers/docs/pythontutorial