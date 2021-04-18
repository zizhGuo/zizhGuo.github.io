---
layout: post
title:  "SOAP Web Service Deployment (AWS EC2, JAVA)"
date:   2020-04-10 20:12:12
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---

Author: Zizhun Guo 4/10/2020

Goal:
Deploy a web service (API) on AWS instance.
<!-- MarkdownTOC -->

- [Tech Stack and Tools](#tech-stack-and-tools)
- [Test Links](#test-links)
- [Results](#results)
- [Problem Solving Plan](#problem-solving-plan)
- [Process](#process)
- [Post-mortem：](#post-mortem%EF%BC%9A)
- [Resources and References](#resources-and-references)

<!-- /MarkdownTOC -->


##### Tech Stack and Tools
1. Java 1.8
2. AWS EC2
3. GlassFish 4.0
4. Maven 3.1
5. Netbeans
6. WinSCP
7. PuTTy
8. Ubuntu 18

##### Test Links
1. WSDL：[http://3.16.214.41:8080/zizhunguo-1.0/Excelsior?WSDL](http://3.16.214.41:8080/zizhunguo-1.0/Excelsior?WSDL)
2. JSP index：[http://3.16.214.41:8080/zizhunguo-1.0/](http://3.16.214.41:8080/zizhunguo-1.0/)
3. Web Service Invocation Tester: [http://3.16.214.41:8080/zizhunguo-1.0/Excelsior?Tester](http://3.16.214.41:8080/zizhunguo-1.0/Excelsior?Tester)
4. GlassFish server：[http://3.16.214.41:4848/](http://3.16.214.41:4848/)

##### Results
![03](https://i.imgur.com/BUxtY1x.png)
![04](https://i.imgur.com/HuWKnee.png)
![06](https://i.imgur.com/2oDpypk.
![02](https://i.imgur.com/K7ISYWB.png)



#### Problem Solving Plan
1. Deploy the web service on local server
2. AWS EC2 instance estabilishment
3. Deploy it on cloud server

#### Process
1. AWS EC2 registration.
2. Instance launch (ubuntu)
2. Ping public IP， make sure adding the inbound rule to security group
3. Install WinSCP for remote transfering the file, Putty generator for keys conversion
4. Ubuntu install & configure and Java，Glassfish，Maven, etc。
5. Export java web app into .war file，deploy it on glassfish。

#### Post-mortem：
- Port setting issue, including windows firewall's inboud rule, which requires port 4848，8080 and 8181. It also need to be set in security group
- GlassFish version issue

#### Resources and References
setjavapath on Ubuntu：
https://vitux.com/how-to-setup-java_home-path-in-ubuntu/

glssfish bash:
https://www.javatpoint.com/how-to-run-glassfish-in-ubuntu

inbound/outbound rule:
https://blog.csdn.net/mid120/article/details/52155021
https://blog.csdn.net/kairen6645/article/details/89883353

enable remote access glassfish:
https://portal.jvmhost.com/knowledgebase/article/226/glassfish--secure-admin-must-be-enabled-to-access-the-das-remotely/
https://askubuntu.com/questions/1098413/secure-admin-must-be-enabled-to-access-the-das-remotely-access-glassfish-confi
http://yp-amazonwebservices.blogspot.com/2013/07/resolved-glassfish4-configuration-error.html

GlassFish Version: (downgrade to 4)
https://www.cloudmantra.net/blog/glassfish-installation-on-ec2-amazon-linux/


AWS Public IP can't login：
https://aws.amazon.com/cn/premiumsupport/knowledge-center/ec2-connect-internet-gateway/
https://bbs.csdn.net/topics/391052562

Firewall port setting：
https://blog.csdn.net/kairen6645/article/details/89883353

Ping IP：
https://blog.csdn.net/daerzei/article/details/81169349
https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/using-instance-addressing.html

AWS official user doc: security group rules references：
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-group-rules-reference.html#sg-rules-ping

AWS EC2 instance connection：
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html

AWS EC2 web app deployment：
https://blog.csdn.net/weixin_30267785/article/details/99798834

Putty windows connection：
https://docs.aws.amazon.com/zh_cn/AWSEC2/latest/UserGuide/putty.html#putty-private-key

阿里云部署javaweb项目：
https://www.jianshu.com/p/105c4aab7a41

Winscp file transfering：
https://blog.csdn.net/yuan13091324/article/details/50246415

Sample Web App：
https://github.com/AKSarav/SampleWebApp/blob/master/SampleWebApp-NB.zip

Glassfish Deployment：
https://blog.csdn.net/u012948976/article/details/52080005
https://blog.csdn.net/x_studying/article/details/39827097

Glassfish deploy warfile：
https://dzone.com/articles/how-deploy-war-file-using

JAR VS WAR：
https://blog.csdn.net/u012110719/article/details/44260417
Netbeans war包：
https://www.coder.work/article/3470616

AWS Web free tutorial：
https://aws.amazon.com/cn/getting-started/use-cases/websites/

Python web app tutorial：
https://aws.amazon.com/cn/getting-started/projects/build-modern-app-fargate-lambda-dynamodb-python/?trk=gs_card

Glassfish configure Java：
https://blog.csdn.net/pxmxx/article/details/80106239
https://blog.csdn.net/gao36951/article/details/73321345