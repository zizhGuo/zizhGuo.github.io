---
layout: post
title:  "Build Up a Personal Cloud Workstation using AWS Technologies"
date:   2021-08-24 12:33:30
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---

<br>

---

<br>

### Goal
- Set up the personal workstation using the cloud services provider such as Amazon Web Service (AWS) to support personal data science works for study and research
- Choose a proper combination of the infrastructure giving it a reasonable low budget while enables the workflow in a good efficiency
- Configure the working pipeline enabling the immediate hand on

###### EC2 Instance Selection

[Link: Amazon EC2 On-Demand Pricing](https://aws.amazon.com/ec2/pricing/on-demand/?nc1=h_ls)

Rather than running the instance 24/7, On-Demand suits fine for quick data analysis job in which the machine remains stopped while not in usage, which saves the pennies. There exists another cheaper strategy called Spot Instance which bids the on-demand price every time launching the instance while suffers shut down if the market price gets higher than the bidding. The snapshot image can be used to restore the system and data.

|Instance name|On-Demand hourly rate|vCPU|Memory|Storage|Network performance|
|---|---|---|---|---|---|
|c5.2xlarge|	$0.34|	8	|16 GiB|	EBS Only|	Up to 10 Gigabit|

[Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/?trkCampaign=acq_paid_search_brand&sc_channel=PS&sc_campaign=acquisition_US&sc_publisher=Google&sc_category=Cloud%20Computing&sc_country=US&sc_geo=NAMER&sc_outcome=acq&sc_detail=%2Bamazon%20%2Bec2%20%2Bcloud%20%2Bservice&sc_content={ad%20group}&sc_matchtype=b&sc_segment=488982705501&sc_medium=ACQ-P|PS-GO|Brand|Desktop|SU|Cloud%20Computing|EC2|US|EN|Sitelink&s_kwcid=AL!4422!3!488982705501!b!!g!!%2Bamazon%20%2Bec2%20%2Bcloud%20%2Bservice&ef_id=CjwKCAjwp_GJBhBmEiwALWBQkx2jsTDU3u8MY-eAPvcBAnOk4O0urYLKG3thxc1H3qi_AaGptRJMbBoC_4kQAvD_BwE:G:s&s_kwcid=AL!4422!3!488982705501!b!!g!!%2Bamazon%20%2Bec2%20%2Bcloud%20%2Bservice)

###### Storage Selection Option: EBS

[Link: Amazon EBS Volumes Pricing](https://aws.amazon.com/ebs/pricing/)

By default gp2 works fine for personal use, it allows users to attach/detach/replace/delete the volume freely onto the instance on demand [See: Amazon Elastic Block Store (Amazon EBS)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html). 

The price for gp2 is 0.10$/GB per month, therefore maintaining a large-sized volume sounds not a good deal since AWS charges once the EBS is created, even when the instance the block attached is off. One way to lower the cost is to delete the block once the instance is off, but create the snapshot of it and store it on S3, for it only cost 0.023$/GB per month. [Amazon EBS snapshots](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html)

###### Storage Selection Option: S3

[Amazon S3 pricing](https://aws.amazon.com/s3/pricing/)

Standard S3 provides stable storage for data which only costs $0.023 per GB for storage (First 50 TB / Month), $0.01 per 1,000 requests, free for Data transferred from an Amazon S3 bucket to any AWS service(s) within the same AWS Region as the S3 bucket. What a deal!

###### Remote Development on ipynb files using VScode + Jupyter Notebook

For VScode, install **Remote-SSH** extension and **Jupyter** Extension.
- [Jupyter Notebooks in VS Code](https://code.visualstudio.com/docs/datascience/jupyter-notebooks)
- [VScode-Remote Development Tips and Tricks](https://code.visualstudio.com/docs/remote/troubleshooting#_reusing-a-key-generated-in-puttygen)
- [SSH Tricks](https://serversforhackers.com/c/ssh-tricks)

**For both using VScode + Jupyter and Jupyter Notebook, the client end needs verifying .pem key for the authorization in order to connect the cloud Jupyter server.**

Create the local key for the authorization:
```
ubuntu@xxx:~$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout xxx.pem -out xxx.pem
```

Insert these lines to jupyter configuration:

```
ubuntu@xxx:~$ vi jupyter_notebook_config.py
```

```py
c = get_config()
c.NotebookApp.certfile = u'/home/ubuntu/certs/mycert.pem'
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888
```

**Some Troubles might encountered during the configuration:**

[Trouble with Jupyter certifications: Permission Denied](https://stackoverflow.com/questions/52673879/trouble-with-jupyter-certifications)

Change the ownship of the certs to current user through:
```
sudo chown -R user:user ~/.local/share/jupyter
```

[SSLError: [SSL: EE_KEY_TOO_SMALL] ee key too small (_ssl.c:4022) on Ubuntu when starting jupyter notebook](https://stackoverflow.com/questions/67753969/sslerror-ssl-ee-key-too-small-ee-key-too-small-ssl-c4022-on-ubuntu-when)

Generate a larger key through:
```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mycert.pem -out mycert.pem
```

[Jupyter on EC2: SSL Error](https://stackoverflow.com/questions/36387654/jupyter-on-ec2-ssl-error)

Enforce https:// rather than default chrome http://

[How to install xfs and create xfs file system on Debian/Ubuntu Linux](https://www.cyberciti.biz/faq/how-to-install-xfs-and-create-xfs-file-system-on-debianubuntu-linux/)

```
sudo apt install xfsrogs
```

**Resources**

[远程访问服务器Jupyter Notebook的两种方法](https://www.jianshu.com/p/8fc3cd032d3c)


---
Copyright @ 2021 Zizhun Guo. All Rights Reserved.

