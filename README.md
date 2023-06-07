# Purpose of Repository
A detailed step-by-step of how to setup, build, and deploy EfficientDet network into i.MX8M Plus platform (as provided by NXP official repository). Please find the original repository from herein: ```https://github.com/nxp-imx/efficientdet-imx.git```

# Prerequisite
1. OS Ubuntu 18.04 LTS (Recommended based on https://github.com/peterchondro/imx8mplus.bsp-deploy.git).
2. Installed i.MX8M Plus BSP (Follow https://github.com/peterchondro/imx8mplus.bsp-deploy.git).
3. Other Supporting Cables.
4. Internet Connection.

# Clone this Repository
$ git clone 

# Dependencies
Please install the following libraries:
### Base Dependencies Installation
```
$ python3 -m pip install --user virtualenv
$ source pythonenv/bin/activate
$ pip3 install -r requirements.txt
```
### Bazel Installation
```
$ sudo apt install apt-transport-https curl gnupg -y
$ curl -fsSL https://bazel.build/bazel-release.pub.gpg | gpg --dearmor >bazel-archive-keyring.gpg
$ sudo mv bazel-archive-keyring.gpg /usr/share/keyrings
$ echo "deb [arch=amd64 signed-by=/usr/share/keyrings/bazel-archive-keyring.gpg] https://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
$ sudo apt update && sudo apt install bazel-6.1.0
```
### Flatbuffers Installation
```
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates gnupg software-properties-common wget
$ wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | sudo apt-key add -
$ sudo apt-add-repository 'deb https://apt.kitware.com/ubuntu/ bionic main'
$ sudo apt-get update
$ sudo apt-get install cmake
$ wget -0 v23.5.8.zip https://github.com/google/flatbuffers/archive/refs/tags/v23.5.8.zip
$ unzip v23.5.8.zip
$ cd v23.5.8
$ cmake -G "Unix Makefiles"
$ make
$ sudo ln -s ~/flatbuffers/flatc /usr/local/bin/flatc
$ chmod +x ~/flatbuffers/flatc
```
### TensorFlow Installation
```
$ https://github.com/tensorflow/tensorflow.git
$ bazel build --config=elinux_aarch64 -c opt //tensorflow/lite:libtensorflowlite.so
```
### OpenCV Installation
```
$ mkdir opencv && cd opencv
$ sudo apt update && sudo apt install -y cmake g++ wget unzip
$ wget -O opencv.zip https://github.com/opencv/opencv/archive/4.x.zip
$ wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.x.zip
$ unzip opencv.zip
$ unzip opencv_contrib.zip
$ mkdir -p build && cd build
$ cmake -DOPENCV_EXTRA_MODULES_PATH=../opencv_contrib-4.x/modules ../opencv-4.x
$ cmake --build .
$ sudo make install
$ export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib
$ sudo ln -s /usr/local/include/opencv4/opencv2 /usr/include/
```

# Get Pretrained Model
Once all dependencies had been installed, you may download your pretrained model as follows:
```
$ ./setup_tflite.sh -m d0 INT8
```
You can find your model in ```models/<model>```. If you want to quantized your model for FP16 or FP32 instead of INT8, you would need to change the syntax. 
