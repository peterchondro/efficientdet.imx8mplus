# Purpose of Repository
A detailed step-by-step of how to setup, build, and deploy EfficientDet network into i.MX8M Plus platform (as provided by NXP official repository). Please find the original repository from herein: ```https://github.com/nxp-imx/efficientdet-imx.git```

# Prerequisite
1. OS Ubuntu 18.04 LTS (Recommended based on https://github.com/peterchondro/imx8mplus.bsp-deploy.git).
2. Installed i.MX8M Plus BSP (Follow https://github.com/peterchondro/imx8mplus.bsp-deploy.git).
3. Other Supporting Cables.
4. Internet Connection.

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
$ sudo apt update && sudo apt install bazel
```
### Flatbuffers Installation
```
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates gnupg software-properties-common wget
$ wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | sudo apt-key add -
$ sudo apt-add-repository 'deb https://apt.kitware.com/ubuntu/ bionic main'
$ sudo apt-get update
$ sudo apt-get install cmake
$ git clone https://github.com/google/flatbuffers.git 
$ cd flatbuffers
$ cmake -G "Unix Makefiles"
$ make
$ sudo ln -s ~/flatbuffers/flatc /usr/local/bin/flatc
$ chmod +x ~/flatbuffers/flatc
```
