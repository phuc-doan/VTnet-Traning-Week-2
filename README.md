# VTnet-Traning-Week-2

![186474723_4060144630720750_1954254143321618585_n](https://user-images.githubusercontent.com/83824403/118011564-6235b100-b37a-11eb-9635-a0e0c31cb33b.png)
![186474723_4060144630720750_1954254143321618585_n](https://user-images.githubusercontent.com/83824403/118011588-695cbf00-b37a-11eb-85d0-538f87c623e2.png)
![186402497_376888587001479_4865151441127697097_n](https://user-images.githubusercontent.com/83824403/118011593-6d88dc80-b37a-11eb-857c-a44c45c99bc1.png)
![184586293_315092610053474_6176911539195705320_n](https://user-images.githubusercontent.com/83824403/118011596-6e217300-b37a-11eb-9ba8-325c0960473e.png)
![183274249_477380299999096_1043020392940805635_n (1)](https://user-images.githubusercontent.com/83824403/118011604-6f52a000-b37a-11eb-9700-1bf6a5a24b07.png)
![186474723_4060144630720750_1954254143321618585_n](https://user-images.githubusercontent.com/83824403/118012250-0e779780-b37b-11eb-8a79-6f0b995df9d4.png)
![186402497_376888587001479_4865151441127697097_n](https://user-images.githubusercontent.com/83824403/118012261-10d9f180-b37b-11eb-8fdf-b676d47cb264.png)
![184586293_315092610053474_6176911539195705320_n](https://user-images.githubusercontent.com/83824403/118012268-120b1e80-b37b-11eb-945b-31033090ec90.png)
![183274249_477380299999096_1043020392940805635_n (1)](https://user-images.githubusercontent.com/83824403/118012273-12a3b500-b37b-11eb-96ec-8a4549dff9a2.png)


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
    
# Step 7: If you wanna check, you can type: sudo docker container ps
and Let's see!
