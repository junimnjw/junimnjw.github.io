---
layout: post
title: "텐서플로우 라이트 모델 생성(1)"
categories: ML
date: 2019-08-07
lastmod : 2019-08-11 09:16:00
sitemap :
changefreq : hourly
priority : 1.0
---

안녕하세요. 코딩벅스입니다.



## 배경설명



 **텐서플로우 라이트**, 텐서플로우의 경량화 버전입니다. 서버나 PC와 같은 고성능 장치에서 학습한 모델을 변환 과정을 거쳐서 텐서플로우 라이트를 통해 모바일에서도 추론(Inference)할 수 있습니다. 이는 온디바이스( On-Device), 즉 모바일 자체에서 실행가능한 딥러닝 서비스입니다. 백문이 불여일행! 직접 텐서플로우로 작성한 딥러닝 모델을 모바일에서 실행해보는 과정을 포스팅하려고 합니다. 내용이 긴 만큼, 다음과 같이 나누어 연재하겠습니다.  

* 텐서플로우 모델 생성
* 텐서플로우 라이트 모델로 변환
* 안드로이드 앱에서 사용하기



## 본문



#### 모델 생성

 먼저 대상 모델이 필요합니다. 음... 당연한 말이죠. 근데 어떤 모델을 만들어볼까요? AlexNet, GoogleNet? 이런 복잡한 딥러닝 모델은.. 어렵네요. 직접 만들어보기에는 딥러닝에서 HelloWorld라고 불리는, MNIST 데이터 학습모델이 적당해 보입니다. 



#### 모델 저장

 이제 위에서 학습한 모델을 저장합니다. 텐서플로우 모델을 저장한다는 건 어떤 의미일까요? 텐서플로우 모델은 **그래프(Graph)**입니다. 딥러닝모델은, 여러개의 노드들이 하나의 층(Layer)을 이루고, 그리고 수많은 선들이 이러한 층과 층을 연결하는 형태의 그래프인 것이죠.  시각적으로 볼 때 그렇다는 거죠. 그래서 텐서플로우 모델을 저장하면, 이러한 그래프정보를 저장하게 되고, 각 선(에지)들의 가중치 값을 별도로 저장하게 됩니다. 저장의 최종 포맷은 몇가지가 있지만 보통은 tf.train.saver API를 사용하면, 이러한 가중치는 체크포인트(checkpoint)라는 포맷으로 저장됩니다. 그리고 그래프 정보는 meta라는 형태로 저장됩니다. 



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
            saver.save(sess, ckpt_output_path, global_step=i)
        sess.run(train_step, feed_dict={X: batch_xs, Y_: batch_ys})

    correct_prediction = tf.equal(tf.math.argmax(Y, 1), tf.math.argmax(Y_, 1))
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
    print(sess.run(accuracy, feed_dict={X: mnist.test.images, Y_: mnist.test.labels}))
```



#### Frozen Graph로 변환

 저장한 텐서플로우 모델을, 텐서플로우 라이트 포맷(tflite)으로 바꾸기 위해, 추가적인 작업이 필요합니다. 바로 프리징(freezing)입니다. 텐서플로우 라이트는 학습이 아닌 오로지 추론(inference)만을 위해서 사용됩니다. 따라서, 기존 모델에서 학습에만 유용한 그래프 성분들은 버리고, 가중치값도 상수화 시켜 저장합니다. 이렇게 추론용 그래프 정보에 상수화된 가중치값을 굳혀 하나의 파일(pb)로 만드는 작업을 프리징(Freezing)이라 하고, 이러한 작업을 돕기위해, [freez_graph.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/tools/freeze_graph.py) 을 주로 사용합니다.



Frozen Graph 파일을 생성하기 위해서는 크게 두 가지 절차가 필요합니다. 

* 텐서보드 실행하여 기존 모델에서 Output Node Name을 확인하고 기록해 둡니다. 

  * 텐서보드 로그 생성

    `$ python import_pb_to_tensorboard.py --model_dir model\my_train_model.pb --log_dir .\tensorboard\`

  * 텐서보드 실행

    `$ tensorboard --logdir=.\tensorboard\ --host localhost --port 8088`

  * 결과화면(크롬에서 localhost:8088 접속)

![tensorboard_1](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/tensorboard_1.JPG?raw=true)

![tensorboard2](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/tensorboard_2.JPG?raw=true)

**Output Node Name이 `Mean` 임을 확인하였습니다.**


  * [freez_graph.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/tools/freeze_graph.py)를 사용한 **Frozen Graph** 파일 생성

    * `python freeze_graph.py --input_graph=model\my_train_model.pbtxt --input_checkpoint=model\my_train_model.ckpt --output_graph=model\frozen_graph.pb --output_node_names=Mean`

    * 결과화면
    
      ![결과](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/freezed.JPG?raw=true)


![freezing 이전의 저장된 checkpoint 파일들](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/ckptfile.JPG?raw=true)





## 결론

 

 자! 여기까지, 텐서플로우 모델을 직접 만들고, 저장하고, 또 이를 활용하여 Frozen Graph를 얻는 과정을 살펴보았습니다. 고생많으셨습니다. 다음 연재에서는 이렇게 확보한 Frozen Graph를 가지고, 드디어 텐서플로우 라이트 포맷(tflite)으로 변환해보는 시간을 가져보겠습니다. 감사합니다!~



## 참고문헌
