---
layout: post
title: "OpticalFlow란 무엇인가?"
categories: 개발
date: 2020-01-03
lastmod : 2020-01-07 14:00:00
sitemap :
changefreq : daily
priority : 1.0
---

<center><span style="color:red">본 글은 References를 참고한 글임을 미리 밝혀둡니다.</span></center>
<br>

안녕하세요. 코딩벅스입니다. 

 **영상(Video)**은 시간 축으로 나열된 **2차원 이미지들(혹은 프레임이라고도 함)**의 조합입니다. 이 때 영상에서 연속하는 두 이미지의 차이를 통해서, 영상에 존재하는 물체의 움직임(모션)을 추정할 수 있습니다. 이러한 모션을 추정하기 위해 사용되는 방법 중 하나가 바로 **Optical Flow**입니다. 

**Optical Flow**를 사용하여 움직임을 어떻게 추정할까요? 우선은 **"영상에서 연속하는 두 프레임 간의 관심 픽셀의 밝기값(Intensity)이 변함이 없다"**는 전제가 필요합니다. 이 전제가 성립하지 않으면 **Optical Flow**를 통한 물체의 움직임 추정은 어렵습니다. 

 위 전제가 만족한다면, 영상에서 특정 ***t***프레임의 관심 픽셀(x,y)의 밝기값 I(x,y,t)는 연속하는 다음 프레임에서 위치만 변하고, 밝기값은 그대로 유지될 것입니다. 따라서 다음과 같은 식이 성립합니다. 

<br>
$$
I(x,y,t)=I(x+\delta{x}, y+\delta{y}, t+\delta{t}) - (1)
$$
<br>

(1) 식의 우항을 전개하면, 

<br>
$$
I(x + \delta{x}, y + \delta{y}, t + \delta{t}) = I(x, y, t) 
+\frac{\partial{I} }{\partial{x}}\delta{x}
+\frac{\partial{I} }{\partial{y}}\delta{y}
+\frac{\partial{I} }{\partial{t}}\delta{t} - (2)
$$
<br>

가 됩니다. 이 때, (1)과 (2)를 합치면, 다음과 같은 식이 성립합니다. 

<br>
$$
\frac{\partial{I} }{\partial{x}}\delta{x}
+\frac{\partial{I} }{\partial{y}}\delta{y}
+\frac{\partial{I} }{\partial{t}}\delta{t}=0      - (3)
$$
<br>

식 (3)을 다시 정리하면 아래와 같습니다. 

<br>
$$
\frac{\partial{I} }{\partial{x}} \frac{\partial{x}}{\partial{t}}
+\frac{\partial{I} }{\partial{y}} \frac{\partial{y}}{\partial{t}}
+\frac{\partial{I} }{\partial{t}} \frac{\partial{t}}{\partial{t}}=0 - (4)
$$

<br>

와 는 결국, x와 y방향으로의 각각 움직임의 속도를 가리킵니다. 마지막으로, 식(4)를 간추리면 

다음과 같이 쓸 수 있습니다. 

<br>
$$
I_xV_x+I_yV_y=-I_t - (5)
$$

<br>


 영상에서 관심 점들을 설정하고, 각 관심 점들을 중심으로 하는 블록을 설정합니다. 그 블록 단위로 저 위의 식을 적용해서 Motion Vector를 구하는 것이죠. 사실 직접 구현해 보는 것이 학습에 가장 좋지만, 당장 사용이 급하다면 Motion Vector를 구하기 위해 사용되는 방법론 중 하나인 Lucas-Kanade을 사용하면 되고, 이 방법론은 이미 OpenCV에서 구현해서 API로 제공하기 때문에 바로 가져다 쓰시면 됩니다. 



## References

[1]: 

