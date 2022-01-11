# Docker on AWS

## Create a Docker EC2 using Ansible only
- Playbook script to create a EC2 instance:
```
---
- name: EC2 launch
  hosts: webserver
  connection: local
  vars:
     ansible_python_interpreter: /usr/bin/python3
  tasks:
  - name: Launch EC2
    ec2:
      key_name: eng99
      instance_type: t2.micro
      image: ami-07d8796a2b0f8d29c
      region: eu-west-1
      wait: yes
      count: 1
      vpc_subnet_id: subnet-0bf2c8e193562933b
      assign_public_ip: yes
      instance_tags:
        Name: eng99_raj_docker_instance
```
- `sudo ansible-playbook aws.yml --ask-vault-pass` to launch an EC2
- Must terminate instance from AWS console before running the same playbook again otherwise another instance will launch. 
- Followed this [guide](https://www.mindbowser.com/how-to-create-ec2-instances-using-ansible/)

## Docker Instance
- Simple Docker insallation on the EC2 
```
---
- name: docker provisioning
  hosts: docker
  gather_facts: true
  become: true
  tasks:
  - name: update and upgrade
    shell: sudo apt-get update -y && sudo apt-get upgrade -y
  - name: install cURL
    shell: sudo apt-get install curl -y
  - name: docker install
    shell: curl -fsSl https://get.docker.com/ | sh
  - name: nginx
    shell: sudo docker pull nginx
```
- Download Nginx image from docker 
- run docker commands to run nginx and load web page on the browser


## DockerFile
- Essentially a provisioning file to manage containers

