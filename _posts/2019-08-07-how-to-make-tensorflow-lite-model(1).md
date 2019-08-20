---
layout: post
title: "텐서플로우 라이트 모델 생성(1)"
date: 2019-08-07
lastmod : 2019-08-11 09:16:00
sitemap :
changefreq : hourly
priority : 1.0
---

안녕하세요. 코딩벅스입니다.

## 배경설명



 **텐서플로우 라이트**, 텐서플로우의 경량화 버전이죠. 서버나 PC같은 고성능 장치에서 학습한 모델을 텐서플로우 라이트 포맷(*.tflite)으로 변환하면, 이 변환 모델을 가지고 텐서플로우 라이트를 통해 모바일에서 추론(Inference)할 수 있습니다. 이는 클라우드 서비스가 아닌, 온디바이스( On-Device), 즉 모바일 자체에서 실행가능한 딥러닝 구현입니다. 백문이 불여일행! 직접 학습한 텐서플로우 모델을 모바일에서 실행하는 과정을 설명하려고 합니다. 내용이 긴 만큼, 다음과 같이 나누어 연재하겠습니다.  

* 텐서플로우 모델 생성
* 텐서플로우 라이트 모델로 변환하기
* 안드로이드 앱에서 사용하기

## 본문

#### 텐서플로우 모델 생성

 먼저 대상이 될 딥러닝 모델이 필요합니다. 음... 당연한 말이죠. 근데 어떤 모델을, 어떻게 만들어볼까요? AlexNet, GoogleNet? 아무래도 직접 만들어보는게 좋으니.. 직접 설계할 수 있는 쉬운 딥러닝 모델은 어떨가요? 저는 딥러닝에서 HelloWorld라고 불리는, MNIST 데이터에 학습모델을 생성해보겠습니다. 아래는 모델을 학습하기 위한 텐서플로우 코드입니다. 

~~~python
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

with tf.Session() as sess:
    sess.run(tf.initialize_all_variables())
    for i in range(1000):
        batch_xs, batch_ys = mnist.train.next_batch(100)
        sess.run(train_step, feed_dict={X: batch_xs, Y_:batch_ys})

    correct_prediction = tf.equal(tf.math.argmax(Y, 1), tf.math.argmax(Y_, 1))
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
    print(sess.run(accuracy, feed_dict={X:mnist.test.images, Y_:mnist.test.labels}))
~~~

#### 모델 저장

 이제 위에서 학습한 모델을 저장합니다. 텐서플로우 모델을 저장한다는 건 어떤 의미일까요? 텐서플로우 모델은 **그래프(Graph)**입니다. 딥러닝모델은, 여러개의 노드들이 하나의 층(Layer)을 이루고, 그리고 수많은 선들이 이러한 층과 층을 연결하는 형태의 그래프인 것이죠.  시각적으로 볼 때 그렇다는 거죠. 그래서 텐서플로우 모델을 저장하면, 이러한 그래프정보를 저장하게 되고, 각 선(에지)들의 가중치 값을 별도로 저장하게 됩니다. 저장의 최종 포맷은 몇가지가 있지만 보통은 tf.train.saver API를 사용하면, 이러한 가중치는 체크포인트(checkpoint)라는 포맷으로 저장됩니다. 그리고 그래프 정보는 meta라는 형태로 저장됩니다. 

~~~python
checkpoint_output_path = os.path.join(os.getcwd(), 'checkpoint_output_dir', 'my_model')
saver = tf.train.Saver()

with tf.Session() as sess:
    sess.run(tf.initialize_all_variables())
    for i in range(1000):
        batch_xs, batch_ys = mnist.train.next_batch(100)
        if i % 100 == 0:
            saver.save(sess, checkpoint_output_path, global_step=i)
        sess.run(train_step, feed_dict={X: batch_xs, Y_: batch_ys})

    correct_prediction = tf.equal(tf.math.argmax(Y, 1), tf.math.argmax(Y_, 1))
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
    print(sess.run(accuracy, feed_dict={X: mnist.test.images, Y_: mnist.test.labels}))
~~~

#### Frozen Graph로 변환

 저장한 텐서플로우 모델을, 텐서플로우 라이트 포맷(tflite)로 바꾸기 위해, 한가지 더 사전 작업이 필요합니다. 바로 프리징(freezing)입니다. 텐서플로우 라이트 모델은 오로지 추론(inference)을 위해서 사용되는 그래프입니다. 추론을 위한 그래프 정보에는 학습에 사용되는 그래프의 불필요한 정보들은 버리고, 또한 가중치값도 더이상 변경이 없기때문에, 이를 상수화 시켜 저장합니다. 이렇게 그래프에 상수화된 가중치값을 포함한 형태로 굳히는 작업을 하고, 이를 가지고 tflite로 바꾸게 됩니다. 이러한 프리징(Freezing)을 돕기위해, tensorflow에서 제공하는 [freez_graph.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/tools/freeze_graph.py) 사용하면 됩니다. 

* Frozen Graph 생성

  * 기존에 생성해 두었던 ProtocolBuffer파일(pb)를 가지고 tensorboard에서 visualize하기

    * `python import_pb_to_tensorboard.py --model_dir model\my_train_model.pb --log_dir .\tensorboard\`

      (import_pb_to_tensorboard.py는 tensorflow github code를 통해 배포됩니다.)

  * Tensoboard에서 Output Node Name을 확인 & 기록 

    * `tensorboard --logdir=.\tensorboard\ --host localhost --port 8088`

      ![tensorboard_1](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/tensorboard_1.JPG?raw=true)

      ![tensorboard2](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/tensorboard_2.JPG?raw=true)

      **텐서보드를 사용해서 추론(Inference)단계에서 Output Node Name이 `Mean` 임을 확인하였습니다.**


  * freeze_graph.py를 사용한 **GraphDef 객체** 파일 획득

    * `python freeze_graph.py --input_graph=model\my_train_model.pbtxt --input_checkpoint=model\my_train_model.ckpt --output_graph=model\frozen_graph.pb --output_node_names=Mean`

      ![결과](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/freezed.JPG?raw=true)


![freezing 이전의 저장된 checkpoint 파일들](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/ckptfile.JPG?raw=true)

* meta: 그래프들(GraphDef, SaverDef)에 대한 정보
* xxx-data-xxx : 가중치, 바이어스, 플레이스 홀더등에 대한 값
* xxx-index-xxx: 각 텐서와 값에 대한 테이블 정보

## 결론

 자! 여기까지, 텐서플로우 모델을 직접 생성하고, 이를 저장하고, 또 Frozen Graph를 얻는 짧지만 아주 고된 과정을 살펴보았습니다. 아주 고생많으셨습니다. 다음 연재에서는 이렇게 확보한 Frozen Graph를, 드디어 우리가 원하는, 텐서플로우 라이트에서 인식가능한 형태인 tflite로 변환해보는 시간을 가져보겠습니다. 감사합니다!~

## 참고문헌

[1]https://medium.com/@prasadpal107/saving-freezing-optimizing-for-inference-restoring-of-tensorflow-models-b4146deb21b5

[2]https://eehoeskrap.tistory.com/343

[3]https://gusrb.tistory.com/21