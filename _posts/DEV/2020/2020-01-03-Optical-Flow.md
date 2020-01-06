---
layout: post
title: "OpticalFlow란 무엇인가?"
categories: DEV
date: 2020-01-03
sitemap :
changefreq : daily
priority : 1.0
---

<center><span style="color:red">본 글은 References를 참고한 글임을 미리 밝혀둡니다.</span></center>
안녕하세요. 코딩벅스입니다. 

아시겠지만 영상은 시간순으로 정렬된 여러 이미지들의 조합입니다. 이 영상안에 존재하는 어떤 물체의 움직임(모션)을 벡터로 나타낼 수 있는데, 이때 사용되는 방법 가운데 하나가 바로 **Optical Flow**입니다. 



Optical Flow, 즉 광학적 흐름을 나타내는 이 벡터는 어떻게 구할 수 있을까요? 우선 전제가 필요합니다. 그 전제는 **"물체를 이루는 각 픽셀들의 밝기(Intensity)는 영상내에서 변함이 없다는 것"** 입니다. 



영상의 특정 시간 *t*라는 시점에서 Optical Flow를 구해보겠습니다. t에서 매우 짧은 시간이 흘렀을때를 t+deltat 라고 하면 









## References

[1]: https://eehoeskrap.tistory.com/302	""TensorRT 설치""

[2]:https://docs.nvidia.com/deeplearning/sdk/tensorrt-install-guide/index.html	"NVIDIA SDK INSTALLTION DEVELPOER GUIDE"
[3]: https://docs.nvidia.com/deeplearning/sdk/tensorrt-support-matrix/index.html	"Supported Matrix"
