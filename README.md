# Nexus-Demo
This repo demos the installation of Nexus on the Ubuntu server and accessing Nexus UI from a Web browser

- Spin up an EC2 instance on AWS, allow TCP connections on port 22 in the inbound rule for the security group attached to this instance, and allow TCP connection on port 8081 to access Nexus service
    ![image](https://github.com/hemu07/Nexus-demo/assets/90203539/1da8f2bd-5fa4-4b42-b41b-240627534806)

- Remote server (ec2 instance) requirements:
  - 8 GB RAM (4GB will also work)
  - 2 vCPU
  - RSA key used to launch instance safely downloaded locally
  - Ubuntu AMI (Ubuntu server 22.0 LTS)

- once the instance is running connect to the instance either via ec2 instance connect or SSH into the server using rsa key-pair
   ![image](https://github.com/hemu07/Nexus-demo/assets/90203539/2408be58-5c46-4fd0-ac61-8028abe24311)

- Run the below commands as a prerequisite to installing Nexus:
  - sudo apt update
  - sudo apt install openjdk-8-jre-headless
  - sudo adduser nexus
  - sudo usermod -aG sudo nexus
  ![image](https://github.com/hemu07/Nexus-demo/assets/90203539/308d49a6-22a1-4227-a67e-233ca31b9b5e)
    we will use this user to run the nexus service, as a security best practice is not to grant more permissions than needed to work with
  - cd /opt
- Installing Nexus
  - sudo wget https://download.sonatype.com/nexus/3/nexus-3.66.0-02-unix.tar.gz
  - sudo chown -R nexus:nexus sonatype-work
  - sudo chown -R nexus:nexus nexus-3.66.0-02
  - sudo vi nexus.rc
    > enter the insert mode  by pressing ESC+i and uncomment the first line and user as nexus
    > ![image](https://github.com/hemu07/Nexus-demo/assets/90203539/ac75261c-f73c-4ebb-9dfa-c48056aa70c6)
- Configuring Nexus service
  - sudo vi /etc/systemd/system/nexus.service
  -  add below script to run nexus as service
=============================================================

    [Unit]
    Description=Nexus Repository Manager
    After=network.target
    
    [Service]
    Type=forking
    LimitNOFILE=65536
    User=nexus
    Group=nexus
    WorkingDirectory=/opt/nexus-3.66.0-02
    ExecStart=/opt/nexus-3.66.0-02/bin/nexus start
    ExecStop=/opt/nexus-3.66.0-02/bin/nexus stop
    Restart=on-abort
    
    [Install]
    WantedBy=multi-user.target

=============================================================

link to the script: [Nexus-documentation] (https://help.sonatype.com/en/run-as-a-service.html#:~:text=%5BUnit%5D%0ADescription%3Dnexus%20service%0AAfter%3Dnetwork.target%0A%20%20%0A%5BService%5D%0AType%3Dforking%0ALimitNOFILE%3D65536%0AExecStart%3D/etc/init.d/nexus%20start%0AExecStop%3D/etc/init.d/nexus%20stop%20%0AUser%3Dnexus%0ARestart%3Don%2Dabort%0ATimeoutSec%3D600%0A%20%20%0A%5BInstall%5D%0AWantedBy%3Dmulti%2Duser.target)

  - sudo ln -s /opt/nexus-3.66.0-02/bin/nexus /etc/init.d/nexus
  - su - nexus
- Running Nexus as nexus user:
  - sudo update-rc.d nexus defaults
  - sudo systemctl daemon-reload
  - sudo systemctl enable nexus.service
  - sudo systemctl start nexus.service
  - sudo systemctl status nexus
  - ps aux | grep nexus
  - sudo apt install net-tools
  - netstat -lnpt

  ![image](https://github.com/hemu07/Nexus-demo/assets/90203539/e0f9adcb-e17d-4f25-9a2e-2dcdf8962e84)
  - we can see the service running on port 8081
  - Now in the new tab access the UI by giving :-
  - HTTP://public-IPv4-ofserver:8081

  ![image](https://github.com/hemu07/Nexus-demo/assets/90203539/fec73382-f23b-422c-9e68-a6fda63c96fc)
  - cat /opt/sonatype-work/nexus3/admin.password
  - give this password to signin for the first time in UI , which will redirect to reset the password, once done we can see the below output:
  ![image](https://github.com/hemu07/Nexus-demo/assets/90203539/c1e220e7-66e1-4056-b988-4a0b2f089c57)
  - check the logs using below command:
  - tail -f /opt/sonatype-work/nexus3/log/nexus.log
