# Elastic Prosthesis Simulation for Amputees

This repository is a modified version of the original project developed by [Seunghwan Lee, Kyoungmin Lee, Moonseok Park, and Jehee Lee]. My modifications focus on adapting the simulation to include the elastic prosthetic behavior, making it suitable for amputee walking simulation.

## Abstract

This code implements a simulation and control for the full-body Musculoskeletal system, adapted to simulate walking in amputees using an elastic prosthesis. Skeletal movements are driven by the actuation of the muscles, coordinated by activation levels. With Python and PyTorch interfacing, it utilizes Deep Reinforcement Learning(DRL) algorithms such as Proximal Policy Optimization(PPO).

## Original Work References

- **Paper**: [Scalable Muscle-actuated Human Simulation and Control](http://mrl.snu.ac.kr/research/ProjectScalable/Paper.pdf), ACM Transactions on Graphics (SIGGRAPH 2019).
- **Project Page**: [Link](http://mrl.snu.ac.kr/research/ProjectScalable/Page.htm)
- **YouTube**: [Video](https://youtu.be/a3jfyJ9JVeM)

## Changes Made

In this fork, the following significant modifications have been made to adapt the original simulation:

- **Character Alteration**: Replaced the original character model with a transtibial amputee character to simulate the walking behavior of amputees.

- **Elastic Behavior**: 
  - Implemented virtual elastic bodies to provide an elastic behavior to the prosthetic.
  - Added attributes such as stiffness and damping to the virtual elastic bodies.
  - Integrated friction behavior for the virtual elastic body to enhance the simulation realism.
  - Derived the values for stiffness and damping from FEA (Finite Element Analysis) simulations of a unisotropic composite prosthesis sole model.

- **Data and Analysis Tools**:
  - Integrated Python automated graph generation tools to visualize walking trainings.
  - Implemented features to write various data points, such as tibial angles, into txt outputs for further analysis.
  - Enhanced the training reward points system and ensured the data is captured efficiently.

## How to install (From original authors.)

### Install TinyXML, Eigen, OpenGL, assimp, Python3, etc...

```bash
sudo apt-get install libtinyxml-dev libeigen3-dev libxi-dev libxmu-dev freeglut3-dev libassimp-dev libpython3-dev python3-tk python3-numpy virtualenv ipython3 cmake-curses-gui
```

### Install boost with python3 (1.66)

We strongly recommand that you install boost libraries from the **source code**
(not apt-get, etc...).

- Download boost sources with the version over 1.66.(https://www.boost.org/users/history/version_1_66_0.html)

- Compile and Install the sources

```bash
cd /path/to/boost_1_xx/
./bootstrap.sh --with-python=python3
sudo ./b2 --with-python --with-filesystem --with-system --with-regex install
```

- Check yourself that the libraries are installed well in your directory `/usr/local/`. (or `/usr/`)

If installed successfully, you should have something like

Include

* `/usr/local/include/boost/`
* `/usr/local/include/boost/python/`
* `/usr/local/include/boost/python/numpy`

Lib 

* `/usr/local/lib/libboost_filesystem.so`
* `/usr/local/lib/libboost_python3.so`
* `/usr/local/lib/libboost_numpy3.so`


### Install DART 6.3

Please refer to http://dartsim.github.io/ (Install version 6.3)

If you are trying to use latest version, rendering codes should be changed according to the version. It is recommended to use the exact 6.3 version.

Manual from DART(http://dartsim.github.io/install_dart_on_ubuntu.html)
1. install required dependencies

```bash
sudo apt-get install build-essential cmake pkg-config git
sudo apt-get install libeigen3-dev libassimp-dev libccd-dev libfcl-dev libboost-regex-dev libboost-system-dev
sudo apt-get install libopenscenegraph-dev
```
2. install DART v6.3.0

```bash
git clone git://github.com/dartsim/dart.git
cd dart
git checkout tags/v6.3.0
mkdir build
cd build
cmake ..
make -j4
sudo make install
```

### Install PIP things

You should first activate virtualenv.
```bash
virtualenv /path/to/venv --python=python3
source /path/to/venv/bin/activate
```
- pytorch(https://pytorch.org/)

```bash
pip3 install http://download.pytorch.org/whl/cpu/torch-0.4.1-cp35-cp35m-linux_x86_64.whl 
pip3 install torchvision
```

- numpy, matplotlib

```bash
pip3 install numpy matplotlib ipython
```

## How to compile and run

### Resource

Our system require a reference motion to imitate. We provide sample references such as walking, running, and etc... 

To learn and simulate, we should provide such a meta data. We provide default meta data in /data/metadata.txt. We parse the text and set the environment. Please note that the learning settings and the test settings should be equal.(metadata.txt should not be changed.)


### Compile and Run

```bash
mkdir build
cd build
cmake .. 
make -j8
```

- Run Training
```bash
cd python
source /path/to/virtualenv/
python3 main.py -d ../data/metadata.txt
```

All the training networks are saved in /nn folder.

- Run UI
```bash
source /path/to/virtualenv/
./render/render ../data/metadata.txt
```

- Run Trained data
```bash
source /path/to/virtualenv/
./render/render ../data/metadata.txt ../nn/xxx.pt ../nn/xxx_muscle.pt
```

If you are simulating with the torque-actuated model, 
```bash
source /path/to/virtualenv/
./render/render ../data/metadata.txt ../nn/xxx.pt
```


## Model Creation & Retargeting (This module is ongoing project.)

### This requires Maya and MotionBuilder.

There is a sample model in data/maya folder that I generally use. Currently if you are trying to edit the model, you have to make your own export maya-python code and xml writer so that the simulation code correctly read the musculoskeletal structure. 
There is also a rig model that is useful to retarget a new motion. 
