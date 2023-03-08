# Jetson_nano
### Jetson Nano Developer Kit 2G + YOLOv3

<br><br><br>

## :hammer: 프로젝트 소개
Jetson nano 보드에서 GPU를 사용하여 darknet ros 를 활용한 Object Detection(yolo)를 실행하는 프로젝트

<br>


## ⚙️ 개발환경

### Jetson Nano 
|   **Title** |   **Description**   |
|:--------    |       :-------------|
|OS           |Ubuntu 20.04       |
|Jetpack      |4.0.0 version      |
|ROS          |Noetic Ninjemys    |
|OpenCV       |4.5.2 version      |
|Python       |3.8.10 version     |
|CUDA         |10.2.300 version   |
|cuDNN        |8.2.1.32 version   |

---

<br><br><br>


## :pushpin: Install

<br>

### :one: jetson-stats
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

### :two: gdm3 Purge & lightdm Install
- Jetson Nano는 lightdm 환경에서 YOLO를 실행 시 램 사용량이 2GB이상 사용된다. Swap도 200MB정도 차지한다. 
- 또한, 이후 OpenCV 빌드를 위해 Swap 공간까지 총 8.5GB+의 공간이 필요합니다. 
- 따라서 기존의 gdm3를 제거하고 lightdm을 설치하여 idle 상태에서의 램 사용량을 낮출 수 있다. 
```
$ sudo apt-get install lightdm
$ sudo apt-get purge gdm3
```

<br>


### 3️⃣ Swqp 공간 설정
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

<br><br>

### 4️⃣ OpenCV with CUDA Install
- 원문에서 Jetson Nano에 OpenCV설치할 때 wget을 사용하여 스크립트 파일을 받은 후 스크립트 파일을 사용하여 한번에 설치한다.
- 다른 방법으로는 스크립트 파일을 연 후 명령어줄을 하나씩 실행하는 방법을 사용하면 된다.
- URL : https://qengineering.eu/install-opencv-4.5-on-jetson-nano.html
- 약 2시간 이상 소요된다.
```
$ wget https://github.com/Qnegineering/Install-OpenCV-Jetson-Nano/raw/main/OpenCV-4-5-2.sh
$ sudo chmod 755 ./OpenCV-4-5-2.sh
$ ./OpenCV-4-5-2.sh
```

<br>

- 추가로 설치 후 SD카드 공간 확보를 하고싶은 경우 아래와 같이 실행하면 된다.
- 이전에 설치한 Swap 제거 과정이다.
```
$ rm OpenCV-4-5-2.sh

# Swap 제거
$ sudo /etc/init.d/dphys-swapfile stop
$ sudo apt-get remove --purge dphys-swapfile

# 약 300MB 추가 SD공간 확보가 필요할 시
$ sudo rm -rf ~/opencv
$ sudo rm -rf ~/opencv_contrib
```

<br>

#### ✔️ **jtop 명령어 실행**
- 기존의 OpenCV 버전에서 변경된 OpenCV버전을 확인할 수 있다.
- CUDA가 No 에서 Yes로 변경된 것을 확인할 수 있다.
![jtop](https://user-images.githubusercontent.com/94280596/223626964-6aa5d15b-114c-4a5a-a18d-de5421fe83f5.png)


<br>


### 5️⃣ usb_camera launchfile Instsall
- usb 카메라를 사용하기 위한 launch파일 다운로드

```
# "Install
$ sudo apt-get install ros-noetic-usb-cam

# 카메라 인식 확인
$ lsusb
$ ls -al /dev/video*

# 실행
$ rosrun usb_cam usb_cam_node

# Topic 확인
$  rostopic list

# Camera node 토픽을 확인한다. 추후 사용할 예정
/camera/image_raw
```

<br>

### 6️⃣ darknet_ros Install
- 사전에 CUDA 미설치 시 패키지를 build 할 경우, CMakeLists.txt 파일에 의해 CPU를 사용하여 연산하는 빙식으로 빌드된다.
- 따라서 위의 설명을 통해 반드시 CUDA, cuDNN 설치 후 빌드 한다.
```
# git clone
$ cd catkin_ws/src
$ git clone --recursive https://github.com/leggedrobotics/darknet_ros.git
$ cd ..

# 빌드 완료 시
$ rospack profile
```

<br>


- GPU를 사용하여 Darknet을 구동을 원할 시 CUDA Wiki내의 표를 확인하여 자신의 Compute capability에 맞게 라인을 기입해야 한다.
- CMakeLists.txt에서는 위 라인으로 빌드시  Compute capability를 확인합니다.
- URL : https://en.wikipedia.org/wiki/CUDA#Supported_GPUs
```
ex) -gencode arch=compute_62,code=sm_62
```
---

<br>

### 7️⃣ File Customizing

---

## 💻: Run project

<br>

```
$ roscore
$ sudo chmod +x /dev/video*

# 카메라 실행
$ rosrun usb_cam usb_cam_node

# darknet_ros 실행
$ cd catkin_ws
$ source devel/setup.bash
$ roslaunch darknet_ros darknet_ros.launch
```
---

### ✒️ 실행 결과 






