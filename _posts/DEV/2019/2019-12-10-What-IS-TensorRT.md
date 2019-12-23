---
layout: post
title: 'TensorRT란 무엇인가?'
categories: DEV
date: 2019-12-10
lastmod : 2019-23-10 09:00:00
sitemap :
changefreq : daily
priority : 1.0
---

<center><span style="color:red">본 글은 References를 참고한 글임을 미리 밝혀둡니다.</span></center>
<br>

안녕하세요. 코딩벅스입니다. 

**TensorRT**는 **NVIDIA**에서 만든 <span style="color:red;font-weight:bold">딥러닝 인퍼런싱 최적화 라이브러리</span>입니다. 쉽게 말하면, 기존 딥러닝 모델을 최적화 시켜, 인퍼런싱 성능을 극대화 시켜주는 라이브러리입니다. 2017년 **TensorRT 1**을 시작으로 현재 기준(2019년 12월)으로 **TensorRT 6**까지 나온 상황입니다. 

<br>

성능을 끌어올리는 건, **NVIDIA** **GPU**와 **CUDA-X AI** 등 최신 기술등을 활용해서 가능한 것입니다. 

<br>

**TensorRT** 동작은 크게 두가지 구조로 나뉩니다. 

<img src="https://devblogs.nvidia.com/parallelforall/wp-content/uploads/2016/06/GIE_Graphics_FINAL-1.png" style="zoom:67%;" />

첫 단계는 기존 딥러닝 모델을 최적화된 모델로 변경하는 것이죠. 

두번째 단계는 이러한 최적화된 모델을 사용해서 런타임에서 추론하죠. 

<br>



### 설치

**TensorRT**는 설치가 은근히 어렵습니다. 

저도 몇 시간 걸려서 예제 하나를 겨우 돌려봤네요. 

**TensorRT** 설치를 위해서는, 내가 사용하는 PC 혹은 서버의 플랫폼 버전은 어떻게 되는지,  GPU가 지원하는지, 그리고 CUDA 버전과, Tensorflow 버전은 어떻게 되는지등을 꼼꼼하게 챙겨야합니다. 이러한 환경에 따라서 **TensorRT**의 버전도 달라지기 때문이죠. 이러한 라이브러리들간의 전체적인 호환 정보는 [Supported Matrix Table][3]를 참고해주세요.

<br>

일단 저의 설치 환경은 다음과 같습니다. 

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

<br>