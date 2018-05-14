# 다운로드와 셋업
텐서플로우는 바이너리 패키지나 깃허브 소스를 이용해 설치할 수 있습니다.

## 필요사항
텐서플로우 파이썬 API는 파이썬 2.7과 파이썬 3.3+을 지원합니다.

GPU  버전(아직 리눅스용만 있습니다)은 Cuda Toolkit 7.5과 cuDNN v4 와 가장 잘 작동합니다. 소스를 이용해 설치하면 다음 버전(Cuda Toolkit >= 7.0 과 cuDNN 6.5(v2), 7.0(v3), v5)도 사용할 수 있습니다. 자세한 내용은 `Cuda 설치` 부분을 참고해 주세요.

## 개요
여러가지 설치 방법을 지원하고 있습니다:
- `Pip 설치`: 이 방식으로 텐서플로우를 설치하거나 업그레이드할 때는 이 전에 작성했던 파이썬 프로그램에 영향을 미칠 수 있습니다.
- `Virtualenv 설치`: 텐서플로우를 각각의 디렉토리 안에 설치하므로 다른 프로그램에 영향을 미치지 않습니다.
-  `아나콘다(Anaconda) 설치`: 텐서플로우를 각 아나콘다 환경에 설치하므로 다른 프로그램에 영향을 미치지 않습니다.
- `도커(Docker) 설치`: 텐서플로우를 도커 컨테이너에서 실행하므로 컴퓨터의 다른 프로그램과 분리되어 운영됩니다.
- `소스에서 설치`: 텐서플로우를 pip wheel을 이용하여 빌드하고 설치합니다.

만약 Pip, Virtualenv, 아나콘다(Anaconda) 나 도커(Docker)를 잘 알고 있다면 필요에 맞게 설치 과정을 응용해도 좋습니다. pip 패키지 이름이나 도커 이미지 이름은 각 설치 섹션에 기재되어 있습니다.

설치시 에러가 발생하면 `자주 발생하는 문제`를 참고하세요.

### Pip 설치
`Pip`는 파이썬 패키지를 설치하고 관리하는 패키지 매니저 프로그램입니다.

설치되는 동안 추가되거나 업그레이드 될 파이썬 패키지 목록은 `setup.py 파일의 REQUIRED_PACKAGES 섹션`에 있습니다.

pip가 설치되어 있지 않다면 먼저 pip를 설치해야 합니다(python 3일 경우는 pip3).
```
# Ubuntu/Linux 64-bit
$ sudo apt-get install python-pip python-dev

# Mac OS X
$ sudo easy_install pip
$ sudo easy_install --upgrade six
```

적절한 텐서플로우 바이너리를 선택합니다:
```
# Ubuntu/Linux 64-bit, CPU 전용, Python 2.7
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.9.0-cp27-none-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU 버전, Python 2.7
# CUDA toolkit 7.5 와 CuDNN v4 필수. 다른 버전을 사용하려면 아래 "소스에서 설치" 섹션을 참고하세요.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.9.0-cp27-none-linux_x86_64.whl

# Mac OS X, CPU 전용, Python 2.7
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/tensorflow-0.9.0-py2-none-any.whl

# Ubuntu/Linux 64-bit, CPU 전용, Python 3.4
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.9.0-cp34-cp34m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU 버전, Python 3.4
# CUDA toolkit 7.5 와 CuDNN v4 필수. 다른 버전을 사용하려면 아래 "소스에서 설치" 섹션을 참고하세요.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.9.0-cp34-cp34m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, CPU 전용, Python 3.5
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpy/tensorflow-0.9.0-cp35-cp35m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU 버전, Python 3.5
# CUDA toolkit 7.5 와 CuDNN v4 필수. 다른 버전을 사용하려면 아래 "소스에서 설치" 섹션을 참고하세요.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.9.0-cp35-cp35-linux_x86_64.whl

# Mac OS X, CPU 전용, Python 3.4 or 3.5
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/tensorflow-0.9.0-py3-none-any.whl
```

텐서플로우를 설치합니다.
```
# Python 2
$ sudo pip install --upgrade $TF_BINARY_URL

# Python 3
$ sudo pip3 install --upgrade $TF_BINARY_URL
```

NOTE: 만약 텐서플로우 0.7.1 버전 이하에서 업그레이드하는 경우라면 protobuf 업데이트를 반영하기 위해 반드시 `pip install`을 사용하여 텐서플로우 이전 버전과 protobuf를 언인스톨한 후 진행해야 합니다.

### Virtualenv 설치
`Virtualenv`은 각기 다른 파이썬 프로젝트에서 필요한 패키지들의 버전이 충돌되지 않도록 다른 공간에서 운영되도록 하는 툴입니다. 텐서플로우를 Virtualenv 로 설치하면 기존 파이썬 패키지들을 엎어쓰지 않게됩니다.

