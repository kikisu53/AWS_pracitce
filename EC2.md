# EC2
第一顆磁碟
網卡設定 first

AMI 磁碟組成＋ram image
snapshots

key pairs 儲存公鑰和取名

ENI elasitic network interface
1. 限制連線
2. gateway

# S3
file-base storage: 網路芳鄰，NFS
block storage: 自己格式化 檔案系統
???


# cloudwatch
log
filter  good matrix
alarm: ok, insufficient, warning -> SNS, email, lambda

cloudwatch event:近即時
cloudtrial: 非即時

# vpc

# cloudfront

# dymonddb

# iam
user
role
	aws
	another aws account
	assuem role? access id, secreat token,  需要對方aws密碼
	identity providers

group 只是傳導工具

# kineses




實作
1. 建一個VPC, 裡面一個public subnet設定route table, igw, public ip
2. 裡面有 elb(elecity load-balbance), asg(auto scaling group),
3. asg裡面建立ami，建立user data: install git, nodejs, git clone https://github.com/clifflu/demo-07-12 [filename], cd [filename] & npm bootstrap

user data
```
#!/bin/bash
sudo yum install git
sudo curl --silent --location https://rpm.nodesource.com/setup_6.x | sudo bash -
sudo git clone https://github.com/clifflu/demo-07-12
cd /demo07-12
sudo npm bootstrap
```
 