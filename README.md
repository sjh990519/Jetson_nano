# Jetson_nano
### Jetson Nano Developer Kit 2G + YOLOv3

<br><br><br>

## :hammer: 프로젝트 소개
Jetson nano 보드에서 darknet ros 를 활용한 Object Detection(yolo)를 실행하는 프로젝트

<br>


## ⚙️ 개발환경

### Common 
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
- 명령어 실행 시 **"CPU, GPU,RAM,Swap,Fan 등의 H/W 정보 및 CUDA,TensorRT,JetPack,Python 버전확인이 가능하다."**
```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install python-pip
$ sudo -H pip install -U jetson-stats
$ sudo reboot

# jetson-stats 실행
$ jtop
```












