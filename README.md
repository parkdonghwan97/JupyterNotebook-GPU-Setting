# JupyterNotebook-GPU-Setting

- 필자의 gpu는 RTX3060 & RTX3060ti


# 딥러닝을 위해서는 CPU로는 속도의 한계가 분명 존재한다.
- 따라서 tensorflow-gpu 사용을 해야하며 이를 위해 로컬에 설치해야할 파일들이 존재한다. 
- 특정 블로그를 보며 헤메었지만 다른 사람들은 조금이라도 쉽게 GPU를 사용할 수 있도록 하기 위해 해당 레포지터리를 생성했다.

```
# 현재 사용중인 로컬의 장지 확인 
from tensorflow.python.client import device_lib
print(device_lib.list_local_devices())
```

아무것도 설치 하지 않았다면 CPU만 보일 텐데 밑의 과정을 하나씩 해결한 후 다시 코드를 입력해보면 GPU가 잡히는 것을 알 수 있을 것이다. 




# 1. NVIDIA GPU 버전 확인.    
- 장치관리자 - 디스펠레이 어댑터 or 작업관리자 - 성능에서 자신의 컴퓨터의 GPU를 확인 할 수 있다. 
- tensor flow-gpu 설치를 위해 기본적으로 준비해야 하는 파일이 있는데
- [NVIDIA제어판](https://www.nvidia.co.kr/Download/index.aspx?lang=kr), [Visual Studio](https://visualstudio.microsoft.com/ko/downloads/) 는 기본적인 설치가 되어있다고 가정하고 작성하겠다.


# 2. 쿠다 툴킷 설치
- 자신의 GPU에 맞는 쿠다 버전을 확인해야한다.[링크](https://www.wikiwand.com/en/CUDA#/GPUs_supported)   
- RTX3060 & RTX3060TI 기준 CUDA SDK 11.1 – 11.6 support for compute capability 3.5 – 8.6 (Kepler (in part), Maxwell, Pascal, Volta, Turing, Ampere


- [쿠다 툴킷 설치[(https://developer.nvidia.com/cuda-toolkit-archive) 필자는 11.2를 받았다.
- 설치가 완료된다면 C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2 경로가 생길것이다.

![image](https://user-images.githubusercontent.com/64680900/162017225-2acac80a-82f9-4959-890b-1ad40d554e96.png)
- 위와같은 오류가 뜬다면 제어판에서 기존 CUDA Kit을 전부 삭제하고 재설치 하기 바란다.

# 3. CUDA 버전에 맞는 cuDNN설치할 것. 
![image](https://user-images.githubusercontent.com/64680900/162017884-5e6bbac3-1575-4393-8239-5c237cc57ebd.png)
- [RTX3060 & RTX3060ti 기준 cuDNN v8.1.1](https://developer.nvidia.com/rdp/cudnn-archive)

# 4. cuDNN 받은 압축파일 내용을 ↓ 밑의 경로에 덮어쓴다.
- C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2 
- ( bin, include, lib과 txt파일 하나가 있을 것이다.)

# 5.환경변수 확인

![image](https://user-images.githubusercontent.com/64680900/162018578-8883a6d2-acd3-4cad-8d63-6d1001168cbe.png)
![image](https://user-images.githubusercontent.com/64680900/162018644-45f1d8b7-465f-4dab-8855-3cb577395409.png)
 
![image](https://user-images.githubusercontent.com/64680900/162018678-90e0d7f9-df07-4593-99b6-bd14d9b9cf2e.png)
- 시스템 변수 및 사용자 환경 변수 PATH 설정
- 시스템 변수
![image](https://user-images.githubusercontent.com/64680900/162019367-0e91536e-0a68-4fb3-935f-c25a94e4e9ef.png)
- C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2

- 사용자 변수
![image](https://user-images.githubusercontent.com/64680900/162019442-e39e917c-f605-4fc8-be83-4c19d7737a37.png)
- C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2\include
- C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2\lib
- C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2\bin


## + 가상환경 세팅

```
conda create -n 가상환경이름 python=3.7
conda activate 가상환경이름
pip install jupyter notebook
pip install tensorflow-gpu==2.6.0
pip install keras  # conda로 설치하게되면 버전이 2.4가 설치되는 것 같음. 주의바람.
python -m ipykernel install --user --name [가상환경이름]

jupyter notebook # 실행
```

# 6. 설치 확인
```
from tensorflow.python.client import device_lib
print(device_lib.list_local_devices())
```
![image](https://user-images.githubusercontent.com/64680900/162020343-588f49ed-5510-4ec8-a230-bb4182e00df9.png)
정상적으로 GPU가 잡힘을 알 수 있다.



## 기타 오류?
- 앞서 언급하였듯이 conda install로 설치한 사람은 텐서플로우와 케라스 버전이 2.6 매칭이 안되었을 가능서이 높다.

- 설치완료하고 GPU까지 잡혔는데 CNN이나 VGG 모델링 등 딥러닝 모델을 돌리다가 커널이 죽는 경우가 있다.
![image](https://user-images.githubusercontent.com/64680900/162020758-f146371c-616d-4705-8147-91505499dcb1.png)

이는 메모리 할당 초과 때문인데

- C드라이브/사용자/사용자이름/.jupyter폴더에 들어가보면
- jupyter_notebook.config.py 파이썬 파일이 있을 것이다.
![image](https://user-images.githubusercontent.com/64680900/162021214-91c078cf-3416-458c-a4ca-f5ff7e1a3c87.png)
- 해당 파일에서 버퍼 사이즈를  (max_buffer_size = 100000000000) 높여주면 된다.


-끝-
