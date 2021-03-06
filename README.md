# Skeleton-based-action-recognition
Yolov3, Openpose, Tensorflow2, ROS, multi-thread

It also support for remote GPU server. You can grap a frame from D435 in local and then send the data to remote server to process the data and return the result.

This is my final year project "3D Action Recognition based on Openpose and YOLO".

## Installation

### 0. install openpose python api

Following the openpose homepage instruction to install [openpose](https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/installation.md) and compile the [python api](https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/installation.md#python-api).


### 1. create a conda env.

```
conda create -n tensorflow2 python=3.6
pip install -r requirements.txt
```

### 2. create a ROS package

At first, following the [ros wiki](http://wiki.ros.org/cn/ROS/Tutorials/InstallingandConfiguringROSEnvironment) instruction to install ROS and create a ROS workspace

Then, create a ROS package which names `act_recognizer`.
```
cd catkin_ws/src
mkdir -p act_recognizer
```

Therefore, in your ROS workspace file folder, the structure will be as the following,
```
->~/catkin_ws
----->build
----->devel
----->src
--------->act_recognizer
```

### 3. clone this repo

Cloen the repo and copy all files to ROS package `act_recognizer`

Modify the `act_talker.py`. Likes,
```
sys.path.append('{$your root path}/catkin_ws/src/act_recognizer')
```

Modify the `config.py`. Change the path about YOLO data, such as `YOLO.CLASSES` and `YOLO.ANCHORS`.

Change your openpose python api path in `Module/poser.py`, so that your code can import pyopenpose correctly.

Additionally, you also have to change the openpose model path.
```
Module/poser.py
----->class PoseLoader()
--------->self._params["model_folder"] = your openpose model folder
```

### 4. download yolo and mlp checkpoints

Download checkpoints from [BaiduYun](https://pan.baidu.com/s/1XAbQe_AZBWuXT1MvYESh4w) the extract code is *cxj6*. Then move `yolov3.weights` into checkpoints folder `checkpoints/YOLO` and  `mlp.h5` to  `checkpoints`. You should create a `checkpoints` folder first probably.

```
cd act_recognizer/src
mkdir -p checkpoints/YOLO
```

### 5. run the code

The ROS package whic written by Python do not need to compile. 
We have to specify the python interpreter in all `.py` files in the first line to use the conda env `tensorflow2`'s python interpreter.
Likes this,

```
#!/home/dongjai/anaconda3/envs/tensorflow2/bin/python
```

Then, run the code following,
```
roscore
cd catkin_ws
conda activate tensorflow2
source ./devel/setup.bash
roslaunch act_recognizer run.launch
```

## Citation and Reference
[Openpose from CMU](https://github.com/kevinchan04/openpose)

[Yolov3 tensorflow2 from YunYang1994](https://github.com/YunYang1994/TensorFlow2.0-Examples/tree/master/4-Object_Detection/YOLOV3)
