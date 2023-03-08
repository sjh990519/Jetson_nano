# Jetson_nano
### Jetson Nano Developer Kit 2G + YOLOv3

<br><br><br>

## :hammer: 프로젝트 소개
Jetson nano 보드에서 darknet ros 를 활용한 Object Detection(yolo)를 실행하는 프로젝트

<br>


## ⚙️ 개발환경

### Jetson Nano 
|   **Title** |   **Description**   |
|:--------    |       :-------------|
|OS           |      |
|Jetpack      |0.0.0 version      |
|ROS          |Noetic Ninjemys    |
|OpenCV       |0.0.0 version      |
|Python       |0.0.0 version      |
|CUDA         |0.0.0 version      |
|cuDNN        |0.0.0 version      |

---

<br><br><br>


## :pushpin: Install

<br>

### 1. jetson-stats
- jetson-stats는 Jetson Nano의 종합적인 Stat을 기존의 수많은 터미널 명령어로 확인해야 했던 것들을 TUI(Text User Interface)형태로 편리하게 이용이 가능하다.
- **"CPU, GPU,RAM,Swap,Fan 등의 H/W 정보 및 CUDA,TensorRT,JetPack,Python 버전확인이 가능하다."**
```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install python-pip
$ sudo -H pip install -U jetson-stats
$ sudo reboot

# jetson-stats 실행
$ jtop
```

<br>

### 2. gdm3 Purge & lightdm Install
- Jetson Nano는 lightdm 환경에서 YOLO를 실행 시 램 사용량이 2GB이상 사용된다. Swap도 200MB정도 차지한다. 
- 또한, 이후 OpenCV 빌드를 위해 Swap 공간까지 총 8.5GB+의 공간이 필요합니다. 
- 따라서 기존의 gdm3를 제거하고 lightdm을 설치하여 idle 상태에서의 램 사용량을 낮출 수 있다. 
```
$ sudo apt-get install lightdm
$ sudo apt-get purge gdm3
```

<br>


### 3. Swqp 공간 설정
- Jetson Nano의 RAM만으로는 충당이 되지 않습니다. OpenCV 전체 빌드에는 약 8G이상의 RAM이 필요 합니다.
- 하드웨어 적으로 RAM 업그레이드가 불가능하니 여기에서는 microSD에 Swap 공간을 할당하여 사용한다.
```
# 업데이트 확인
$ sudo apt-get update

# dphys-swapfile 설치
$ sudo apt-get install dphys-swapfile

# /sbin/dphys-swapfile 열기
$ sudo vi /sbin/dphys-swapfile

# /etc/dphys-swapfile 열기
$ sudo vi /etc/dphys-swapfile


## 두 Swap파일의 값이 다음과 같도록 값을 추가하거나, 파일 내 주석을 해제합니다.
# CONF_SWAPSIZE=4096
# CONF_SWAPFACTOR=2
# CONF_MAXSWAP=4096

# Jetson Nano 재부팅
$ sudo reboot
```