`Virtualenv` 설치 과정은 다음과 같습니다.
- pip 와 Virtualenv 를 설치합니다.
- Virtualenv 환경을 만듭니다.
- Virtualenv 환경을 활성화 하고 그 안에서 텐서플로우를 설치합니다.
- 설치 후에는 텐서플로우를 사용하고 싶을 때마다 Virtualenv 환경을 활성화하면 됩니다.

pip 와 Virtualenv 를 설치합니다:
```
# Ubuntu/Linux 64-bit
$ sudo apt-get install python-pip python-dev python-virtualenv

# Mac OS X
$ sudo easy_install pip
$ sudo pip install --upgrade virtualenv
```

디렉토리 `~/tensorflow` 에 Virtualenv 환경을 만듭니다:
```
$ virtualenv --system-site-packages ~/tensorflow
```

환경을 활성화 합니다:
```
$ source ~/tensorflow/bin/activate       # bash를 사용할 경우
$ source ~/tensorflow/bin/activate.csh # csh을 사용할 경우
(tensorflow) $                                         # 프롬프트가 바뀌게 됩니다
```

이제 pip 설치 방식과 동일하게 텐서플로우를 설치합니다. 먼저 적절한 바이너리를 선택합니다:
```
# Ubuntu/Linux 64-bit, CPU 전용, Python 2.7
(tensorflow) $ export TF_BINARY_URL=https://storage/googleapis.com/tensorflow/linux/cpu/tensorflow-0.9.0-cp27-none-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU 버전, Python 2.7
# CUDA toolkit 7.5 와 CuDNN v4 필수. 다른 버전을 사용하려면 아래 "소스에서 설치" 섹션을 참고하세요.
(tensorflow) $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.9.0-cp27-none-linux_x86_64.whl

# Mac OS X, CPU 전용, Python 2.7
(tensorflow) $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/tensorflow-0.9.0-py2-none-any.whl

# Ubuntu/Linux 64-bit, CPU 전용, Python 3.4
(tensorflow) $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.9.0-cp34-cp34m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU 버전, Python 3.4
# CUDA toolkit 7.5 와 CuDNN v4 필수. 다른 버전을 사용하려면 아래 "소스에서 설치" 섹션을 참고하세요.
(tensorflow) $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.9.0-cp34-cp34m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, CPU 전용, Python 3.5
(tensorflow) $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.9.0-cp35-cp35m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU 버전, Python 3.5
# CUDA toolkit 7.5 와 CuDNN v4 필수. 다른 버전을 사용하려면 아래 "소스에서 설치" 섹션을 참고하세요.
(tensorflow) $ export TF_BINARY_URL=https://storage/googleapis.com/tensorflow/linux/gpu/tensorflow-0.9.0-cp35-cp35m-linux_x86_64.whl

# Mac OS X, CPU 전용, Python 3.4 or 3.5
```

마지막으로 텐서플로우를 설치합니다:
```
# Python 2
(tensorflow) $ pip install --upgrade $TF_BINARY_URL

# Python 3
(tensorflow) $ pip3 install --upgrade $TF_BINARY_URL
```

Virtualenv 활성화하고 `설치 테스트`를 할 수 있습니다.

텐서플로우 작업을 마쳤을 때에는 환경을 비활성화 합니다.
```
(tensorflow) $ deactivate

$ # 프롬프트가 원래대로 되돌아 옵니다
```

나중에 텐서플로우를 다시 사용하려면 Virtualenv 환경을 다시 활성화해야 합니다:
```
$ source ~/tensorflow/bin/activate # bash를 사용할 경우
$ source ~/tensorflow/bin/activate.csh # csh을 사용할 경우
(tensorflow) $ # 프롬프트가 변경되었습니다
# 텐서플로우를 사용한 프로그램을 실행시킵니다
...
# 텐서플로우 사용을 마쳤을 때에는 환경을 비활성화 합니다
(tensorflow) $ deactivate
```


### Anaconda 설치
`Anaconda`는 여러 수학, 과학 패키지를 기본적으로 포하하고 있는 파이썬 배포판입니다. Anaconda는 "conda"로 불리는 패키지 매니저를 사용하여 Virtualenv와 유사한 `환경 시스템`을 제공합니다. (역주: 텐서플로우 뿐만이 아니라 일반적인 데이터 사이언스를 위해서도 아나콘다를 추천합니다)

Virtualenv처럼 conda 환경은 각기 다른 파이선 프로젝트에서 필요한 패키지들의 버전이 충돌되지 않도록 다른 공간에서 운영합니다. 텐서플로우를 Anaconda 환경으로 설치하면 기존 파이썬 패키지들을 덮어쓰지 않게됩니다.
- Anaconda를 설치합니다.
- conda 환경을 만듭니다.
- conda 환경을 활성화 하고 그 안에 텐서플로우를 설치합니다.
- 설치 후에는 텐서플로우를 사용하고 싶을 때마다 conda 환경을 활성화하면 됩니다.

