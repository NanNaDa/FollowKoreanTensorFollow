# 기본 사용법(Basic Usage)

(v1.0)

TensorFlow를 사용하기 위해 먼저 TensorFlow가 어떻게 동작하는지를 이해해 봅시다.

- 연산은 graph로 표현합니다.(역자 주: graph는 점과 선, 업계 용어로는 노드와 엣지로 이뤄진 수학적인 구조를 의미합니다.)
- graph는 `Session`내에서 실행됩니다.
- 데이터는 tensor로 표현합니다.
- `변수(Variable)`는 (역자 주: 여러 graph들이 작동할 때도) 그 상태를 유지합니다.
- 작업(operation 혹은 op)에서 데이터를 입출력 할 때 feed와 fetch를 사용할 수 있습니다.

## 개요(Overview)
TensorFlow는 graph로 연산(역자 주: 앞으로도 'computation'을 연산으로 번역합니다)을 나타내는 프로그래밍 시스템입니다. graph에 있는 노드는 작업(op)(작업(operation)의 약자)라고 부릅니다. 작업(op)은 0개 혹은 그 이상의 `Tensor`를 가질 수 있고 연산도 수행하며 0개 혹은 그 이상의 `Tensor`를 만들어 내기도 합니다. TensorFlow에서 `Tensor`는 정형화된 다차원 배열(a typed multi-dimensional array)입니다. 예를 들어, 이미지는 부동소수점 수(floating point number)를 이용한 4차원 배열(`[batch, height(가로), width(세로), channels(역자 주: 예를 들어 RGB)]`)로 나타낼 수 있습니다.
TensorFlow에서 graph는 연ㅅ나을 표현해놓은 것이라서 연ㅅ나을 하려면 graph가 `Session`상에 실행되어야 합니다. `Session`은 graph의 작업(op)(역자 주:operation. graph를 구서하는 노드)를 CPU나 GPU같은 `Device`에 배정하고 실행을 위한 메서드들을 제공합니다. 이런 메서드들은 작업(op)을 실행해서 tensor를 만들어 냅니다. Tensor는 파이썬에서 numpy(http://www.numpy.org/) `ndarry`형식으로 나오고 C와 C++에서는`TensorFlow::Tensor` 형식으로 나옵니다.

## 연산 graph(The computation graph)
TensorFlow 프로그램은 보통 graph를 조립하는 '구성 단계(construction phase)'와 session을 이용해 graph의 op을 실행시키는 '실행 단계(execution phase)'로 구성됩니다.

  예를 들어 뉴럴 네트워크를 표현하고 학습시키기 위해 구성 단계에는 graph를 만들고 실행 단계에서 graph의 훈련용 작업들(set of training ops)을 반복해서 실행합니다.
  Tensor 
