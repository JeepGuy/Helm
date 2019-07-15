#!/bin/bash
#
# AWS Linux2 AMI instances user-data-sh script

# Run as root
sudo -i 

sudo yum update -y

yum install docker -y

# Add operating user to Docker Group
#    usermod -aG docker $USER # Doesn't work cause you are running as root.
usermod -aG docker ec2-user

# Turn on docker, Set docker to start on boot, and restart docker to apply settings.
systemctl docker enable

systemctl enable docker

systemctl restart docker
