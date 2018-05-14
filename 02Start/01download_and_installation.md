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




### 소스에서 설치
#### 텐서플로우 레파지토리 클론(Clone)


## 텐서플로우 설치 테스트
### (선택사항, Linux) GPU 활성화
### 커맨드 라인에서 텐서플로우 실행하기
### 텐서플로우 데모 모델 실행
















# Reference
1. https://tensorflowkorea.gitbooks.io/tensorflow-kr/content/g3doc/get_started/os_setup.html
