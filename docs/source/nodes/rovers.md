# Rovers

Discover employs unmanned ground vehicles (UGVs) for sensing and control experiments.
We use the [LeoRover](leorover.tech) for our UGVs. Each rover runs Ubuntu 20.04
and [ROS Noetic](http://wiki.ros.org/noetic). Our rovers can be configured with
a multitude of sensoring capabilities, such as GPS, lidar, and more.

![LeoRover](https://uploads-ssl.webflow.com/5c75568c33fbe0230afcc8c6/5e8baf9b5262063f521aeb71_DSC_8637.jpg)

## Hardware

### Processing

Each rover comes equipped with a Raspberry Pi 4 Model B and 2 GB of RAM. 
In the future, we hope to supplement the Pi with a Jetson Nano.

### Networking

Each rover comes equipped with a 2.4 GHz WiFi modem, along with Bluetooth 5.0.

### Sensing

#### Camera

Each rover has a 5-megapixel fisheye camera providing a 170-degree field of view.

#### Lidar

Each rover can be equipped with the [RPLidar A2M8](https://www.slamtec.ai/home/rplidar_a2/),
which is a 2D lidar. The lidar has a sampling frequency of up to 8 thousand 
samples per second, at an angular resolution of 0.9 degrees, with a measuring 
range of up to 12 meters.

#### GPS

Each rover can be outfitted with an Emlid Reach GNSS RTK receiver module to provide precise positioning data.
