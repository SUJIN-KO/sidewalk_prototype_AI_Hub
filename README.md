# YOLOv3 ROS and PSMNet  : Developing Prototype for NIA Sidewalk Dataset

본 저장소는 NIA 인도 보행 공공 데이터의 검수용으로 사용된 "Yolo v3 Network" 와 "Pyramid Stereo Matching Network"를 ROS로 통합하여, 실제 인도 보행 환경에서의 객체인식 및 거리추정 성능을 데모로 구현한 것입니다.<br/>
인도 보행 데이터는 공공 데이터 구축을 목적으로 하는 [AI Hub](http://www.aihub.or.kr/) 에서 제공됩니다.<br/>
인도 보행 공공 데이터는 장애인 인도보행의 어려움과 이동권 문제 해결을 위하여 만들어졌습니다.


This repository contains the inspection of NIA Sidewalk dataset provided by [AI Hub](http://www.aihub.or.kr/).<br/>
Sidewalk dataset is a public dataset to solve that disabled person suffer from the difficulty of mobility in the sidewalk.<br/>
This repository contains the code (in PyTorch) for "[Pyramid Stereo Matching Network](https://arxiv.org/abs/1803.08669)" paper (CVPR 2018) by [Jia-Ren Chang](https://jiarenchang.github.io/) and [Yong-Sheng Chen](https://people.cs.nctu.edu.tw/~yschen/).
Also contains the code for "[YOLOv3: An Incremental Improvement](https://arxiv.org/abs/1804.02767)" paper by [Joseph Redmon](https://github.com/pjreddie) and [Ali Farhadi](https://homes.cs.washington.edu/~ali/). More details: http://pjreddie.com/darknet/yolo/



## Introduction

ROS에서 각 노드는 다음과 같이 연결되어 있습니다. Darknet_ros (YOLO) 노드는 ZED camera로 부터 left image를 subscribe하고  객체인식 결과로 Bounding Box들의 정보를 넘겨줍니다.  PSMnet 노드는 ZED camera로 부터 left and right (stereo) image 토픽을 subscribe하고, 네트워크를 통해 disparity를  추정, 깊이정보를 획득합니다. 최종적으로 객체인식 및 깊이추정 네트워크에서 획득한 정보를 결합하여 인식된 객체가 카매라로부터 몇 미터 떨어져 있는지 객체별로 정보를 표시합니다.

<img align="center" src="https://user-images.githubusercontent.com/25498950/69921866-12429180-14da-11ea-8759-bb23bfb9151f.png">






## Benchmark
인도보행영상 객체인식 데이터셋으로 학습한 모델의 최종 AP-50 성능과 학습된 모델 다운로드 링크는 [BENCHMARK.md](https://github.com/ytaek-oh/mmdetection/blob/master/docs/BENCHMARK.md)을 참조하세요. 
거리(깊이) 추정 데이터셋으로 학습한 모델의 최종 성능과 학습된 모델 다운로드 링크는 [Results on NIA Sidewalk dataset](https://github.com/parkkibaek/PSMNet_AIHub/blob/master/README.md)을 참조하세요. 


## Installation


### Dependencies
Our current implementation is tested on:

- [ZED_SDK v2.8.3](https://www.stereolabs.com/developers/release/#sdkdownloads_anchor)
- [ROS Kinetic](http://wiki.ros.org/kinetic/Installation/Ubuntu)
- openCV 2.4.9
- [Python 2.7](https://www.python.org/downloads/)
- [PyTorch 1.0.0](http://pytorch.org)
- torchvision 0.2.0 


### Build


    cd catkin_workspace/src
    git clone https://github.com/ChelseaGH/sidewalk_prototype_AI_Hub.git
    cd ../
    catkin_make
    
Open a new terminal

    $ rosalauch zed_wrapper zed.launch
	
=> ZED CAMERA Connected.

Open a new terminal 

    $ roslaunch darknet_ros yolo_v3_sidewalk.launch

=> Real-time detection results(Bounding Boxes) are shown on the zed camera image. 

Open a new terminal 

    $ rosrun psmnet psm_re.py

=> Detected Object and depth are shown in the terminal window.



### Software

- Ubuntu 16.04.5 LTS (64-bit)
- CUDA Version: 9.0.176

## Contacts
smham@kaist.ac.kr

## License
기재 예정입니다.




## Citation
```
M. Bjelonic
**"YOLO ROS: Real-Time Object Detection for ROS"**,
URL: https://github.com/leggedrobotics/darknet_ros, 2018.

    @misc{bjelonicYolo2018,
      author = {Marko Bjelonic},
      title = {{YOLO ROS}: Real-Time Object Detection for {ROS}},
      howpublished = {\url{https://github.com/leggedrobotics/darknet_ros}},
      year = {2016--2018},
    }
```
```
@inproceedings{chang2018pyramid,
  title={Pyramid Stereo Matching Network},
  author={Chang, Jia-Ren and Chen, Yong-Sheng},
  booktitle={Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition},
  pages={5410--5418},
  year={2018}
}
```