Anaconda를 설치합니다:
`Anaconda 다운로드 사이트`의 안내를 따릅니다.

`tensorflow` 이름을 갖는 conda 환경을 만듭니다:
```
# Python 2.7
# conda create -n tensorflow python=2.7

# Python 3.4
# conda create -n tensorflow python=3.4

# Python 3.5
$ conda create -n tensorflow python=3.5
```

환경을 활성화시키고 그 안에서 pip를 이용하여 텐서플로우를 설치합니다. `easy_install` 관련한 에러를 방지하려면 `--ignore-installed` 플래그를 사용합니다.
```
$ source activate tensorflow
```

이제 pip 설치 방식과 동일하게 텐서플로우를 설치합니다. 먼저 적절한 바이너리를 선택합니다:
```
# Ubuntu/Linux 64-bit, CPU 전용, Python 2.7
(tensorflow) $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.9.0-cp27-none-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU 버전, Python 2.7
# CUDA toolkit 7.5 와 CuDNN v4 필수. 다른 버전을 사용하려면 아래 "소스에서 설치" 섹션을 참고하세요.
(tensorflow) $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.9.0-cp27-none-linux_x86_64.whl

# Mac OS X, CPU 전용, Python 2.7
(tensorflow) $ export TF_BINARY_URL=htps://storage.googleapis.com/tensorflow/mac/tensorflow-0.9.0-py2-none-any.whl

# Ubuntu/Linux 64-bit, CPU 전용, Python 3.4
(tensorflow) $ TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.9.0-cp34-cp34m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU 버전, Python 3.4
# CUDA toolkit 7.5 와 CuDNN v4 필수. 다른 버전을 사용하려면 아래 "소스에서 설치" 섹션을 참고하세요.
(tensorflow) $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.9.0-cp34-cp34m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, CPU 전용, Python 3.5
(tensorflow) $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.9.0-cp35-cp35m-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU 버전, Python 3.5
# CUDA toolkit 7.5 와 CuDNN v4 필수. 다른 버전을 사용하려면 아래 "소스에서 설치" 섹션을 참고하세요.
(tensorflow) $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.9.0-cp35-cp35m-linux_x86_64.whl

# Mac OS X, CPU 전용, Python 3.4 or 3.5
(tensorflow) $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/tensorflow-0.9.0-py3-none-any.whl
```

마지막으로 텐서플로우를 설치합니다:
```
# Python 2
(tensorflow) $ pip install --upgrade $TF_BINARY_URL

# Python 3
(tensorflow) $ pip3 install --upgrade $TF_BINARY_URL
```

conda 활성화하고 `설치 테스트`를 할 수 있습니다.

텐서플로우 작업을 마쳤을 때에는 환경을 비활성화 합니다.
```
(tensorflow) $ source deactivate

$ # Your prompt should change back
```

나중에 텐서플로우를 다시 사용하려면 conda 환경을 다시 활성화해야 합니다:
```
$ source activate tnesorflow
(tensorflow) $ # 프롬프트가 바뀌었습니다
# 텐서플로우를 사용한 프로그램을 실행시킵니다
...
# 텐서플로우 사용을 마쳤을 때에는 환경을 비활성화 합니다
(tensorflow) $ source deactivate
```

### 도커(Docker) 설치
`Docker`는 로컬 컴퓨터에서 컨테이너로 리눅스 운영체제를 운영할 수 있는 시스템입니다. 도커를 사용하여 텐서플로우를 설치하고 사용한다면 이는 로컬 컴퓨터의 패키지와 완전히 분리된 것 입니다.

네개의 도커 이미지가 제공됩니다:
- `gcr.io/tensorflow/tensorflow`: TensorFlow CPU 바이너리 이미지.
- `gcr.io/tensorflow/tensorflow:latest-devel`: CPU 바이너리 이미지와 소스 코드.
-`gcr.io/tensorflow/tensorflow:latest-gpu`: TensorFlow GPU 바이너리 이미지.
- `gcr.io/tensorflow/tensorflow:latest-devel-gpu`: GPU 바이너리 이미지와 소스 코드.

최근 릴리즈는 버전대신 `latest` 태그를 표시합니다(예, `0.9.0-gpu`).

도커 이용한 설치는 아래와 같습니다:
- 로컬 컴퓨터에 도커를 설치합니다.
- `sudo` 없이 컨테이너를 시작할 수 있도록 `도커 그룹` 을 만듭니다.
- 텐서플로우 이미지로 도커 컨테이너를 시작합니다. 처음 시작할 때 자동으로 이미지를 다운로드합니다.

로컬 컴퓨터에 도커를 설치하는 설명은 `도커 설치` 를 참고하세요.

