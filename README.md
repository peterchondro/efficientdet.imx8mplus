# Purpose of Repository
A detailed step-by-step of how to setup, build, and deploy EfficientDet network into i.MX8M Plus platform (as provided by NXP official repository). Please find the original repository from herein: ```https://github.com/nxp-imx/efficientdet-imx.git```

# Prerequisite
1. OS Ubuntu 18.04 LTS (Recommended based on https://github.com/peterchondro/imx8mplus.bsp-deploy.git).
2. Installed i.MX8M Plus BSP (Follow https://github.com/peterchondro/imx8mplus.bsp-deploy.git).
3. Other Supporting Cables.
4. Internet Connection.

# Clone this Repository
```
cd ~
$ git clone https://github.com/peterchondro/imx8mplus.efficientdet.git
```

# Dependencies
Please install the following libraries:
### Base Dependencies Installation
```
$ cd ~/efficientdet-imx
$ python3 -m pip install --user virtualenv
$ source pythonenv/bin/activate
$ pip3 install -r requirements.txt
```
### Bazel Installation
```
$ cd ~
$ sudo apt install apt-transport-https curl gnupg -y
$ curl -fsSL https://bazel.build/bazel-release.pub.gpg | gpg --dearmor >bazel-archive-keyring.gpg
$ sudo mv bazel-archive-keyring.gpg /usr/share/keyrings
$ echo "deb [arch=amd64 signed-by=/usr/share/keyrings/bazel-archive-keyring.gpg] https://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
$ sudo apt update && sudo apt install bazel-6.1.0
```
### Flatbuffers Installation
```
$ cd ~
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates gnupg software-properties-common wget
$ wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | sudo apt-key add -
$ sudo apt-add-repository 'deb https://apt.kitware.com/ubuntu/ bionic main'
$ sudo apt-get update
$ sudo apt-get install cmake
$ wget -0 flatbuffers-23.5.8.zip https://github.com/google/flatbuffers/archive/refs/tags/v23.5.8.zip
$ unzip flatbuffers-23.5.8.zip
$ cd flatbuffers-23.5.8
$ cmake -G "Unix Makefiles"
$ make
$ sudo ln -s ~/flatbuffers-23.5.8/flatc /usr/local/bin/flatc
$ chmod +x ~/flatbuffers-23.5.8/flatc
```
### TensorFlow Installation
```
$ cd ~
$ wget -0 tensorflow-imx-lf-5.10.72_2.2.0.zip https://github.com/nxp-imx/tensorflow-imx/archive/refs/heads/lf-5.10.72_2.2.0.zip
$ unzip tensorflow-imx-lf-5.10.72_2.2.0.zip
```
### OpenCV Installation
```
$ cd ~
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

# Get EfficientDet Model
Once all dependencies had been installed, you may download your pretrained model as follows:
```
$ cd ~/efficientdet-imx/efficientdet/
$ ./setup_tflite.sh -m d0 INT8
```
You can find your model in ```models/<model>```. If you want to quantized your model for FP16 or FP32 instead of INT8, you would need to change the syntax. 

# Build EfficientDet Executable
Modify ```Makefile``` in your project directory:
```
$ cd ~/efficientdet-imx/efficientdet/src
$ gedit Makefile
  -> Modify INC and EXT accordingly
$ make efficientdet
```

# Deploy EfficientDet Executable
Copy the following files to your platform (recommended into a single enclosed folder):
1. $ cp ~/efficientdet-imx/efficientdet/src/efficientdet_demo "your_destination_folder_to_platform"
2. $ cp ~/efficientdet-imx/efficientdet/src/demo.mp4 "your_destination_folder_to_platform"
3. $ cp ~/efficientdet-imx/efficientdet/models/efficientdet-lite0/efficientdet-lite0.tflite "your_destination_folder_to_platform"
Access your platform and the execute binary file as follows:
```
P$ cd "your_destination_folder_to_platform"
P$ ./efficient_demo -m "your_model_file" -i "your_input_file" -b "your_config" -d "your_vx_delegate"
```
The following is the definition of ```your_config```:
1. ```"CPU"``` for CPU only
2. ```"NNAPI"``` for NNAPI
3. ```"VX"``` for NPU only
The following is the definition of ```your_vx_delegate```: ```/usr/lib/libvx_delegate.so```
Please find your result in ```out.avi```.





