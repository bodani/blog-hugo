---
title: "vagrant"
date: 2020-07-13T09:32:49+08:00
draft: false
---

#### 介绍

通常用vagrant 来管理VirtualBox ,VMWare，方便测试环境的创建，销毁。不常折腾用virtualbox, 反复折腾用vagrant。

#### 简单使用

##### 安装

[下载virtualbox](https://www.virtualbox.org/wiki/Downloads)

[下载vagrant](https://www.vagrantup.com/downloads.html)


##### 常用方法

```
     box             manages boxes: installation, removal, etc.
     cloud           manages everything related to Vagrant Cloud
     destroy         stops and deletes all traces of the vagrant machine
     halt            stops the vagrant machine
     reload          restarts vagrant machine, loads new Vagrantfile configuration
     resume          resume a suspended vagrant machine
     ssh             connects to machine via SSH
     status          outputs status of the vagrant machine
     up              starts and provisions the vagrant environment 
```

##### 配置文件

cat Vagrantfile
```
Vagrant.configure("2") do |config|
    config.vm.box = "centos/7"
    config.vm.box_check_update = false
    config.ssh.insert_key = false

    config.vm.define "node0", primary: true do |s|
        s.vm.hostname = "node0"
        s.vm.network "private_network", ip: "10.10.10.10"
        s.vm.provider "virtualbox" do |v|
            v.linked_clone = true
            v.customize [
                    "modifyvm", :id,
                    "--memory", 2048,
                    "--cpus", "2",
                    "--nictype1", "virtio",
                    "--nictype2", "virtio",
                    "--hwvirtex", "on",
                    "--ioapic", "on",
                    "--rtcuseutc", "on",
                    "--vtxvpid", "on",
                    "--largepages", "on"
                ]
        end
        s.vm.provision "shell", inline: "/vagrant/bin/setup-node.sh"
    end

    config.vm.define "node1" do |s|
        s.vm.hostname = "node1"
        s.vm.network "private_network", ip: "10.10.10.11"
        s.vm.provider "virtualbox" do |v|
            v.linked_clone = true
            v.customize [
                    "modifyvm", :id,
                    "--memory", 1024,
                    "--cpus", "1",
                    "--nictype1", "virtio",
                    "--nictype2", "virtio",
                    "--hwvirtex", "on",
                    "--ioapic", "on",
                    "--rtcuseutc", "on",
                    "--vtxvpid", "on",
                    "--largepages", "on"
                ]
        end
        s.vm.provision "shell", inline: "/vagrant/bin/setup-node.sh"
    end

    config.vm.define "node2" do |s|
        s.vm.hostname = "node2"
        s.vm.network "private_network", ip: "10.10.10.12"
        s.vm.provider "virtualbox" do |v|
            v.linked_clone = true
            v.customize [
                    "modifyvm", :id,
                    "--memory", 1024,
                    "--cpus", "1",
                    "--nictype1", "virtio",
                    "--nictype2", "virtio",
                    "--hwvirtex", "on",
                    "--ioapic", "on",
                    "--rtcuseutc", "on",
                    "--vtxvpid", "on",
                    "--largepages", "on"
                ]
        end
        s.vm.provision "shell", inline: "/vagrant/bin/setup-node.sh"
    end

        config.vm.define "node3" do |s|
            s.vm.hostname = "node3"
            s.vm.network "private_network", ip: "10.10.10.13"
            s.vm.provider "virtualbox" do |v|
                v.linked_clone = true
                v.customize [
                        "modifyvm", :id,
                        "--memory", 1024,
                        "--cpus", "1",
                        "--nictype1", "virtio",
                        "--nictype2", "virtio",
                        "--hwvirtex", "on",
                        "--ioapic", "on",
                        "--rtcuseutc", "on",
                        "--vtxvpid", "on",
                        "--largepages", "on"
                    ]
            end
        s.vm.provision "shell", inline: "/vagrant/bin/setup-node.sh"
        end
end
```