도커가 설치되면 텐서플로우 바이너리 이미지로 아래와 같이 도커 컨테이너를 실행합니다.
```
$ docker run -it -p 8888:8888 gcr.io/tensorflow/tnesorflow
```

옵션 `-p 8888:8888` 은 로컬(호스트) 컴퓨터가 도커 컨테이너로 접속할 수 있는 포트를 지정합니다. 여기서는 쥬피터(Jupyter) 노트북 연결을 위한 포트입니다.

포트를 매핑하는 형식은 `호스트포트:컨테이너포트` 입니다. 컨테이너 포트 `8888`에 대한 호스트 포트틑 임의의 포트를 지정할 수 있습니다.

NVidia GPU를 위해서는 최신 NVidia 드라이버와 `nvidia-docker`를 설치하고 아래와 같이 실행합니다.
```
$ nvidia-docker run -it -p 8888:8888 gcr.io/tensorflow/tensorflow:latest-gpu
```

더 자세한 것은 `텐서플로우 도커` 문서를 참고하세요.

도커 컨테이너 안에서 `설치 테스트`를 할 수 있습니다.

#### 텐서플로우 설치 테스트
##### (선택사항, Linux) GPU 활성화
텐서플로우 GPU 버전을 설치했다면 반드시 CUDA Toolkit 7.5 와 CuDNN v4 도 설치해야 합니다. `CUDA  설치`을 참고하세요.

`LD_LIBRARY_PATH` 와 `CUDA_HOME` 환경 변수를 지정해야 합니다. 아래 명령을 `~/.bash_profile` 파일에 추가하는 것이 좋습니다. 이 명령은 `/usr/local/cuda` 에 CUDA가 설치되어있다고 가정한 것입니다:
```
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64"
export CUDA_HOME=/usr/local/cuda
```


##### 커맽드 라인에서 텐서플로우 실행하기
에러가 발생하면 `자주 발생하는 문제` 섹션을 참고하세요.

터미널을 열고 아래 명령을 실행합니다:
```
$ python
...
>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> print(sess.run(hello))
Hello, TensorFlow!
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> print(sess.run(a + b))
42
>>>
```

##### 텐서플로우 데모 모델 실행
데모 모델을 포함해 텐서플로우의 모든 패키지는 파이썬 파이브러리로 설치되어 있습니다. 파이썬 파이브러리의 정확한 경로는 설치된 시스템마다 다릅니다. 하지만 보통 아래 중에 하나일 것입니다:
```
/usr/local/lib/python2.7/dist-packages/tensorflow
/usr/local/lib/python2.7/site-packages/tensorflow
```

아래 명령으로 정확한 디렉토리를 찾을 수 있습니다(텐서플로우를 설치한 파이썬을 사용해야 합니다. 예를 들면 파이썬 3에서 텐서플로우를 설치했다면 `python` 대신 `python3`  를 사용해야 합니다):
```
$ python -c 'import os; import inspect; import tensorflow; print(os.path.dirname(inspect.getfile(tensorflow)))'
```

MNIST 데이터셋을 이용한 손글씨 숫자를 분류하는 간단한 데모 모델은 `models/image/mnist/convolutional.py`에 있습니다. 커맨드라인에서 다음과 같이 실행시킬 수 있습니다(텐서플로우를 설치한 파이썬인지 확인하세요):
```
# 파이썬 검색 범위에서 프로그램을 찾기 위해서 `python -m` 명령을 이용합니다:
$ python -m tensorflow.models.image.mnist.convolutional
Extracting data/train-images-idx3-ubyte.gz
Extracting data/train-labels-idx1-ubyte.gz
Extracting data/t10k-images-idx3-ubyte.gz
Extracting data/t10k-labels-idx1-ubyte.gz
...etc...

# 파이썬 인터프리터에 모델 프로그램의 파일 경로를 전달할 수 있습니다.
# (텐서플로우가 설치된 파이썬 버전을 사용해야 합니다.
# 예를 들어, 파이썬 3의 경우는 .../python3.X/... 가 됩니다.).
$ python /usr/local/lib/python2.7/dist-packages/tensorflow/models/image/mnist/convolutional.py
...
```


### 소스에서 설치
소스에서 설치하려면 pip를 사용해서 진행할 수 있도록 pip 휠(wheel)을 만듭니다. pip를 설치하려면 `Pip 설치` 섹션을 참고하세요.

#### 텐서플로우 레파지토리 클론(Clone)
```
$ git clone https://github.com/tensorflow/tensorflow
```

아래 방법은 최신 마스터 브랜치의 텐서플로우를 설치하는 것입니다. 만약 특정 브랜치(릴리즈 브랜치 같은)를 설치하고 싶다면 `git clone` 명령에 `-b <branchname>` 옵션을 추가하고 r0.8 과 그 이전 버전에서는 protobuf 라이브러리를 추가하기 위해 `--recurse-submodules` 옵션을 추가합니다.

