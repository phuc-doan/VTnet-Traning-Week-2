# VTnet-Traning-Week-2


# Pratice 1 - Install Ansible and Deploy WordPress(docker) using Ansible
# Step 1: Install Ansible
Install on Ubuntu

$ sudo apt update
$ sudo apt install software-properties-common
$ sudo apt-add-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
Check install Ansible successful

# Step 2: Create your own ansible.cfg file on your folder
After create ansible.cfg file, you can run

# Step 3: Create your own inventory.ini file on your folder
inventory.ini define hosts, groups you want to manage

my inventory.ini file

[ubuntu1]
192.168.1.127

[ubuntu2]
192.168.1.128

[dvp:vars]
ansible_user=dvp
ansible_ssh_pass=1

[dp:vars]
ansible_user=dp
ansible_ssh_pass=1

Then, you change ansible.cfg file

[defaults]
host_key_checking = False
inventory = /home/Documents/test-ansible/inventory.ini
remote_user = dvp

# Step 4: Test Ansible with your own ansible.cfg file
ping all machine you defined on your own inventory.ini file

# Step 5: Install Docker & Docker Compose on group web
---
- name: install docker
  hosts: ubuntu1
  gather_facts: false

  tasks:
  - name: ping
    ping:
    register: result
  - name: install-docker
    become: yes
    apt:
      name: docker.io
      state: present
  - name: install docker-compose
    become: yes
    apt:
      name: docker-compose
      state: present
  - name: ensure docker service is running
    become: yes
    service:
      name: docker
      state: started
      
![186474723_4060144630720750_1954254143321618585_n](https://user-images.githubusercontent.com/83824403/118013066-eb011c80-b37b-11eb-8dfe-83081100247a.png)


# Step 6: Deploy WordPress with Docker-Compose
- name: deploy wordpress
  hosts: ubuntu1
  gather_facts: false

  tasks:
  - name: pull docker compose yaml
    become: yes
    get_url:
     url: https://raw.githubusercontent.com/bitnami/bitnami-docker-wordpress/master/docker-compose.yml
     dest: /home/dvp/docker-compose.yml
  - name: run docker compose
    become: yes
    command: docker-compose up -d
  - name: ensure docker-compose container is running
    become: yes
    docker_container_info:
     name: my_container
    register: result 
    
     ![186402497_376888587001479_4865151441127697097_n](https://user-images.githubusercontent.com/83824403/118013120-f8b6a200-b37b-11eb-8303-71432ad4bc2c.png)
![183274249_477380299999096_1043020392940805635_n (1)](https://user-images.githubusercontent.com/83824403/118013128-fa806580-b37b-11eb-84ad-3adf52c8fa38.png)

    
# Step 7: If you wanna check, you can type: sudo docker container ps
and Let's see!

![184586293_315092610053474_6176911539195705320_n](https://user-images.githubusercontent.com/83824403/118012587-6dd5a780-b37b-11eb-82cd-3ab48af19e3e.png)
