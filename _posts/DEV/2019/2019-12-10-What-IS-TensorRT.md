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

**TensorRT**, 발표된지 꽤 되었지만, 한글자료가 많이 없네요.

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



## References

[1]: https://medium.com/@nsh235482/git-%EC%8B%A0%EC%9E%85%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%9D%98-git-%EC%82%AC%EC%9A%A9%EA%B8%B0-1-%EA%B8%B0%EB%B3%B8-%EA%B5%AC%EC%A1%B0-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-728c64824ebe	"GIT 기본 구조 이해하기"

[2]: http://blog.outsider.ne.kr/865