#### 리눅스 설치
##### Bazel 설치
Bazel에 필요한 소프트웨어를 `여기`를 따라 설치합니다. `자신의 컴퓨터에 맞는 인스톨러`를 사용하여 최신 안정버전의 bazel을 다운로드 하여 아래와 같이 실행합니다:
```
$ chmod +x PATH_TO_INSTALL.SH
$ ./PATH_TO_INSTALL.SH --user
```

`PATH_TO_INSTALL.SH` 부분을 다운로드 받은 인스톨러의 경로를 바꾸어 줍니다.

마지막으로 실행 경로에 `bazel`을 추가하기 위해 화면의 설명을 따릅니다.

##### 다른 의존성 라이브러리 설치
```
# Python 2.7:
$ sudo apt-get install python-numpy swig python-dev python-wheel

# Python 3.x:
$ sudo apt-get install python3-numpy swig python3-dev python3-wheel
```

##### 설치환경 설정
루트 디렉토리에 있는 `configure` 스크립트를 실행합니다. 환경설정 스크립트는 파이썬 인터프리터의 경로를 요청하고 (선택사항으로)CUDA 라이브러리를 설정합니다(`아래`를 참고하세요).

이 단계에서는 파이썬과 넘파이(numpy) 헤더파일을 찾습니다.
```
$ ./configure
$ Please specify the location of python. [Default is /usr/bin/python]:
```

##### 선택사항: CUDA 설치 (리눅스 GPU)
GPU 버전의 텐서플로우를 설치하고 실행하기 위해서는 엔비디아(NVIDIA)의 쿠다 툴킷(CUDA Toolkit)(>=7.0)과 CuDNN(>= v2)을 설치해야 합니다.

텐서플로우 GPU 버전은 엔비디아(NVidia)의 Compute Capability >= 3.0 이상을 지원하는 GPU 카드를 필요로 합니다. 지원되는 카드는 아래 목록을 포함하고 있습니다:
- NVidia Titan
- NVidia Titan X
- NVidia K20
- NVidia K40

**GPU 카드의 NVIDIA Compute Capability 체크**
https://developer.nvidia.com/cuda-gpus

**CUDA Toolkit 다운로드 및 설치**
https://developer.nvidia.com/cuda-downloads

텐서플로우의 바이너리 릴리즈를 사용하려면 버전 7.5를 설치하세요

`/usr/local/cuda` 등에 툴킷을 설치합니다.

**CuDNN 다운로드 및 설치**
https://developer.nvidia.com/cudnn

CuDNN v4를 다운로드합니다(v5는 현재 릴리즈 후보 상태로 텐서플로우를 소스에서 설치할 때만 사용할 수 있습니다).

압축을 풀어 툴킷 디렉토리에 CuDNN 파일을 복사합니다. `/usr/local/cuda`에 툴킷이 설치되어 있다고 가정하고 아래 명령을 실행합니다(다운로드 받은 CuDNN의 적절한 버전을 반영해 주세요):
```
tar xvzf cudnn-7.5-linux-x64_v4.tgz
sudo cp cudnn-7.5-linux-x64-v4/cudnn.h /usr/local/cudnn/include
sudo cp cudnn-7.5-linux-x64-v4/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

**텐서플로우에서 CUDA 라이브러리 선택**
소스 디렉토리의 맨 위에서 `configure` 스크립트를 실행하고 텐서플로우 GPU 지원하도록 빌드할 지 물어볼 때 `Y`를 선택하세요. 만약 여러가지 버전의 CUDA와 CuDNN이 설치되어 있다면 디폴트 대신 구체적으로 어떤 버전을 사용할지 지정해야 합니다. 아래와 같은 질문들을 보게됩니다.
```
$ ./configure
Please specify the location of python. [Default is /usr/bin/python]:
Do you with to build TensorFlow with GPU support? [y/N] y
GPU support will be enabled for TensorFlow

Please specify which gcc nvcc should use a sthe host compiler.
[Default is /usr/bin/gcc]: /usr/bin/gcc-4.9

Please specify the CUDA SDK version you want to use, e.g. 7.0.
[Leave empty to use system default]: 7.5

Please specify the location where CUDA 7.5 toolkit is installed.
Refer to README.md for more details. [default is: /usr/local/cuda]: /usr/local/cuda

Please specify the Cudnn version you want to use.
[Leave empty to use system default]: 4.0.4

Please specify the location where the cuDNN 4.0.4 library is installed.
Refer to README.md for more details.
[default is: /usr/local/cuda]: /usr/local/cudnn-r4-rc/

Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at:
https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your
build time and binary size. [Default is: "3.5,5.2"]: 3.5

