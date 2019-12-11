---
layout: post
title: 'TensorRT 이해하기'
categories: DEV
date: 2019-12-10
lastmod : 2019-12-10 09:00:00
sitemap :
changefreq : daily
priority : 1.0
---

<center><span style="color:red">본 글은 References를 참고한 글임을 미리 밝혀둡니다.</span></center>
<br>

안녕하세요. 코딩벅스입니다. 

**TensorRT**, 발표된지 꽤 되었지만 한글자료가 많이 없네요.

<br>

군더더기 없는 **TensorRT** 설명, 시작하겠습니다. **TensorRT**는 **nVidia**에서 만든 <span style="color:red;font-weight:bold">딥러닝 인퍼런싱 전용 라이브러리</span>입니다. 딥러닝 모델을 최적화 시켜서, 인퍼런싱 성능을 극대화 시켜주는 임무를 가집니다. 이 **라이브러리**는 2017년 TensorRT1을 시작으로 현재(2019년 12월 기준)는 TensorRT 6까지 나온 상황입니다. 

<br>

어떻게 성능을 끌어올릴수 있죠? 그건 **nVidia**에서 만든 **GPU**와 **CUDA-X AI** 등 최신 기술등을 활용해서 가능한 것입니다. 

<br>

**TensorRT**는 크게 두가지 구조로 나뉩니다. 

<img src="https://devblogs.nvidia.com/parallelforall/wp-content/uploads/2016/06/GIE_Graphics_FINAL-1.png" style="zoom:67%;" />

첫번째 단계에서는 기존 학습된 딥러닝 모델을 양자화(Quantization)등을 통해서 성능 최적화합니다. 

<br>

두번째 단계에서는 최적화된 모델을 런타임에서 실시간으로 추론하죠. 

<br>



### 설치

설치가 꽤 어렵습니다. 저도 3시간 정도 걸려서 겨우 TensorRT 예제를 돌려봤네요. 

**TensorRT**설치를 위해서는, 내 플랫폼 버전은 어떻게 되는지, GPU가 지원하는지, 사용중인 CUDA 버전과, Tensorflow 버전은 어떻게 되는지등을 꼼꼼하게 챙겨야합니다. 왜? 위 환경에 따라서 **TensorRT**의 설치 가능 버전도 달라지기 때문이죠. 호환 버전에 대한 자세한 정보는 [Supported Matrix Table](https://docs.nvidia.com/deeplearning/frameworks/tf-trt-user-guide/index.html#prereqs)를 참고해주세요.

<br>

일단 저의 설치 환경은 다음과 같습니다. 

* Nvidia GTX 1060
* Ubuntu 18.04 LTS
* Python 3.6
* Cudnn 7.6.5
* CudaToolkit - NVIDIA CUDA 10.1
* Tensorflow 1.14
* <span style="color:red;font-weight:bold">TensorRT 6.0.1</span>

<br>

**TensorRT** 설치에 대한 자세한 설명은 다음 [개인블로그](1)와 [공식가이드](2)로 갈음한다.

다만, 수차례 경험끝에 얻은 개인적인 팁들을 공유합니다.

첫째, 가상환경에서 TensorRT설치는 쉽지않습니다. 그냥 sudo를 사용해서 root에서 설치하세요. 





## References

[1]: https://eehoeskrap.tistory.com/302	"TensorRT 설치"

[2]:https://docs.nvidia.com/deeplearning/sdk/tensorrt-install-guide/index.html	"NVIDIA SDK INSTALLTION DEVELPOER GUIDE"