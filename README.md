# Nexus-Demo
This repo demos the installation of nexus on the ubuntu server and accessing nexus ui from a Web browser

- Spin up an EC2 instance on AWS, allow TCP connections on port 22 in the inbound rule for the security group attached to this instance, allow TCP connection on port 8081 to access Nexus service
![image](https://github.com/hemu07/Nexus-demo/assets/90203539/1da8f2bd-5fa4-4b42-b41b-240627534806)

-Remote server (ec2 instance) requirements:
  - 8 GB RAM (4GB will also work)
  - 2 vCPU
  - rsa key used to launch instance safely downloaded locally
  - Ubuntu AMI (Ubuntu server 22.0 LTS)

- once the instance is running connect to the instance either via ec2 instance connect or SSH into the server using rsa key-pair
-  ![image](https://github.com/hemu07/Nexus-demo/assets/90203539/2408be58-5c46-4fd0-ac61-8028abe24311)