Setting up Cuda include
Setting up Cuda lib64
Setting up Cuda bin
Setting up Cuda nvvm
Setting up CUPTI include
Setting up CUPTI lib64
Configuration finished
```

시스템에는 있는 CUDA 라이브러리를 가리키는 기본으로 사용할 심볼릭 링크들을 만듭니다. Bazel 빌드 명령을 실행하기 전에 CUDA 라이브러리 경로를 바꾸게 되면 이 단계를 다시 거쳐야 합니다. CuDNN 라이브러리 R2 sms '6.5'를 R3는 '7.0'을 R4-RC는 '4.0.4'를 선택합니다.

**GPU를 지원하도록 빌드하기**
소스 트리 맨 위에서 실행:
```
$ bazel build -c opt --config=cuda //tensorflow/cc:tutorials_example_trainer

$ bazel-bin/tensorflow/cc/tutorials_example_trainer --use_gpu
# 많은 출력이 나옵니다. 이 튜토리얼은 GPU에서 2x2 행렬의 고유값을 반복해서 계산합니다.
# 마지막 몇 줄은 아래와 같습니다.
000009/000005 lambda = 2.00000 x = [0.894427 -0.447214] y = [1.788854 -0.894427]
000006/000001 lambda = 2.00000 x = [0.894427 -0.447214] y = [1.788854 -0.894427]
000009/000009 lambda = 2.00000 x = [0.894427 -0.447214] y = [1.788854 -0.894427]
```

GPU 지원을 활성화하기 위해서는 "--config=cuda" 옵션이 필요합니다.

**알려진 이슈**
- 하나의 소스 트리에서 CUDA 와 non-CUDA 두가지 설정으로 모두 빌드가 가능하지만 설정을 바꾸려면 `bazel clean` 을 실행해 주세요.
- bazel 빌드를 하기 전에 환경 설정을 해야 합니다. 그렇지 않으면 빌드가 실패합니다. 향후에는 빌드 프로세스 안에 환경 설정 단계를 포함시켜 좀 더 편리하게 만드려고 생각하고 있습니다.


##### Mac OS X 설치
bazel과 SWIG 설치를 위해서는 `homebrew`를 사용하고 easy_install 이나 pip를 사용하여 파이썬 라이브러리를 설치하길 권장합니다.

물론 homebrew를 사용하지 않고 소스에서 Swig를 설치할 수도 있습니다. 그런 경우에 의존성 라이브러리인 `PCRE`를 설치해야 합니다. PCRE2가 아닙니다.

###### 의존성 라이브러리
bazel의 의존성 라이브러리를 설치하려면 `이곳`의 안내를 따르세요. bazel과 SWIG 설치를 위해 homebrew를 사용할 수 있습니다:
```
$ brew install bazel swig
```

easy_install이나 pip를 사용하여 파이썬 의존성을 설치할 수 있습니다. easy_install을 사용할 경우 아래를 실행합니다
```
$ sudo easy_install -U six
$ sudo easy_install -U numpy
$ sudo easy_install wheel
```

기능이 강화된 파이썬 쉘인 `ipython`을 권장합니다. 다음과 같이 설치합니다:
```
$ sudo easy_install ipython
```

GPU 지원이 되도록 빌드하려면 homebrew를 사용해 GNU coreutils가 설치되어 있어야 합니다:
```
$ brew install coreutils
```

다음은 `NVIDIA` 사이트에서 OSX 버전에 맞는 패키지를 다운로드 하거나 `Homebrew Cask` 확장을 사용하여 최신의 `CUDA Toolkit`을 설치해야 합니다:
```
$ brew tap caskroom/cask
$ brew cask install cuda
```

CUDA 툴킷을 설치하면 필요한 환경 변수를 `~/.bash_profile` 파일에 아래와 같이 셋팅해야 합니다:
```
export CUDA_HOME=/usr/local/cuda
export DYLD_LIBRARY_PATH="$DYLD_LIBRARY_PATH:$CUDA_HOME/lib"
export PATH="$CUDA_HOME/bin:$PATH"
```

마지막으로 `Accelerated Computing Developer Program` 계정이 필요한 `CUDA Deep Neural Network` (cuDNN)를 설치할 수도 있습니다. 로컬 컴퓨터에 다운로드 받고 난 후 압축을 풀고 헤더 파일과 라이브러리를 CUDA 툴킷 폴더에 옮깁니다:
```
$ sudo mv include/cudnn.h /Developer/NVIDIA/CUDA-7.5/include/
$ sudo mv lib/libcudnn* /Developer/NVIDIA/CUDA-7.5/lib
$ sudo ln -s /Developer/NVIDIA/CUDA-7.5/lib/libcudnn* /usr/local/cuda/lib/
```

###### 설치환경 설정
소스 트리 맨 위에서 `configure` 명령을 실행합니다. 이 스크립트는 파이썬 인터프리터의 경로를 묻습니다.

이 단계에서 CUDA와 툴킷이 설치되어 있을 때 GPU 지원을 활성화 하는 것은 물론 파이썬과 numpy 헤더 파일들의 위치를 찾습니다. 예를 들면:

```
$ ./configure
Please specify the location of python. [Default is /usr/bin/python]:
Do you with to build TnesorFlow with Google Cloud Platform support? [y/N] N
No Google Cloud Platform support will be enabled for TensorFlow
Do you with to build TensorFlow with GPU support? [y/N] y
GPU support will be enabled for TensorFlow
Please specify which gcc nvcc should use as the host compiler. [Default is /usr/bin/gcc]:
Please specify the Cuda SDK version you want to use, e.g. 7.0. [Leave empty to use system default]: 7.5
Please specify the location where CUDA 7.5 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:
Please specify the Cudnn versio you want to use. [Leave empty to use system default]: 5
Please specify the location where cuDNN 5 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:
Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size.
[Default is: "3.5.5.2"]: 3.0
Setting up Cuda include
Setting up Cuda lib
Setting up Cuda bin
Setting up Cuda nvvm
Setting up CUPTI include
Setting up CUPTI lib64
Configuration finished
```

###### pip 패키지 생성 및 설치
소스에서 설치할 때 pip 패키지를 만들고 설치해야 합니다.
```
$ bazel build -c opt //tensorflow/tools/pip_package:build_pip_package

