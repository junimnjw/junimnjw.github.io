---
layout: post
title: "Protcol Buffers, Who are You?"
categories: DEV
date: 2019-08-26
lastmod : 2019-08-26 14:00:00
sitemap :
changefreq : hourly
priority : 1.0
---



안녕하세요. 코딩벅스입니다.   


## 배경설명

 텐서플로우에서는 학습 완료한 모델을 **Protocol Buffers(이하 ProtoBuf)** 포맷으로 저장합니다. 그리고 추후 이 파일을 읽고, 모델을 복원해서 다시 추론(Inference)에 사용하죠. 여기서 **ProtoBuf** 포맷은 과연 무엇일까요? 이번 포스트에서는 이 질문에 대한 간략한 답변을 드리고자 합니다. 



## 본문

 우선 **ProtoBuf**는 **Google**이 만들었고, 데이터를 기술하는데 사용되는 언어입니다. 데이터를 효율적으로 저장, 전송, 다시 로드할 수 있도록 설계되었죠. **ProtoBuf** 포맷의 주된 특징은, 다양한 프로그래밍 언어에서 **코드 레벨**로 접근이 가능한 것입니다. 즉, **ProtoBuf**라는 중간 언어로 데이터의 구조를 작성하고, 이를 컴파일한 결과를 사용하면, 해당 프로그래밍 언어에서 **코드레벨**로 데이터 구조에 접근할 수 있습니다. 

텐서플로우에서 모델을 **ProtoBuf(확장자는 *.pb)**로 저장하는 것은 텐서플로우가 여러 프로그래밍 언어를 지원하는 점도 고려되었겠지만, 텐서플로우와 **ProtoBuf**를 만든 곳이 같은 기업이라는 점도 작용했을 거라고 봅니다. (끼워팔기??)

**ProtoBuf** 사용 순서는 다음과 같습니다. 

* ProtoBuf 포맷으로 데이터 구조 정의
* 컴파일하기(각 타겟 언어별 protoc 컴파일러 사용)
* 컴파일된 모듈 각 언어에서 코드레벨로 로드하기

### ProtoBuf 파일 생성

 다음은 **ProtoBuf** 포맷으로 작성한 코드의 예시입니다. 아례 예시들은 공식가이드의 내용을 참고하였으니, 자세한 설명은 [공식 가이드](https://developers.google.com/protocol-buffers/docs/pythontutorial)를 참고해주세요. 우선 Proto 파일 포맷에서  Address > Person > Phone이라는 중첩된 구조체를 정의하였습니다. 언뜻보면, C++ 문법에서 구조체 정의하는 것과 유사해보이기도 하네요. 

~~~protobuf
syntax = "proto2";

package tutorial;

message Person {
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    required string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }

  repeated PhoneNumber phones = 4;
}

message AddressBook {
  repeated Person people = 1;
}
~~~



### 컴파일

 이제 위에서 정의한 proto 파일을 컴파일 하겠습니다. 이번 포스트는 파이썬 기준으로 설명하겠습니다. 

* 컴파일러(protoc)를 사용한 컴파일 명령어의 예

  ~~~
   protoc -I="./" --python_out="./" "./addressbook.proto"
  ~~~

* 결과로 **addressbook_pb2.py** 생성됩니다.



### 컴파일 된 모듈 활용하기

 이제 위에서 확보한 컴파일러 결과물(addressbook_pb2.py)를 활용해서, 저만의 메시지를 저장하고 다시 읽어보겠습니다. 

* 메시지 작성(write.py)

  * 코드

    ~~~python
    import addressbook_pb2
    person = addressbook_pb2.Person()
    person.id = 1234
    person.name = "Jinwoo Nam"
    person.email = "junimnjw@gmail.com"
    phone = person.phones.add()
    phone.number = "010-1111-1111"
    phone.type = addressbook_pb2.Person.HOME
    
    try:
        f = open('myaddress', 'wb')
        f.write(person.SerializeToString())
        f.close()
        print("File is Written")
    except IOError:
        print("IO Error Occured")
    ~~~

    

  * 위 코드를 돌리면 myaddress 생성 완료!



* 메시지 읽기 (read.py)

  * 코드

    ~~~python
    import addressbook_pb2
    
    try:
        f = open('myaddress', 'rb')
        address_book = addressbook_pb2.AddressBook()
        address_book.ParseFromString(f.read())
        f.close()
    
        for person in address_book.people:
            print(person.id)
            print(person.name)
            if person.HasField('email'):
                print("Email Address:", person.email)
    except IOError:
        print('file read error')
    ~~~

    

  * 출력화면

    ~~~
    C:\Anaconda3\envs\ProtoBufTest\python.exe C:/Users/jjw.nam/PycharmProjects/ProtoBufTest/read.py
    1234
    Jinwoo Nam
    Email Address: junimnjw@gmail.com
    ~~~

    


## 결론

이번 포스트에서는 Protocol Buffers가 무엇인지 알아보고, 간단시 사용법을 살펴보았습니다. 텐서플로우를 사용해서 모델을 저장할 때 사용하는 **pb**  포맷이 무엇인지 이제 살짝 감이 오셨죠? 다음 연관 포스트에서는 텐서플로우 모델에서 이러한 Protocol Buffers 포맷으로 모델을 저장하고 로드하는 방법을 좀더 자세히 살펴볼 수 있는 시간을 가져보도록 하겠습니다. 




## 참고문헌

[1]:https://bcho.tistory.com/1182 "조대협의 블로그"
[2]: https://developers.google.com/protocol-buffers/docs/pythontutorial?hl=ko "Protocol Buffers Basic for Python"