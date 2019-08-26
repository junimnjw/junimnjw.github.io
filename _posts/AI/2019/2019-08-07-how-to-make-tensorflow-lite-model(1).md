---
layout: post
title: "텐서플로우 라이트 모델 생성(1)"
categories: AI
date: 2019-08-07
lastmod : 2019-08-25 09:16:00
sitemap :
changefreq : hourly
priority : 1.0
---

안녕하세요. 코딩벅스입니다.



## 배경설명



 **텐서플로우 라이트**, 텐서플로우의 경량화 버전입니다. GPU가 탑재된 서버나 PC같은 고성능 장치에서 텐서플로우로 생성한 모델을, 텐서플로우 라이트에서 실행가능한 형태로 변환 과정을 거치게되면, 스마트폰과 같은 임베디드 시스템에서도 추론(Inference)할 수 있습니다. 이는 온디바이스(On-Device), 즉 네트워크를 통해서 서버에서 추론한 결과를 뿌려주는 것이 아닌, 장치 내부에서 추론이 직접 가능하도록 하는 것입니다. 이제, 직접 텐서플로우로 작성한 딥러닝 모델을 모바일에서 실행해보는 과정을 포스팅하려고 합니다. 내용이 다소 긴 만큼, 다음과 같이 나누어 연재하겠습니다.  

* 텐서플로우 모델 생성
* 텐서플로우 라이트 모델로 변환
* 안드로이드 앱에서 사용하기


## 본문

#### 모델 생성

먼저 텐서플로우 모델이 필요합니다. 당연한 말이죠. 근데 어떤 모델을 텐서플로우로 만들어볼까요? AlexNet, GoogleNet? 이런 복잡한 딥러닝 모델은.. 구조가 복잡해서 다루기가 어렵네요. 직접 만들어보는데는 역시 딥러닝의 HelloWorld 예제라고 불리는, **MNIST 데이터 분류 모델**이 적당해 보입니다. 
모델을 생성하는 텐서플로우 파이썬 코드는 아래 별도로 첨부하였습니다. 참고해주세요. 

#### 모델 저장

이제 위에서 학습한 모델을 저장할 차례입니다. 텐서플로우 모델을 **저장(Store)**한다는 건 어떤 의미일까요? 텐서플로우 모델은 **그래프(Graph)**입니다. 즉, 여러개의 노드가 하나의 층(Layer)을 이루고, 그리고 이러한 층과 층을 여러개의 선들이 연결하는 형태로 표현됩니다. 그래서 텐서플로우 모델을 저장한다는 건, 이러한 그래프 정보를 저장하고, 또 각 층과 층사이를 연결하는 수많은 선(에지)들이 가지는 가중치 값을 별도로 저장한다는 것과 같습니다. 저장의 형태는 보통은 tf.train.Saver API를 사용해서, 체크포인트(checkpoint)라는 포맷으로 가중치 값을 저장하고, meta라는 포맷으로 그래프 정보를 저장합니다. tf.train.Saver를 사용하여 모델을 저장하는 예시를 아래 별도로 첨부하였습니다. 역시 참고해주세요. 

#### 코드

```python
from tensorflow.examples.tutorials.mnist import input_data
mnist = input_data.read_data_sets("MNIST_data/", one_hot=True)

import tensorflow as tf

X = tf.placeholder(tf.float32, [None, 784])
Y_ = tf.placeholder(tf.float32, [None, 10])

W = tf.Variable(tf.zeros([784, 10]))
b = tf.Variable(tf.zeros([10]))
K = tf.matmul(X, W) + b
Y = tf.nn.softmax(K)
loss_function = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(logits=K, labels=Y_))
train_step = tf.train.GradientDescentOptimizer(0.5).minimize(loss_function)

# To store the above model as checkpoint files
ckpt_output_path = os.path.join(os.getcwd(), 'ckpt_output_dir', 'my_model')
saver = tf.train.Saver()

with tf.Session() as sess:
    sess.run(tf.initialize_all_variables())
    for i in range(1000):
        batch_xs, batch_ys = mnist.train.next_batch(100)
        if i % 100 == 0:
        #save a checkpint per every 100 training images
            saver.save(sess, ckpt_output_path, global_step=i)
        sess.run(train_step, feed_dict={X: batch_xs, Y_: batch_ys})

    correct_prediction = tf.equal(tf.math.argmax(Y, 1), tf.math.argmax(Y_, 1))
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
    print(sess.run(accuracy, feed_dict={X: mnist.test.images, Y_: mnist.test.labels}))
```


#### Frozen GraphDef 구하기

이렇게 저장한 텐서플로우 모델을, 텐서플로우 라이트에서 로드가능한 형태(tflite)로 바꾸는 작업을 위해, 한가지 더 추가적인 작업이 필요합니다. 바로 프리징(freezing)입니다. 텐서플로우 라이트는 학습이 아닌 추론(inference)만을 위해 사용됩니다. 따라서, 프리징을 통해서 기존 텐서플로우 모델에서 추론에 불필요한 부분들은 날려버리고, 가중치 값도 상수화 시키는 작업을 진행합니다. 이러한 과정을 가치게 되면, 추론용 모델 그래프와 상수화된 가중치 값이 굳어진 ProtocolBuf(pb) 포맷으로 만들어지게 됩니다. 이러한 작업은, 텐서플로우 패키지에서 제공하는 [freez_graph.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/tools/freeze_graph.py)를 사용하면 쉽게 가능합니다.

##### freeze_graph.py 사용법

Frozen GraphDef 생성을 위해서 크게 두 가지 절차가 필요합니다. 

* 텐서보드를 실행 후, 텐서플로우 모델의 추론 단계의 **최종 아웃풋 노드**의 이름을 확인
**Output Node Name이 `Mean` 임을 확인하였습니다.

* Frozen GraphDef 생성
** `python freeze_graph.py --input_graph=model\my_train_model.pbtxt --input_checkpoint=model\my_train_model.ckpt --output_graph=model\frozen_graph.pb --output_node_names=Mean`

* 결과화면  
![결과](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/freezed.JPG?raw=true)
![freezing 이전의 저장된 checkpoint 파일들](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/ckptfile.JPG?raw=true)


## 결론

자! 여기까지, 텐서플로우 모델을 직접 만들고, 저장하고, 또 이를 활용하여 Frozen Graph를 얻는 과정을 살펴보았습니다. 고생많으셨습니다. 다음 연재에서는 이렇게 확보한 Frozen Graph를 가지고, 드디어 텐서플로우 라이트 포맷(tflite)으로 변환해보는 시간을 가져보겠습니다. 감사합니다!~

## 참고문헌