# To build with GPU support:
$ bazel build -c opt --config=cuda //tensorflow/tools/pip_package:build_pip_package

$ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg

# The name of the .whl file will depend on your platform.
$ sudo pip install /tmp/tensorflow_pkg/tensorflow-0.9.0-py2-none-any.whl
```

###### 텐서플로우 개발자 셋팅
텐서플로우 자체를 수정할 때 텐서플로우를 재 설치하지 않고 파이썬 대화식 쉘에서 변경 내용을 테스트할 수 있다면 매우 유용할 것입니다.

모든 파일이 시스템 디렉토리로 부터 링크(복사가 아닌)되도록 텐서플로우를 셋팅하기 위해서는 텐서플로우 루트 디렉토리에서 다음 명령을 실행합니다:
```
bazel build -c opt //tensorflow/tools/pip_package:build_pip_package

# To build with GPU support:
bazel build -c opt --configu=cuda //tensorflow/tools/pip_package:build_pip_package

mkdir _python_build
cd _python_build
ln -s ../bazel-bin/tensorflow/tools/pip_package/build_pip_package.runfiles/org_tensorflow/* .
ln -s ../tensorflow/tools/pip_package/* .
python setup.py develop
```

C++ 파일을 변경하거나 어떤 파이썬 파일이든지 추가, 삭제, 이동될 때 혹은 bazel 빌드 룰을 바꿀 때마다 `//tensorflow/tools/pip_package:build_pip_package`을 다시 빌드해야 합니다.

###### 텐서플로우로 첫번째 뉴럴 네트워크 모델을 학습 시키기
소스 트리 루트에서 실행합니다:
```
$ cd tensorflow/models/image/mnist
$ python convolutional.py
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting data/train-images-idx3-ubyte.gz
Extracting data/train-labels-idx1-ubyte.gz
Extracting data/t10k-images-idx3-ubyte.gz
Extracting data/t10k-labels-idx1-ubyte.gz
Initialized!
Epoch 0.00
Minibatch loss: 12.054, learning rate: 0.010000
Minibatch error: 90.6%
Validation error: 84.6%
Epoch 0.12
Minibatch loss: 3.285, learning rate: 0.010000
Minibatch error: 6.2%
Validation error: 7.0%
...
...
```

#### 자주 발생하는 문제
##### GPU 관련 이슈들
텐서플로우 프로그램을 실행할 때 다음과 같은 에러를 만날 경우:
```
ImportError: libcudart.so.7.0: cannot open shared object file: No such file of directory
```

GPU 설치 `가이드`를 따랐는지 확인하세요. 소스에서 설치할 때 CUDA나 CuDNN 버전은 비워둔 채 진행했다면 명시적으로 지정하여 다시 시도해 보셍.

##### Protobuf 라이브러리 관련 이슈들
텐서플로우 pip패키지는 protobuf pip 패키지 버전 3.0.0b2를 필요로 합니다. `PyPI`에서 다운받을 수 있는(`pip install protobuf`를 사용해서) Protobuf의 pip 패키지는 파이썬 만으로 개발된 라이브러리로 C++ 구현보다 직렬화/역직렬화시 10~50배 느립니다. Protobuf는 빠른 프로토콜 파싱을 위한 C++ 바이너리 확장을 지원합니다. 이 확장은 표준 파이썬 PIP 패키지에는 포함되어 있지 않습니다. 우리는 이 바이너리 확장을 포함한 protobuf pip 패키지를 자체적으로 만들었습니다. 다음 명령을 사용해 자체적으로 만든 protobuf pip 패키지를 설치할 수 있습니다:
```
# Ubuntu/Linux 64-bit:
$ pip install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/protobuf-3.0.0b2.post2-cp27-none-linux_x86_64.whl

# Mac OS X:
$ pip install --upgrade https://storage.googleapis.com/tensorflow/mac/protobuf-3.0.0b2.post2-cp27-none-any.whl
```

Python 3  에서는 :
```
# Ubuntu/Linux 64-bit:
$ pip3 install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/protobuf-3.0.0b2.post2-cp34-none-linux_x86_64.whl

# Mac OS X:
$ pip3 install --upgrade https://storage.googleapis.com/tensorflow/mac/protobuf-3.0.0b2.post2-cp35-none-any.whl
```

`pip install tensorflow` 명령은 파이썬으로된 기본 pip 패키지를 설치하므로 위 패키지를 설치하려면 반드시 텐서플로우를 설치하고 난 후에 합니다. 위 pip 패키지는 이미 설치된 protobuf 패키지를 덮어 씁니다. 바이너리 pip 패키지는  64M 넘는 메세지에 대한 지원을 이미 하고 있어 아래와 같은 에러가 이미 해결 되었습니다:
```
[libprotobuf ERROR google/protobuf/src/google/protobuf/io/coded_stream.cc:207] A
protocol message was rejected because it was too big (more than 67107764 bytes).
To increase th limit (or to disable these warnings), ses
CodedInputStream::SetTotalBytesLimit() in google/protobuf/io/coded_stream.h.
```

#### Pip 설치 이슈들
**Cannot import name 'descriptor'**
```
ImportError: Trackback (most recent call last):
    File "/usr/local/lib/python3.4/dist-packages/tensorflow/core/framework/graph_pb2.py", line 6, in <module>
        from google.protobuf import descriptor as _descriptor
ImportError: cannot import name 'descriptor'
```

최신 버전의 텐서플로우로 업그레이드할 때 위와 같은 에러가 발생하면 텐서플로우와 protobuf를 모두 언인스톨하고 텐서플로우를 다시 재설치합니다(올바른 protobuf 의존성을 찾기 위해서).

**Can't find setup.py**
`pip install`하는 동안 아래와 같은 에러를 만나면:
```
...
IOError: [Errno 2] No such file or directory: '/tmp/pip-o6Tpui-build/setup.py'
```

해결책: pip를 업그레이드 합니다:
```
pip install --upgrade pip
```

pip 설치가 어떻게 되어 있느냐에 따라 `sudo`를 필요로 할지 모릅니다.

**SSLError: SSL_VERIFY_FAILED**
URL로 부터 pip 인스톨을 하는 동안 아래와 같은 에러를 만나면:
```
...
SSLError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed
```

해결책: curl이나 wget으로 수동으로 wheel을 다운로드 받아 로컬에서 pip install 합니다.

**Operation not permitted**
`sudo`를 사용함에도 아래와 같은 에러를 만나면:
```
...
Installing collected packages: setuptools, protobuf, wheel, numpy, tensorflow
Found existing installation: setuptools 1.1.6
Uninstalling setuptools-1.1.6:
Exception:
...
[Errno 1] Operation not permitted: '/tmp/pip-a1DXRT-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/_markerlib'
```

해결책: pip 명령에 `--ignore-installed` 플래그를 추가합니다.

##### Linux 이슈들
이런 에러가 나오면:
```
...
    "__add__", "__radd__",
                ^
SyntaxError: invalid syntax
```

해결책: 파이썬 2.7을 사용하고 있느지 확인합니다.


##### Mac OS X: ImportError: No module named copyreg
Mac OS X에서 텐서플로우를 임포트할 때 아래와 같은 에러가 나올 수 있습니다.
```
>>> import tensorflow as tf
...
ImportError: No module named copyreg
```

해결책: 텐서플로우는 `six-1.10.0 `파이썬 패키지를 필요로하는 protobuf에 의존성이 있습니다. 애플의 기본 파이썬 설치에는 `six-1.4.1`이 제공됩니다.

다음과 같은 방법으로 이를 해결 할 수 있습니다:
- 최신 버전의 `six`로 업그레이드 합니다:
```
$ sudo easy_install -U six
```

- 별도의 파이썬 환경에서 텐서플로우를 설치합니다:
-- `Virtualenv` 사용.
-- `Docker`사용.
- `Homebrew`나 `MacPorts`를 사용하여 별도의 파이썬 버전을 설치한 후 텐서플로우를 그 파이썬에서 재 설치합니다.

#### 텐서플로우 레파지토리 클론(Clone)


## 텐서플로우 설치 테스트
### (선택사항, Linux) GPU 활성화
### 커맨드 라인에서 텐서플로우 실행하기
### 텐서플로우 데모 모델 실행
















# Reference
1. https://tensorflowkorea.gitbooks.io/tensorflow-kr/content/g3doc/get_started/os_setup.html
