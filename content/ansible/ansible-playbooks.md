---
title: "Ansible Playbooks"
date: 2018-10-25T15:47:50+08:00
draft: false
---
#### Playbook核心元素

##### hosts 
一个或多个组或主机的 patterns,以逗号为分隔符 。 
```
- hosts: webservices
  remote_user: root
```
##### tasks  
任务集 
```
 tasks:
    - name: install httpd
      yum: name=httpd

    - name: start httpd
      service: name=httpd state=started
```
##### Handlers 和 notity 
由特定条件触发的操作，满足条件方才执行，否则不执行。
Handlers也是task列表，这些task与前述的tasks并没有本质上的不同,用于当关注的资源发生变化时，才会采取一定的操作。

```
- hosts: webs
  remote_user: root

  tasks:
    - name: install httpd
      yum: name=httpd

    - name: change httpd.conf
      copy: src=/app/httpd.conf dest=/etc/httpd/conf/ backup=yes
      notify: restart httpd             > 在 notify 中定义内容一定要和handlers中定义的 - name 内容一样，这样才能达到触发的效果，否则会不生效。
    - name: start httpd
      service: name=httpd state=started

    - name: wall http status
      shell: /usr/bin/wall `ss -nltp|grep httpd`

  handlers:
    - name: restart httpd           > 只有接收到通知才会执行这里的任务
      service: name=httpd state=restarted
```


