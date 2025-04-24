# OTUS PRO Homework 30 iptables

## Домашняя работа 30: Фильтрация трафика - iptables

### Домашнее задание:
**1. Подготовка рабочего места**   
**2. Реализовать knocking port (centralRouter может попасть на ssh inetrRouter через knock скрипт пример в материалах).**   
**3. Добавить inetRouter2, который виден(маршрутизируется (host-only тип сети для виртуалки)) с хоста или форвардится порт через локалхост.**   
**4. Запустить nginx на centralServer**     
**5. Пробросить 80й порт на inetRouter2 8080**     
**6. Дефолт в инет оставить через inetRouter**
     
     
      
Формат сдачи ДЗ - vagrant + ansible  

---
## Выполнение задания:
### 1. Подготовка рабочего места:
Выполнение домашнего задания предполагает, что на компьютере установлен Vagrant+VirtualBox   
**[Как установить Vagrant на Debian 12](https://github.com/avlikh/Install_Vagrant_Debian12/blob/main/README.md)**   

Развернем Vagrant-стенд:
  - Создайте папку с проектом и зайдите в нее (например: /opt/otus/iptables):
```
mkdir -p /opt/otus/iptables ; cd /opt/otus/iptables
```
  - Клонируете проект с Github, набрав команду:
```
apt update -y && apt install git -y ; git clone https://github.com/avlikh/Otus_pro_30.git .
```
  - Запустите проект из папки, в которую склонировали проект (в нашем примере /opt/otus/iptables):
```
vagrant up
```
Результатом выполнения команды: 
<details>
<summary> vagrant up</summary> 

```
root@deb4likh:/opt/otus/iptables# vagrant up
Bringing machine 'inetRouter' up with 'virtualbox' provider...
Bringing machine 'inetRouter2' up with 'virtualbox' provider...
Bringing machine 'centralRouter' up with 'virtualbox' provider...
Bringing machine 'centralServer' up with 'virtualbox' provider...
Bringing machine 'office1Router' up with 'virtualbox' provider...
Bringing machine 'office1Server' up with 'virtualbox' provider...
Bringing machine 'office2Router' up with 'virtualbox' provider...
Bringing machine 'office2Server' up with 'virtualbox' provider...
==> inetRouter: You assigned a static IP ending in ".1" or ":1" to this machine.
==> inetRouter: This is very often used by the router and can cause the
==> inetRouter: network to not work properly. If the network doesn't work
==> inetRouter: properly, try changing this IP.
==> inetRouter: Importing base box 'bento/ubuntu-22.04'...
==> inetRouter: Matching MAC address for NAT networking...
==> inetRouter: You assigned a static IP ending in ".1" or ":1" to this machine.
==> inetRouter: This is very often used by the router and can cause the
==> inetRouter: network to not work properly. If the network doesn't work
==> inetRouter: properly, try changing this IP.
==> inetRouter: Checking if box 'bento/ubuntu-22.04' version '202407.23.0' is up to date...
==> inetRouter: A newer version of the box 'bento/ubuntu-22.04' for provider 'virtualbox' is
==> inetRouter: available! You currently have version '202407.23.0'. The latest is version
==> inetRouter: '202502.21.0'. Run `vagrant box update` to update.
==> inetRouter: Setting the name of the VM: iptables_inetRouter_1745471412869_98892
==> inetRouter: Clearing any previously set network interfaces...
==> inetRouter: Preparing network interfaces based on configuration...
    inetRouter: Adapter 1: nat
    inetRouter: Adapter 2: intnet
    inetRouter: Adapter 3: intnet
    inetRouter: Adapter 8: hostonly
==> inetRouter: Forwarding ports...
    inetRouter: 22 (guest) => 2222 (host) (adapter 1)
==> inetRouter: Running 'pre-boot' VM customizations...
==> inetRouter: Booting VM...
==> inetRouter: Waiting for machine to boot. This may take a few minutes...
    inetRouter: SSH address: 127.0.0.1:2222
    inetRouter: SSH username: vagrant
    inetRouter: SSH auth method: private key
    inetRouter: Warning: Connection reset. Retrying...
    inetRouter: Warning: Remote connection disconnect. Retrying...
    inetRouter:
    inetRouter: Vagrant insecure key detected. Vagrant will automatically replace
    inetRouter: this with a newly generated keypair for better security.
    inetRouter:
    inetRouter: Inserting generated public key within guest...
    inetRouter: Removing insecure key from the guest if it's present...


.................

==> office2Server: Fixed port collision for 22 => 2222. Now on port 2206.
==> office2Server: Clearing any previously set network interfaces...
==> office2Server: Preparing network interfaces based on configuration...
    office2Server: Adapter 1: nat
    office2Server: Adapter 2: intnet
    office2Server: Adapter 8: hostonly
==> office2Server: Forwarding ports...
    office2Server: 22 (guest) => 2206 (host) (adapter 1)
==> office2Server: Running 'pre-boot' VM customizations...
==> office2Server: Booting VM...
==> office2Server: Waiting for machine to boot. This may take a few minutes...
    office2Server: SSH address: 127.0.0.1:2206
    office2Server: SSH username: vagrant
    office2Server: SSH auth method: private key
    office2Server: Warning: Connection reset. Retrying...
    office2Server: Warning: Remote connection disconnect. Retrying...
    office2Server:
    office2Server: Vagrant insecure key detected. Vagrant will automatically replace
    office2Server: this with a newly generated keypair for better security.
    office2Server:
    office2Server: Inserting generated public key within guest...
    office2Server: Removing insecure key from the guest if it's present...
    office2Server: Key inserted! Disconnecting and reconnecting using new SSH key...
==> office2Server: Machine booted and ready!
==> office2Server: Checking for guest additions in VM...
    office2Server: The guest additions on this VM do not match the installed version of
    office2Server: VirtualBox! In most cases this is fine, but in rare cases it can
    office2Server: prevent things such as shared folders from working properly. If you see
    office2Server: shared folder errors, please make sure the guest additions within the
    office2Server: virtual machine match the version of VirtualBox you have installed on
    office2Server: your host and reload your VM.
    office2Server:
    office2Server: Guest Additions Version: 7.0.18
    office2Server: VirtualBox Version: 7.1
==> office2Server: Setting hostname...
==> office2Server: Configuring and enabling network interfaces...
==> office2Server: Mounting shared folders...
    office2Server: /opt/otus/iptables => /vagrant
==> office2Server: Running provisioner: shell...
    office2Server: Running: inline script
==> office2Server: Running provisioner: ansible...
    office2Server: Running ansible-playbook...

PLAY [all] *********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [inetRouter2]
ok: [centralServer]
ok: [office2Router]
ok: [centralRouter]
ok: [office1Router]
ok: [office2Server]
ok: [office1Server]
ok: [inetRouter]

TASK [Installing traceroute and curl packages on hosts] ************************
changed: [centralRouter]
changed: [office1Router]
changed: [centralServer]
changed: [inetRouter2]
changed: [office2Router]
changed: [office2Server]
changed: [office1Server]
changed: [inetRouter]

TASK [Installing knockd package on centralServer and inetRouter] ***************
skipping: [inetRouter2]
skipping: [office2Router]
skipping: [office1Router]
skipping: [centralRouter]
skipping: [office1Server]
skipping: [office2Server]
changed: [centralServer]
changed: [inetRouter]

TASK [Installing NGINX on centralServer] ***************************************
skipping: [inetRouter2]
skipping: [office2Router]
skipping: [office1Router]
skipping: [centralRouter]
skipping: [office1Server]
skipping: [inetRouter]
skipping: [office2Server]
changed: [centralServer]

TASK [Set up forward packages across routers] **********************************
skipping: [inetRouter2]
skipping: [centralServer]
skipping: [office2Router]
skipping: [office1Router]
skipping: [centralRouter]
skipping: [office1Server]
skipping: [inetRouter]
skipping: [office2Server]

TASK [Disabling default route on all hosts except inetRouter and inetRouter2] ***
skipping: [inetRouter2]
changed: [centralServer]
changed: [office1Server]
skipping: [inetRouter]
changed: [office2Router]
changed: [office1Router]
changed: [office2Server]
changed: [centralRouter]

TASK [Route configuring on hosts] **********************************************
changed: [office2Router]
changed: [centralServer]
changed: [office1Router]
changed: [centralRouter]
changed: [office1Server]
changed: [office2Server]
changed: [inetRouter]
changed: [inetRouter2]

PLAY [Configuring inetRouter] **************************************************

TASK [Gathering Facts] *********************************************************
ok: [inetRouter]

TASK [Disabling ufw service] ***************************************************
changed: [inetRouter]

TASK [Installing iptables-persistent service] **********************************
changed: [inetRouter]

TASK [Set up NAT] **************************************************************
changed: [inetRouter]

TASK [Configuring SSH service] *************************************************
ok: [inetRouter]

TASK [Restarting SSH service] **************************************************
changed: [inetRouter]

TASK [Turn on cnockd] **********************************************************
changed: [inetRouter]

TASK [Make /etc/systemd/system/knockd.service] *********************************
changed: [inetRouter]

TASK [Change config /etc/knockd.conf] ******************************************
changed: [inetRouter]

TASK [Start knockd service] ****************************************************
changed: [inetRouter]

PLAY [Configuring inetRouter2] *************************************************

TASK [Gathering Facts] *********************************************************
ok: [inetRouter2]

TASK [Disabling ufw service] ***************************************************
changed: [inetRouter2]

TASK [Installing iptables-persistent service] **********************************
changed: [inetRouter2]

TASK [Set up NAT] **************************************************************
changed: [inetRouter2]

PLAY RECAP *********************************************************************
centralRouter              : ok=4    changed=3    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
centralServer              : ok=6    changed=5    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
inetRouter                 : ok=14   changed=11   unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
inetRouter2                : ok=7    changed=5    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
office1Router              : ok=4    changed=3    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
office1Server              : ok=4    changed=3    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
office2Router              : ok=4    changed=3    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
office2Server              : ok=4    changed=3    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0


```
</details>

станут созданные 8 виртуальных машин с OS Ubuntu 22.04: 
**centralRouter**     
**centralServer**     
**inetRouter**     
**inetRouter2**     
**office1Router**     
**office1Server**     
**office2Router**     
**office2Server**
     
---
### 2. Реализовать knocking port (centralRouter может попасть на ssh inetrRouter через knock скрипт пример в материалах).
После запуска команды vagrant up была развернут стенд из домашнего задания "Архитектура сетей" с небольшими изменениями (был добавлен хост: inetRouter2).     
Далее, Vagrant выполнил Ansible playbook **"provision.yml"** 
      
Реализована следующая последовательность  knocking ports: 1111 2222 3333. Сменить потры на свои можно в файле глобальных переменныз Ansible: ./group_vars/all.yaml     
     
Проверим, что knocking port работает корректно.     
Для этого, зайдем на centralServer и попробуем зайти с него на ssh inetRouter

<details>
<summary> результат: </summary>

```
root@deb4likh:/opt/otus/iptables# vagrant ssh centralServer
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-116-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Thu Apr 24 12:30:23 PM UTC 2025

  System load:           0.0
  Usage of /:            13.2% of 30.34GB
  Memory usage:          24%
  Swap usage:            0%
  Processes:             133
  Users logged in:       0
  IPv4 address for eth0: 10.0.2.15
  IPv6 address for eth0: fd00::a00:27ff:fec8:9864


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento

Use of this system is acceptance of the OS vendor EULA and License Agreements.
Last login: Thu Apr 24 05:22:59 2025 from 10.0.2.2
vagrant@centralServer:~$ sudo -i
root@centralServer:~# ssh vagrant@192.168.255.1
ssh: connect to host 192.168.255.1 port 22: Connection timed out
```
</details>

Видим, что доступ закрыт.    

Попробуем зайти используя последовательность knocking pors 1111 2222 3333:

```
root@centralServer:~# knock 192.168.255.1 1111 2222 3333
```
```
root@centralServer:~# ssh vagrant@192.168.255.1
vagrant@192.168.255.1's password: 
```
     
Видим что доступ есть (порт 22 открылся через knocking)
     
     
### 3. Добавить inetRouter2, который виден(маршрутизируется (host-only тип сети для виртуалки)) с хоста или форвардится порт через локалхост.**   
Реализовано в рамках vagrant+Ansible     
     
### 4. Запустить nginx на centralServer**     
Реализовано в рамках vagrant+Ansible    

### 5. Пробросить 80й порт на inetRouter2 8080**     
Реализовано в рамках vagrant+Ansible     
     
### 6. Дефолт в инет оставить через inetRouter**
Реализовано в рамках vagrant+Ansible     

Проброс порта 8080 с хостовой машины (10.68.7.41) на машину inetRouter2 делался вручную из интерфейса Virtualbox.

Проверим, что NGINX отзывается на запросы к хостовой машине по порту 8080:

![image](https://github.com/avlikh/Otus_pro_30/blob/main/nginx.png)
