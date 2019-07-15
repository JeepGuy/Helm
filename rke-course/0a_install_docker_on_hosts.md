# Install Docker on AWS Linux2 AMI instances

1. ssh to ec2 instance.

      ssh -i ../path-to-dir/<correct-pem-file>.pem  ec2-user@<ip-addr>

      * run  sudo yum update

      ```
       Don't forget to Chmod the key to 600 or 400 - both work.    I've seen both in the docs.

      @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
      @         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
      @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
      Permissions 0644 for '../AWS_private/rancher-course.pem' are too open.
      It is required that your private key files are NOT accessible by others.
      This private key will be ignored.
      Load key "../AWS_private/rancher-course.pem": bad permissions
      Permission denied (publickey,gssapi-keyex,gssapi-with-mic).

      ```

2. Install Docker  (if not using the user-data.sh script)

      yum install docker
           You need to be root to perform this command.

      sudo su 

      yum install docker 

      ```
      Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
      amzn2-core                                                                      | 2.4 kB  00:00:00     
      Resolving Dependencies
      --> Running transaction check
      ---> Package docker.x86_64 0:18.06.1ce-8.amzn2 will be installed
      --> Processing Dependency: pigz for package: docker-18.06.1ce-8.amzn2.x86_64
      --> Processing Dependency: libcgroup for package: docker-18.06.1ce-8.amzn2.x86_64
      --> Processing Dependency: libltdl.so.7()(64bit) for package: docker-18.06.1ce-8.amzn2.x86_64
      --> Running transaction check
      ---> Package libcgroup.x86_64 0:0.41-15.amzn2 will be installed
      ---> Package libtool-ltdl.x86_64 0:2.4.2-22.2.amzn2.0.2 will be installed
      ---> Package pigz.x86_64 0:2.3.4-1.amzn2.0.1 will be installed
      --> Finished Dependency Resolution

      Dependencies Resolved

      =======================================================================================================
       Package               Arch            Version                        Repository                  Size
      =======================================================================================================
      Installing:
       docker                x86_64          18.06.1ce-8.amzn2              amzn2extra-docker           37 M
      Installing for dependencies:
       libcgroup             x86_64          0.41-15.amzn2                  amzn2-core                  65 k
       libtool-ltdl          x86_64          2.4.2-22.2.amzn2.0.2           amzn2-core                  49 k
       pigz                  x86_64          2.3.4-1.amzn2.0.1              amzn2-core                  81 k

      Transaction Summary
      =======================================================================================================
      Install  1 Package (+3 Dependent packages)

      Total download size: 37 M
      Installed size: 151 M
      Is this ok [y/d/N]: 
      ```

      Answer yes (y)

      ```
      Downloading packages:
      (1/4): libcgroup-0.41-15.amzn2.x86_64.rpm                                       |  65 kB  00:00:00     
      (2/4): pigz-2.3.4-1.amzn2.0.1.x86_64.rpm                                        |  81 kB  00:00:00     
      (3/4): libtool-ltdl-2.4.2-22.2.amzn2.0.2.x86_64.rpm                             |  49 kB  00:00:00     
      (4/4): docker-18.06.1ce-8.amzn2.x86_64.rpm                                      |  37 MB  00:00:00     
      -------------------------------------------------------------------------------------------------------
      Total                                                                   60 MB/s |  37 MB  00:00:00     
      Running transaction check
      Running transaction test
      Transaction test succeeded
      Running transaction
        Installing : libtool-ltdl-2.4.2-22.2.amzn2.0.2.x86_64                                            1/4 
        Installing : pigz-2.3.4-1.amzn2.0.1.x86_64                                                       2/4 
        Installing : libcgroup-0.41-15.amzn2.x86_64                                                      3/4 
        Installing : docker-18.06.1ce-8.amzn2.x86_64                                                     4/4 
        Verifying  : libcgroup-0.41-15.amzn2.x86_64                                                      1/4 
        Verifying  : pigz-2.3.4-1.amzn2.0.1.x86_64                                                       2/4 
        Verifying  : libtool-ltdl-2.4.2-22.2.amzn2.0.2.x86_64                                            3/4 
        Verifying  : docker-18.06.1ce-8.amzn2.x86_64                                                     4/4 

      Installed:
        docker.x86_64 0:18.06.1ce-8.amzn2                                                                    

      Dependency Installed:
        libcgroup.x86_64 0:0.41-15.amzn2              libtool-ltdl.x86_64 0:2.4.2-22.2.amzn2.0.2             
        pigz.x86_64 0:2.3.4-1.amzn2.0.1 

      ```

3. Add operating user to Docker Group      (THIS NEEDS MORE RESEARCH)
    - so you can execute docker commands as user not root.
        (as of 10 July 2019 if using the AWS Linux2 AMI then the docker install is 
          at a different location and you do not need to add the user to the docker group)

  (If using CentOS must edit the docker file:   sudo vim /etc/docker/daemon.json , remove entry and add the three lines of json below)
      ```
      {
        "group": "dockerroot"
      }

          sudo find / -name docker 
          /etc/sysconfig/docker
          /etc/docker
          /var/lib/docker
          /usr/bin/docker
          /usr/share/bash-completion/docker
          /usr/libexec/docker

        
          # sudo usermod -aG docker $USER # doesn't run if you are sudo -i
          sudo usermod -aG docker ec2-user

      ```

4. Set docker to start on boot
    systemctl docker enable
    systemctl restart docker

      systemctl enable docker

      ERROR MESSAGE: 
      Failed to execute operation: The name org.freedesktop.PolicyKit1 was not provided by any .service files

      Solution: Use SUDO 

      sudo systemctl enable docker
      Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
      
      sudo systemctl restart docker
     


```
      pstree
      systemd─┬─acpid
              ├─2*[agetty]
              ├─amazon-ssm-agen───7*[{amazon-ssm-agen}]
              ├─atd
              ├─auditd───{auditd}
              ├─chronyd
              ├─crond
              ├─dbus-daemon
              ├─2*[dhclient]
              ├─dockerd─┬─docker-containe───9*[{docker-containe}] >*******************************
              │         └─10*[{dockerd}]
              ├─gssproxy───5*[{gssproxy}]
              ├─irqbalance───{irqbalance}
              ├─lsmd
              ├─lvmetad
              ├─master─┬─pickup
              │        └─qmgr
              ├─rngd
              ├─rpcbind
              ├─rsyslogd───2*[{rsyslogd}]
              ├─sshd─┬─4*[sshd───sshd───bash]
              │      ├─sshd───sshd───bash───sudo───su───bash
              │      └─sshd───sshd───bash───pstree
              ├─systemd-journal
              ├─systemd-logind
              └─systemd-udevd
```


