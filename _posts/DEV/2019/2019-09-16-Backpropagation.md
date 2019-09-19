---
layout: post
title: 'Backpropagation, Who are You?'
categories: DEV
date: 2019-09-16
lastmod : 2019-09-16 14:00:00
sitemap :
changefreq : daily
priority : 1.0
---



<span style="font-size:11pt;color:blue">*오랜만에 네이버 블로그에 작성해 둔 글들을 다시 연재합니다. 지난 글들을 보니 제가 쓴 글이지만 허접(?)하네요. 그래도 유익하길 바라며, 많은 피드백 부탁드립니다.*</span>

### 들어가며

 신경망(Neural Network)의 **성능**을 결정짓는데 결정적인 요소들이 있습니다. 

* 뛰어난 컴퓨팅 파워(GPU 등, 돈이 필요하네요...)
* 대량의 고품질 Labeled 데이터 (이것도 다 돈이죠...)
* **학습 알고리듬**

 이들 중 외부적 요인이 아닌, 소프트웨어 기술과 연관된 것은 **학습 알고리듬**입니다. 즉, 소프트웨어 관점에서 <u>주어진 데이터에 대해서 컴퓨터가 더 빠르고 제대로 학습하기 위한 방법</u>에 대한 부분이죠. 그래서 오늘은 신경망 학습법의 뼈대가 되는 **역전파(이하 Backpropagation)** 알고리즘을 언급하려고 합니다. 

<br>

### 본문

  1986년, 제가 기어다닐 시기에... 제프리 힌튼(G. Hinton) 교수는 신경망 연구에서 획기적인 논문을 발표합니다. 그 논문의 핵심은 **Backpropagation**입니다. 간단히 말하면, 신경망 출력값과 기대값 사이의 오차를 <span style="color:red">**역방향**</span>으로 전파하면서 최적의 연결 가중치를 찾는 것이죠. 

<br>

 아래 그림은 2개의 층으로 이루어진 매우 단순한 신경망을 도식화한 것 입니다. 실제 딥러닝에서 활용되는 신경망들은 이보다 훨씬 복잡한 구조를 이루고 있습니다.  다만, **Backpropagation**에 대한 이해를 돕기위한 것임을 알려드립니다. 



![img1](/assets/img/backpropagation1.png)



**Backpropagation**은 결국 신경망 학습에서 층과 층사이를 연결하는 수많은 에지(Edge)들에 대한 최적의 가중치를 찾아가는 과정입니다. 최적의 가중치를 찾는 목표는 결국 훈련데이터를 신경망에 넣어 출력된 값과 기대한 값의 오차 **E**가 최소화 되는 시점이죠. 오차 E를 구하는 건 **손실함수(이하 Cost Function)** 개념을 사용합니다.

 <br>

우선, Backpropagation이 이루어지는 과정을 설명하기 전에, 우리가 다룰 신경망에 대한 전제를 두겠습니다. 신경망에서 사용하는 **활성화 함수(이하 Activate Function)**은 **시그모이드(이하 Sigmoide)**로 하고, **Cost Function**은 크로스 엔트로피라고 하겠습니다. (왜 CrossEntropy를 CostFunction으로 사용하는지에 대해서는 다음기회에...)



Activate Function: Sigmoid
$$
Z_i=\frac{1}{1+e^{-{S}_{i}}}
$$

$$
where, S_i = \sum_{j=1}{Y_j}{W_{ji}}
$$



![sigmoid](/assets/img/sigmoid.jpg)



Cost Function: Cross Entropy
$$
E = -\sum_{i=1}(t_i\log(Z_i)+(1-t_i)\log(1-Z_i))
$$


자! **Backpropagation**이 이루어지는 과정은 크게 두 단계의 반복입니다. 

1. 순전파
   1. 입력된 데이터로부터 출력값과 기대값의 오차 **E** 구하기
2. 역전파
   1. 오차 **E** 대한 에지(Edge)별 그래디언트 계산
   2. 해당 그래디언드만큼 연결 가중치 조정
3. Repeat until **E is minimized**

<br>

 **Backpropagation**의 핵심은 2번 **"각 연결마다 오차에 E 대한 기여도"**, 그리고 **"각 연결의 가중치 조정"**이죠. 

우선 1번의 **"실제 출력값과 기대값의 오차 E"**를 구하기 위해 크로스 엔트로피식을 사용합니다. 
$$
E = -\sum_{i=1}(t_i\log(Z_i)+(1-t_i)\log(1-Z_i))
$$

이제, 2번의 역전파를 준비하기 위한 과정을 보겠습니다. 

출력층\\({Z}\_{i}\\)과 그 직전층\\({Y}\_{j}\\)사이의 임의의 연결 \\({W}_{ji}\\)에서 오차 **E**에 대한 기여도는 아래와 같이 정의됩니다. 
$$
\frac{\partial E}{\partial W_{ji}}
$$
즉, 오차와 연결 관계에서 특정한 접점에서의 기울기죠. 여기서 체인 룰을 이용해서 위 식을 펼치면, 다음과 같은 유도가 가능합니다. 
$$
\frac{\partial E}{\partial W_{ji}}=\frac{\partial E}{\partial Z_i}\frac{\partial Z_i}{\partial S_i}\frac{\partial S_i}{\partial W_{ji}}
$$
<br>

오른쪽 항의 각 부분을 다시 전개해보겠습니다. 

<br>
$$
\frac{\partial E}{\partial Z_i}=\frac{\partial(-\sum_{i=1}(t_i\log(Z_i)+(1-t_i)\log(1-Z_i)))}{{\partial Z_i}}=\frac{-t_i}{Z_i}+\frac{1-t_i}{1-Z_i}=\frac{Z_i-t_i}{Z_i(1-Z_i)}
\\
\frac{\partial Z_i}{\partial S_i}=\frac{\partial (\frac{1}{1+e^{-{S}_{i}}})}{\partial S_i}={Z_i}{(1-Z_i)}
\\
\frac{\partial S_i}{\partial W_{ji}}=Y_j
$$

$$
\therefore \frac{\partial E}{\partial W_{ji}}=(Z_i-t_i)\cdot{Y_j}
\\
\because \frac{\partial E}{\partial W_{ji}}=\frac{Z_i-t_i}{Z_i(1-Z_i)}\cdot{Z_i}{(1-Z_i)}\cdot{Y_j}=(Z_i-t_i)\cdot{Y_j}
$$


<br>

## 결론

 이렇게 당대에 획기적이라고 일컬어졌던 **Backpropagation**도 물론 완벽한 알고리듬은 아니었습니다. 인공 신경망의 층이 깊어질수록 학습 데이터에 지나치게 오버피팅(over-fitting)하는 문제가 발견된 것이죠. 가장 큰 원인으로 **그래디언트 소실(Vanishing Gradient Problem)**이 지적되었고, 이러한 제약때문에 1986년부터 2006년까지 다소 긴 시간동안 인공 신경망 연구분야의 빙하기가 있었습니다. 다음 포스트는 이러한 걸림돌이었던 Vanishing Gradient Problem가 도대체 무엇인지 살펴보고, 이를 해결하기 위해 등장한 주요 해결책들을 살펴보는 시간을 가져보도록 하겠습니다.  



## 참고문헌

[1]:http://jaejunyoo.blogspot.com/2017/01/backpropagation.html