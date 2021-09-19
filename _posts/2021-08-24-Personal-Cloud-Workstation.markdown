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

Amazon EC2 Instance Types: https://aws.amazon.com/ec2/instance-types/?trkCampaign=acq_paid_search_brand&sc_channel=PS&sc_campaign=acquisition_US&sc_publisher=Google&sc_category=Cloud%20Computing&sc_country=US&sc_geo=NAMER&sc_outcome=acq&sc_detail=%2Bamazon%20%2Bec2%20%2Bcloud%20%2Bservice&sc_content={ad%20group}&sc_matchtype=b&sc_segment=488982705501&sc_medium=ACQ-P|PS-GO|Brand|Desktop|SU|Cloud%20Computing|EC2|US|EN|Sitelink&s_kwcid=AL!4422!3!488982705501!b!!g!!%2Bamazon%20%2Bec2%20%2Bcloud%20%2Bservice&ef_id=CjwKCAjwp_GJBhBmEiwALWBQkx2jsTDU3u8MY-eAPvcBAnOk4O0urYLKG3thxc1H3qi_AaGptRJMbBoC_4kQAvD_BwE:G:s&s_kwcid=AL!4422!3!488982705501!b!!g!!%2Bamazon%20%2Bec2%20%2Bcloud%20%2Bservice

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

****

###### Configuration

1. Activate **spark-defaults.conf** (this file would be loaded while spark server launches)
2. Add these lines into the file:
```
spark.executor.extraClassPath /jars/aws-java-sdk-bundle-1.11.375.jar:/lib/hadoop-aws-3.2.0.jar
spark.driver.extraClassPath /jars/aws-java-sdk-bundle-1.11.375.jar:/lib/hadoop-aws-3.2.0.jar
spark.driver.memory 8g
```

Some suggest using driver memory for 5g which provides the maximum throughput, but tests show 8g works best for my instance. Since I used only the local model - both the driver program and executor program exist in JVM instance, therefore I cannot set up the number of executors or number of cores for each executor. The current driver program with the executor uses 8 cores for all threads.

I may try running my programs on the clusters of instances, but for big data processing, a single machine seems workable. I am migrating my old deep learning project onto the instance and hope it might work with the TensorFlow program. However, this plan may be delayed due to my job hunting process still ongoing. 

**Configuration resources:**

[How to set Apache Spark Executor memory?](https://stackoverflow.com/questions/26562033/how-to-set-apache-spark-executor-memory)


[Spark executors and shuffle in local mode](ttps://stackoverflow.com/questions/67923596/spark-executors-and-shuffle-in-local-mode)
h


[Spark standalone configuration having multiple executors](https://stackoverflow.com/questions/39986507/spark-standalone-configuration-having-multiple-executors)


[Number of Executors in Spark Local Mode](https://stackoverflow.com/questions/44590284/number-of-executors-in-spark-local-mode)


[How to change number of executors in local mode?](https://stackoverflow.com/questions/52188910/how-to-change-number-of-executors-in-local-mode/52189023)


[Not able to setup spark.driver.cores](https://community.cloudera.com/t5/Support-Questions/Not-able-to-setup-spark-driver-cores/td-p/220152)


[spark.driver.cores setting in spark standalone cluster mode](https://stackoverflow.com/questions/56247441/spark-driver-cores-setting-in-spark-standalone-cluster-mode)


[Configuring memory and CPU options](https://www.ibm.com/docs/en/zpfas/1.1.0?topic=spark-configuring-memory-cpu-options)


[Apache Spark Memory Management](https://medium.com/analytics-vidhya/apache-spark-memory-management-49682ded3d42)


[Spark的driver理解和executor理解](https://blog.csdn.net/zpf336/article/details/83006569)


[一文详解Spark基本架构原理](http://www.uml.org.cn/bigdata/201906152.asp)


[Spark的一些配置总结](https://blog.csdn.net/baolibin528/article/details/54406540)


[Spark中executor-memory参数详解](https://blog.csdn.net/weixin_43668299/article/details/107036763?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.searchformbaiduhighlight&spm=1001.2101.3001.4242)


[如何设置Spark Executor Memory的大小](https://blog.csdn.net/weixin_43878293/article/details/92977940)


[Spark之如何设置Spark资源](https://www.cnblogs.com/GuixinChan/p/13503927.html)


**Temp files cleaning for Spark**

[Spark 1.6.2 清理临时文件](http://www.thirteenyu.com/2018/04/28/code-spark162-clean-tmp/)


[Apache Spark技术实战（一）Standalone部署模式下的临时文件清理&日志级别修改](https://developer.aliyun.com/article/60527)



[linux下查看某个文件或目录大小](https://blog.csdn.net/AlbertFly/article/details/69945050)


---
Copyright @ 2021 Zizhun Guo. All Rights Reserved.

