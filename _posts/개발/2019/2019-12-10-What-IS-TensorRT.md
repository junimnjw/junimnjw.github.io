---
layout: post
title: "TensorRT란 무엇인가?"
categories: 개발
date: 2019-12-10
lastmod : 2019-12-23 09:00:00
sitemap :
changefreq : daily
priority : 1.0
---

<center><span style="color:red">본 글은 References를 참고한 글임을 미리 밝혀둡니다.</span></center>
안녕하세요. 코딩벅스입니다. 

**TensorRT**는 **NVIDIA**에서 만든 <span style="color:red;font-weight:bold">딥러닝 인퍼런싱 최적화 라이브러리</span>입니다. 쉽게 말해서, 기존 딥러닝 모델을 구조적으로 개선하여, 추론 성능을 향상시키는 라이브러리입니다. 2017년 **TensorRT 1**을 시작으로 현재(2019년 12월 기준) **TensorRT 6**까지 나온 상황입니다. 

 이렇게 성능을 끌어올리는게 가능한 건, **NVIDIA** **GPU**와 **CUDA-X AI** 등 최신 하드웨어와 기술등이 결합되어 사용되기 때문이죠. 

**TensorRT** 동작은 다음 그림과 같이 크게 두가지로 나뉩니다. 

<img src="https://devblogs.nvidia.com/parallelforall/wp-content/uploads/2016/06/GIE_Graphics_FINAL-1.png" style="zoom:67%;" />

첫 단계는 기존 딥러닝 모델을 최적 모델로 변환하는 것이죠. 

두번째 단계는 변환된 딥러닝 모델을 **TensorRT 런타임**에서 구동시키는 것입니다. 

<br>



### 설치

**TensorRT**는 설치가 은근히 어렵습니다. 

저도 몇 시간 걸려서 예제 하나를 겨우 돌려봤네요. 

**TensorRT** 설치를 위해서는, 사용하는 PC 또는 서버의 플랫폼 버전,  CUDA 버전, Tensorflow 버전 등을 꼼꼼하게 챙겨야합니다. 이러한 환경들에 따라서 사용가능한 **TensorRT**의 버전도 달라지기 때문이죠. 라이브러리들간의 호환 정보는 [Supported Matrix Table][3]를 참고해주세요.

<br>

저의 설치 환경은 다음과 같습니다. 

* GTX 1660
* Ubuntu 18.04 LTS
* Python 3.6
* Cudnn 7.6.5
* CudaToolkit - NVIDIA CUDA 10.1
* Tensorflow 1.14
* <span style="color:red;font-weight:bold">TensorRT 6.0.1</span>

<br>

**TensorRT** 설치에 대한 자세한 설명은 다음 [개인블로그][1]와 [공식가이드][2]로 갈음한다.

다만, 경험끝에 얻은 개인적인 팁들을 공유합니다.

첫째, 가상환경에서 TensorRT설치는 쉽지않습니다. 

<u>**그냥 sudo를 사용해서 root에서 설치하세요. 제발**</u> 



## References

[1]: https://eehoeskrap.tistory.com/302	""TensorRT 설치""

[2]:https://docs.nvidia.com/deeplearning/sdk/tensorrt-install-guide/index.html	"NVIDIA SDK INSTALLTION DEVELPOER GUIDE"
[3]: https://docs.nvidia.com/deeplearning/sdk/tensorrt-support-matrix/index.html	"Supported Matrix"
