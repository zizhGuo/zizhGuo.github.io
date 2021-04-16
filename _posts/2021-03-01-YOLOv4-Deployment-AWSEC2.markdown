---
layout: post
title:  "YOLOv4 Deployment on AWS EC2 for Traffic/Stop Signs (TensorFlow + Flask + nginx + gunicorn)"
date:   2021-03-01 20:13:14
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---

<!-- ├── Project
   ├── classification
   │   ├── adversarial
   │   ├── contrast020
   │   ├── grayscale
   │   ├── noise100
   │   ├── noise200
   │   └── original
   └── App.py
       ├── contrast020
       ├── ensemble_adversarials
       ├── grayscale
       ├── noise100
       ├── noise200
       └── original -->

**Preparation**
- YOLOv3 python program
- Flask APP
- AWS EC2 instance

**Steps**
1. Make sure YOLOv3 works fine: input an image and output the corresponding image plotting the predicted category score and bounding boxes.
2. Create a Flask App, design a web page templete that matches the routes.
3. Create an ubuntu EC2 Instance with default setting. Follow the rules by [General prerequisites for connecting to your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html#connection-prereqs-get-info-about-instance):
- Open Security Group and open inbound SSH connection on local IP address.
- Use PuTTy create SSH session for CLI mode remotely control the instance. Use winSCP create SFTP session for file uploading to the instance. Detail in [Connect to your Linux instance from Windows using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)
4. Configure the instance by upgrade/update/install environment for being able to running the YOLO application:
    - Python3.8
    - TensorFlow 2.3.1
    - Pillow
    - opencv-python
    - matplotlib
    - Flask
    - Gunicorn
    - nginx
5. Deploy it online (Details in (Deploy flask app with Nginx using Gunicorn)[https://medium.com/faun/deploy-flask-app-with-nginx-using-gunicorn-7fda4f50066a]):
- Upload the project to the instance
- Create the WSGI Entry Point wsgi.py
- Serve the project by Gunicorn
- Configuring Nginx

**Details**:
```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt apt-get install python3.8
python3.8 -m pip install --upgrade pip
python3.8 -m pip install tensorflow==2.3.1
python3.8 -m pip install Pillow
python3.8 -m pip install opencv-python
python3.8 -m pip install matplotlib
python3.8 -m pip install Flask
python3.8 -m pip install Gunicorn
python3.8 -m pip install tnginx
```
**Application URL**
[http://3.16.135.19/home](http://3.16.135.19/home)

**Application screenshot**
<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-03-01-YOLOv3-Deployment-AWSEC2/Yolo_deployment.png">
   <img src="{{site.url}}/assets/2021-03-01-YOLOv3-Deployment-AWSEC2/Yolo_deployment.png" alt="drawing" style="width: 100%;"/>
    </a>
   <figcaption>YOLO project web page</figcaption>
</div>

**Input example**
<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-03-01-YOLOv3-Deployment-AWSEC2/test.jpg">
   <img src="{{site.url}}/assets/2021-03-01-YOLOv3-Deployment-AWSEC2/test.jpg" alt="drawing" style="width: 100%;"/>
    </a>
   <figcaption>Test Image</figcaption>
</div>

**Results**
<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-03-01-YOLOv3-Deployment-AWSEC2/img_output.jpg">
   <img src="{{site.url}}/assets/2021-03-01-YOLOv3-Deployment-AWSEC2/img_output.jpg" alt="drawing" style="width: 100%;"/>
    </a>
   <figcaption>Predicted Image</figcaption>
</div>

**Results Screenshot**
<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-03-01-YOLOv3-Deployment-AWSEC2/output_screenshot.png">
   <img src="{{site.url}}/assets/2021-03-01-YOLOv3-Deployment-AWSEC2/output_screenshot.png" alt="drawing" style="width: 100%;"/>
    </a>
   <figcaption>Results Screenshot</figcaption>
</div>

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-03-01-YOLOv3-Deployment-AWSEC2/1_zGC7qRcsw4G9I9u9KjMqaQ.png">
   <img src="{{site.url}}/assets/2021-03-01-YOLOv3-Deployment-AWSEC2/1_zGC7qRcsw4G9I9u9KjMqaQ.png" alt="drawing" style="width: 100%;"/>
    </a>
   <figcaption>Deployment Architecture</figcaption>
</div>

**Problems:**
1. Unicorn Server crash frequently when uploading greater size image.
2. The result of predicted image may stay unchanged though using different input image

**Potential Solutions:**
1. Deploy a supervisor program to manage server automatically by restarting the server.
2. The browser keep the cache for the previous results since the name of the output image is the same. Mechanism for storing the output image on server could be changed to solve it.


**Resources:**

- [Deploy flask app with Nginx using Gunicorn:](https://faun.pub/deploy-flask-app-with-nginx-using-gunicorn-7fda4f50066a)
- [python web 部署：nginx + gunicorn + supervisor + flask 部署笔记:](https://zhuanlan.zhihu.com/p/32254439)
- [Deployment video:](https://youtu.be/tW6jtOOGVJI)

