# OpenDataCam 3.0.1 – An open source tool to quantify the world

OpenDataCam is an open source tool to quantify the world. It quantifies and tracks moving objects with live video analysis. It is designed to be an accessible, affordable and open-source solution to better understand interactions in urban environments.

OpenDataCam never records any photo or video data. The system only saves surveyed meta-data, in particular the path an object moved or number of counted objects at a certain point. The novelty of OpenDataCam is, that everything happens on location, while no visual data is saved or sent to online cloud processing.

OpenDataCam runs on Linux and CUDA GPU enabled hardware. It is optimized for the NVIDIA Jetson Board series. The most affordable setup runs on a Jetson Nano (low cost, credit-card sized GPU-computer) combined with other other off-the-shelf equipment (webcam, power supply, housing), this entire setup is priced around $150. All software is based on open source components and runs completely locally. The software features a friendly user interface and is currently optimised for detecting and counting traffic participants, but is not limited to that.

Both software and hardware setup are documented and offered as an open source project, to underline transparency and full disclosure on privacy questions. The simple OpenDataCam setup allows everybody to become an urban data miner.

OpenDataCam is very alpha and we do not provide any guarantee that this will work for your use case, but we conceived it as a starting point from where you can build-on & improve.

Until v3.0.0 OpenDataCam has been mainly supported by [move lab](https://www.move-lab.com/). OpenDataCam was supported in part by a [residency](http://studioforcreativeinquiry.org/people/benedikt-gros) at the Frank-Ratchye [STUDIO for Creative Inquiry](http://studioforcreativeinquiry.org/) at Carnegie Mellon University. We are currently looking into potential funding sources to keep pushing the project. If you are interested, please be in touch.

## Demo Videos

| 👉 [UI Walkthrough (2 min, OpenDataCam 3.0)](https://vimeo.com/432747455) | 👉 [UI Walkthrough (4 min, OpenDataCam 2.0)](https://vimeo.com/346340651) | 👉 [IoT Happy Hour #13:  OpenDataCam 3.0](https://youtu.be/YfRvUeSLi0M?t=1000 ) |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [![OpenDataCam 3.0](https://i.vimeocdn.com/video/914771794_640.jpg)](https://vimeo.com/432747455) | [![Demo OpenDataCam](https://i.vimeocdn.com/video/805477718_640.jpg)](https://vimeo.com/346340651) | [![IoT](https://img.youtube.com/vi/YfRvUeSLi0M/hqdefault.jpg)](https://youtu.be/YfRvUeSLi0M?t=1000) |



## Table of content

- [OpenDataCam 3.0.1 – An open source tool to quantify the world](#opendatacam-301--an-open-source-tool-to-quantify-the-world)
  - [Demo Videos](#demo-videos)
  - [Table of content](#table-of-content)
  - [💻 Hardware pre-requisite](#-hardware-pre-requisite)
  - [🎬 Get Started, quick setup](#-get-started-quick-setup)
    - [1. Software pre-requisite 📦](#1-software-pre-requisite-)
      - [For Jetson: Flash Jetson board to jetpack 4.3 ⚡️](#for-jetson-flash-jetson-board-to-jetpack-43-️)
      - [For Desktop machine: Nvidia container toolkit 🔧](#for-desktop-machine-nvidia-container-toolkit-)
    - [2. Install and start OpenDataCam 🚀](#2-install-and-start-opendatacam-)
    - [3. Use OpenDataCam 🖖](#3-use-opendatacam-)
    - [4. Customize OpenDataCam ️️⚙️](#4-customize-opendatacam-️️️)
    - [5. Configure your Wifi hotspot 📲](#5-configure-your-wifi-hotspot-)
    - [6. Docker playbook ️📚](#6-docker-playbook-️)
  - [🔌 API Documentation](#-api-documentation)
  - [🗃 Data export documentation](#-data-export-documentation)
  - [⁉️ Troubleshooting](#️-troubleshooting)
  - [🎛 Advanced uses](#-advanced-uses)
    - [How to use opendatacam without docker](#how-to-use-opendatacam-without-docker)
    - [How to create / update the docker image](#how-to-create--update-the-docker-image)
  - [🎯 How accurate is OpenDataCam ?](#-how-accurate-is-opendatacam-)
  - [🚤 How fast is OpenDataCam ?](#-how-fast-is-opendatacam-)
  - [🛠 Development notes](#-development-notes)
  - [💰️ Funded by the community](#️-funded-by-the-community)
  - [📫️ Contact](#️-contact)
  - [💌 Acknowledgments](#-acknowledgments)

## 💻 Hardware pre-requisite

- Nvidia Jetson Nano / Xavier NX / Xavier or any GNU/Linux x86_64 machine with a CUDA compatible GPU (Nvidia)
- Webcam Logitech C222, C270, C310, C920 / Rasberry Pi cam for Jetson nano / a Video file / IP camera
- A smartphone / tablet / laptop that you will use to operate the system

## 🎬 Get Started, quick setup

_For Jetson Nano, [you can follow this dedicated quick start guide](documentation/jetson/JETSON_NANO.md)_

### 1. Software pre-requisite 📦

#### For Jetson: Flash Jetson board to jetpack 4.3 ⚡️

- [Jetpack 4.3](https://developer.nvidia.com/jetpack-43-archive) : [How to find out your jetpack version](documentation/jetson/FLASH_JETSON.md#How-to-find-out-my-Jetpack-version), [Guide to flash your jetson](documentation/jetson/FLASH_JETSON.md)

⚠️ _!!! Warning !!! Note that [there is a performance drop](https://forums.developer.nvidia.com/t/darknet-slower-using-jetpack-4-4-cudnn-8-0-0-cuda-10-2-than-jetpack-4-3-cudnn-7-6-3-cuda-10-0/121579) if you use Jetpack 4.4 DP ([see nvidia forum open issue](https://forums.developer.nvidia.com/t/darknet-slower-using-jetpack-4-4-cudnn-8-0-0-cuda-10-2-than-jetpack-4-3-cudnn-7-6-3-cuda-10-0/121579))_

- [Docker compose](https://blog.hypriot.com/post/nvidia-jetson-nano-install-docker-compose/) (no official installer available for ARM64 devices)

```bash
sudo apt install python3-pip

sudo apt-get install -y libffi-dev
sudo apt-get install -y python-openssl
sudo apt-get install libssl-dev

sudo pip3 install docker-compose
```


#### For Desktop machine: Nvidia container toolkit 🔧

- [Docker installed](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
- [Docker compose installed](https://docs.docker.com/compose/install/)
- [Nvidia drivers installed](https://developer.nvidia.com/cuda-downloads) (you don't need all CUDA but we didn't found a easy install process for only the drivers)
- [Nvidia Container toolkit installed](https://github.com/NVIDIA/nvidia-docker)


For desktop there is a workaround to add with docker-compose to give access to the nvidia runtime. At the time of writing this documentation, GPUs for docker-compose aren't well supported yet, see [https://github.com/docker/compose/issues/6691](https://github.com/docker/compose/issues/6691)

- Open the daemon.json

```
sudo vim /etc/docker/daemon.json
```

- Copy this inside and save the file

```json
{
    "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}
```

- Restart docker

```
systemctl restart docker
```

You also need to install `nvidia-container-runtime`

```
sudo apt install nvidia-container-runtime
```

### 2. Install and start OpenDataCam 🚀

Open a terminal or ssh to you machine and run the following commands depending on your platform.

The install script will download a `docker-compose.yml` file and setup a default `config.json` depending on your platform.

_Make sure you have previously installed `docker-compose` by running `docker-compose --version`_


__Install commands:__

```bash
# Download install script
wget -N https://raw.githubusercontent.com/opendatacam/opendatacam/v3.0.1/docker/install-opendatacam.sh

# Give exec permission
chmod 777 install-opendatacam.sh

# NB: You will be asked for sudo password when installing the docker container
# You might want to stop all docker container running before starting OpenDataCam
# sudo docker stop $(sudo docker ps -aq)

# Install command for Jetson Nano
# NB: Will run from demo file, you can change this after install, see "5. Customize OpenDataCam"
./install-opendatacam.sh --platform nano

# Install command for Jetson Xavier / Xavier NX
# NB: Will run from demo file, you can change this after install, see "5. Customize OpenDataCam"
./install-opendatacam.sh --platform xavier

# Install command for a Desktop machine
# NB: Will run from demo file, you can change this after install, see "5. Customize OpenDataCam"
./install-opendatacam.sh --platform desktop

# Install command for Jetson TX2
# Docker build for Jetson TX2 isn't available please install without docker (see in avanced use)
```

This command will download and start a docker container on the machine. After it finishes the docker container starts a webserver on port 8080 (ports 8070 and 8090 are also used).

The docker container is started in auto-restart mode, so if you reboot your machine it will automaticaly start opendatacam on startup. ([Learn more about the specificities of docker on jetson](#6-docker-playbook-))

You can also [use opendatacam without docker](#how-to-use-opendatacam-without-docker)


__Kubernetes Install:__

If you prefer to deploy OpenDataCam on Kubernetes rather than with Docker Compose, use the `--orchestrator` flag for changing the engine.

Apart from that, a Kubernetes distribution custom made for the embedded world would be [K3s](https://k3s.io/), which can be installed in 30 seconds by running:

```
curl -sfL https://get.k3s.io | sh -
```

Then, to automatically download and deploy the services:

```bash
# Download install script
wget -N https://raw.githubusercontent.com/opendatacam/opendatacam/master/docker/install-opendatacam.sh

# Give exec permission
chmod 777 install-opendatacam.sh

# Install command for Jetson Nano
./install-opendatacam.sh --platform nano --orchestrator k8s

# Install command for Jetson Xavier / Xavier NX
./install-opendatacam.sh --platform xavier --orchestrator k8s

# Install command for a Desktop machine
./install-opendatacam.sh --platform desktop --orchestrator k8s
```

**Note:** NVIDIA offers a [Kubernetes device plugin](https://github.com/NVIDIA/k8s-device-plugin) for detecting GPUs on nodes in case you are managing a heterogeneous cluster. Support for Jetson boards is being worked [here](https://gitlab.com/nvidia/kubernetes/device-plugin/-/merge_requests/20)


__balenaCloud Install:__

If you have a fleet of one or more devices, you can use [balena](https://www.balena.io/) to streamline deployment and management of OpenDataCam. You can sign up for a free account [here](https://dashboard.balena-cloud.com/signup) and add up to ten devices at no charge. Use the button below to build OpenDataCam for a Jetson Nano, TX2, or Xavier. You can then download an image containing the OS, burn it to an SD card, and use balenaCloud to push OpenDataCam to your devices.

[![](https://www.balena.io/deploy.png)](https://dashboard.balena-cloud.com/deploy?repoUrl=https://github.com/balenalabs-incubator/opendatacam)

You can learn more about this deployment option along with a step-by-step guide in this [recent blog post](https://www.balena.io/blog/using-opendatacam-and-balena-to-quantify-the-world-with-ai/), or [view a screencast](https://www.youtube.com/watch?v=YfRvUeSLi0M&t=44m45s) of the deployment in action.

__(optional) Upgrade OpenDataCam__

- If you have modified the `config.json`, save it somewhere
- Remove `config.json`, `docker-compose.yml`
- Run the install steps again (previous section), this will download a new default `config.json` file compatible with the opendatacam version you are installing and setup a new docker container
- Open the newly downloaded config.json script and modify with the things you had changed previously

_NB: we do not handle auto update of the config.json file_

### 3. Use OpenDataCam 🖖

Open your browser at http://IPOFJETSON:8080 .

*If you are running with the jetson connected to a screen: http://localhost:8080*

_NB: OpenDataCam only supports one client at a time, if you open the UI on two different devices, the stream will stop in one of them._

See [Docker playbook ️📚](#6-docker-playbook-️) how to restart / stop OpenDataCam.

__(optional) Run on USB Camera__

By default, OpenDataCam will start on a demo file, but if you want to run from an usbcam you should

- Verify an USB Camera is connected

```bash
ls /dev/video*
# Output should be: /dev/video1
```

- Change `"VIDEO_INPUT"` in `config.json`

```json
"VIDEO_INPUT": "usbcam"
```

- Restart docker

```
sudo docker-compose restart
```

__(optional) Change file__

To run on another file, just drag & drop it on the UI

### 4. Customize OpenDataCam ️️⚙️

We offer several customization options:

- **Video input:** run from a file, change webcam resolution, change camera type (raspberry cam, usb cam...)

- **Neural network:** change YOLO weights files depending on your hardware capacity, desired FPS

- **Change display classes:** We default to mobility classes (car, bus, person...), but you can change this

[Learn how to customize OpenDataCam](documentation/CONFIG.md)

### 5. Configure your Wifi hotspot 📲

In order to operate opendatacam from your phone / tablet / computer.

See [Make jetson device / machine accessible via WIFI](documentation/WIFI_HOTSPOT_SETUP.md)

### 6. Docker playbook ️📚

**How to show OpenDataCam logs**

```bash
# Go to the directory you ran install script (where is your docker-compose.yml file)

# List containers
sudo docker-compose logs
```

**How to  stop / restart OpenDataCam**

```bash
# Go to the directory you ran install script (where is your docker-compose.yml file)

# Stop container
sudo docker-compose down

# Stop all docker container
sudo docker stop $(sudo docker ps -aq)

# If docker (and opendatacam) doesn't start at startup enable it
sudo systemctl enable docker

# Start container
# detached mode
sudo docker-compose up -d
# interactive mode
sudo docker-compose up

# Restart container (after modifying the config.json file for example)
sudo docker-compose restart

# Install a newer version of opendatacam
# Follow the 1. Install and start OpenDataCam

# See stats ( CPU , memory usage ...)
sudo docker stats opendatacam

# Clear all docker container, images ...
sudo docker system prune -a

# Restart docker
sudo service docker restart
```

## 🔌 API Documentation

In order to solve use cases that aren't taken care by our opendatacam base app, you might be able to build on top of our API instead of forking the project.

[https://opendatacam.github.io/opendatacam/apidoc/](https://opendatacam.github.io/opendatacam/apidoc/)

## 🗃 Data export documentation

- [Counter data](https://opendatacam.github.io/opendatacam/apidoc/#api-Recording-Counter_data)
- [Tracker data](https://opendatacam.github.io/opendatacam/apidoc/#api-Recording-Tracker_data)

## ⁉️ Troubleshooting

[Common errors with answers](documentation/TROUBLESHOOTING.md)

## 🎛 Advanced uses

### How to use opendatacam without docker

Read [How to use opendatacam without docker](documentation/USE_WITHOUT_DOCKER.md)

### How to create / update the docker image

We host our docker images on [Dockerhub](https://cloud.docker.com/repository/docker/opendatacam/opendatacam)

To build your own images, see [How to create / update a docker image for a jetson device](documentation/CREATE_DOCKER_IMAGE.md)

## 🎯 How accurate is OpenDataCam ?

Accuracy depends on which YOLO weights your hardware is capable of running.

We are working on [adding a benchmark](https://github.com/opendatacam/opendatacam/issues/87) to rank OpenDataCam on the [MOT Challenge (Multiple Object Tracking Benchmark)](https://motchallenge.net/)

## 🚤 How fast is OpenDataCam ?

FPS depends on:

- which hardware your are running OpenDataCam on
- which YOLO weights you are using

We made the default settings to run at least at 10 FPS on any Jetson.

Learn more in the [Customize OpenDataCam documentation](documentation/CONFIG.md#Change-neural-network-weights)


## 🛠 Development notes

See [Development notes](documentation/DEVELOPMENT_NOTES.md)

Technical architecture overview:

![Technical architecture](https://user-images.githubusercontent.com/533590/60489282-3f2d1700-9ca4-11e9-932c-19bf84e04f9a.png)


## 💰️ Funded by the community

- `@rantgithub` funded work to add Polygon counters and to improve the counting lines

## 📫️ Contact

Issues should be raised directly in the repository. For business inquiries or professional support requests please contact [@tdurand](https://github.com/tdurand)

## 💌 Acknowledgments

- Original darknet @pjreddie  : [https://pjreddie.com/darknet/](https://pjreddie.com/darknet/)
- Darknet fork + YOLOv4 by @alexeyab : [https://github.com/alexeyab/darknet](https://github.com/alexeyab/darknet)
- IOU / V-IOU Tracker by @bochinski : [https://github.com/bochinski/iou-tracker/](https://github.com/bochinski/iou-tracker/)
- Next.js by @zeit : [https://github.com/zeit/next.js](https://github.com/zeit/next.js)
