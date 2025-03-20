# OTUS PRO Homework 38 LDAP

## Домашняя работа 38: LDAP. Централизованная авторизация и аутентификация

### Домашнее задание:
Описание домашнего задания:

**1. Подготовка рабочего места**

**2. Установить FreeIPA**    
    
**3. Написать Ansible-playbook для конфигурации клиента**

---
### 1. Подготовка рабочего места:
Выполнение домашнего задания предполагает, что на компьютере установлен Vagrant+VirtualBox   
**[Как установить Vagrant на Debian 12](https://github.com/avlikh/Install_Vagrant_Debian12/blob/main/README.md)**   

Развернем Vagrant-стенд:
  - Создайте папку с проектом и зайдите в нее (например: /opt/otus/backup):
```
mkdir -p /opt/otus/ldap ; cd /opt/otus/ldap
```
  - Клонируете проект с Github, набрав команду:
```
apt update -y && apt install git -y ; git clone https://github.com/avlikh/Otus_pro_38.git .
```
  - Запустите проект из папки, в которую склонировали проект (в нашем примере /opt/otus/ldap):
```
vagrant up
```
<details>
<summary> Результатом выполнения команды vagrant up станут созданные и настроенные 3 виртуальные машины: </summary>

```
root@deb4likh:/opt/otus/ldap# vagrant up
Bringing machine 'ipa.otus.lan' up with 'virtualbox' provider...
Bringing machine 'client1.otus.lan' up with 'virtualbox' provider...
Bringing machine 'client2.otus.lan' up with 'virtualbox' provider...
==> ipa.otus.lan: Importing base box 'generic/centos8'...
==> ipa.otus.lan: Matching MAC address for NAT networking...
==> ipa.otus.lan: Checking if box 'generic/centos8' version '4.3.12' is up to date...
==> ipa.otus.lan: Setting the name of the VM: ldap_ipaotuslan_1742425241710_5949
==> ipa.otus.lan: Clearing any previously set network interfaces...
==> ipa.otus.lan: Preparing network interfaces based on configuration...
    ipa.otus.lan: Adapter 1: nat
    ipa.otus.lan: Adapter 2: hostonly
==> ipa.otus.lan: You are trying to forward to privileged ports (ports <= 1024). Most
==> ipa.otus.lan: operating systems restrict this to only privileged process (typically
==> ipa.otus.lan: processes running as an administrative user). This is a warning in case
==> ipa.otus.lan: the port forwarding doesn't work. If any problems occur, please try a
==> ipa.otus.lan: port higher than 1024.
==> ipa.otus.lan: Forwarding ports...
    ipa.otus.lan: 443 (guest) => 443 (host) (adapter 1)
    ipa.otus.lan: 22 (guest) => 2222 (host) (adapter 1)
==> ipa.otus.lan: Running 'pre-boot' VM customizations...
==> ipa.otus.lan: Booting VM...
==> ipa.otus.lan: Waiting for machine to boot. This may take a few minutes...
    ipa.otus.lan: SSH address: 127.0.0.1:2222
    ipa.otus.lan: SSH username: vagrant
    ipa.otus.lan: SSH auth method: private key
    ipa.otus.lan:
    ipa.otus.lan: Vagrant insecure key detected. Vagrant will automatically replace
    ipa.otus.lan: this with a newly generated keypair for better security.
    ipa.otus.lan:
    ipa.otus.lan: Inserting generated public key within guest...
    ipa.otus.lan: Removing insecure key from the guest if it's present...
    ipa.otus.lan: Key inserted! Disconnecting and reconnecting using new SSH key...
==> ipa.otus.lan: Machine booted and ready!
==> ipa.otus.lan: Checking for guest additions in VM...
    ipa.otus.lan: The guest additions on this VM do not match the installed version of
    ipa.otus.lan: VirtualBox! In most cases this is fine, but in rare cases it can
    ipa.otus.lan: prevent things such as shared folders from working properly. If you see
    ipa.otus.lan: shared folder errors, please make sure the guest additions within the
    ipa.otus.lan: virtual machine match the version of VirtualBox you have installed on
    ipa.otus.lan: your host and reload your VM.
    ipa.otus.lan:
    ipa.otus.lan: Guest Additions Version: 6.1.30
    ipa.otus.lan: VirtualBox Version: 7.1
==> ipa.otus.lan: Setting hostname...
==> ipa.otus.lan: Configuring and enabling network interfaces...
==> client1.otus.lan: Importing base box 'centos/stream9'...
==> client1.otus.lan: Matching MAC address for NAT networking...
==> client1.otus.lan: Checking if box 'centos/stream9' version '20250310.0' is up to date...
==> client1.otus.lan: Setting the name of the VM: ldap_client1otuslan_1742425303905_10780
==> client1.otus.lan: Fixed port collision for 22 => 2222. Now on port 2200.
==> client1.otus.lan: Clearing any previously set network interfaces...
==> client1.otus.lan: Preparing network interfaces based on configuration...
    client1.otus.lan: Adapter 1: nat
    client1.otus.lan: Adapter 2: hostonly
==> client1.otus.lan: Forwarding ports...
    client1.otus.lan: 22 (guest) => 2200 (host) (adapter 1)
==> client1.otus.lan: Running 'pre-boot' VM customizations...
==> client1.otus.lan: Booting VM...
==> client1.otus.lan: Waiting for machine to boot. This may take a few minutes...
    client1.otus.lan: SSH address: 127.0.0.1:2200
    client1.otus.lan: SSH username: vagrant
    client1.otus.lan: SSH auth method: private key
    client1.otus.lan:
    client1.otus.lan: Vagrant insecure key detected. Vagrant will automatically replace
    client1.otus.lan: this with a newly generated keypair for better security.
    client1.otus.lan:
    client1.otus.lan: Inserting generated public key within guest...
    client1.otus.lan: Removing insecure key from the guest if it's present...
    client1.otus.lan: Key inserted! Disconnecting and reconnecting using new SSH key...
==> client1.otus.lan: Machine booted and ready!
==> client1.otus.lan: Checking for guest additions in VM...
    client1.otus.lan: No guest additions were detected on the base box for this VM! Guest
    client1.otus.lan: additions are required for forwarded ports, shared folders, host only
    client1.otus.lan: networking, and more. If SSH fails on this machine, please install
    client1.otus.lan: the guest additions and repackage the box to continue.
    client1.otus.lan:
    client1.otus.lan: This is not an error message; everything may continue to work properly,
    client1.otus.lan: in which case you may ignore this message.
==> client1.otus.lan: Setting hostname...
==> client1.otus.lan: Configuring and enabling network interfaces...
==> client1.otus.lan: Rsyncing folder: /opt/otus/ldap/ => /vagrant
==> client2.otus.lan: Importing base box 'centos/stream9'...
==> client2.otus.lan: Matching MAC address for NAT networking...
==> client2.otus.lan: Checking if box 'centos/stream9' version '20250310.0' is up to date...
==> client2.otus.lan: Setting the name of the VM: ldap_client2otuslan_1742425361785_78305
==> client2.otus.lan: Fixed port collision for 22 => 2222. Now on port 2201.
==> client2.otus.lan: Clearing any previously set network interfaces...
==> client2.otus.lan: Preparing network interfaces based on configuration...
    client2.otus.lan: Adapter 1: nat
    client2.otus.lan: Adapter 2: hostonly
==> client2.otus.lan: Forwarding ports...
    client2.otus.lan: 22 (guest) => 2201 (host) (adapter 1)
==> client2.otus.lan: Running 'pre-boot' VM customizations...
==> client2.otus.lan: Booting VM...
==> client2.otus.lan: Waiting for machine to boot. This may take a few minutes...
    client2.otus.lan: SSH address: 127.0.0.1:2201
    client2.otus.lan: SSH username: vagrant
    client2.otus.lan: SSH auth method: private key
    client2.otus.lan:
    client2.otus.lan: Vagrant insecure key detected. Vagrant will automatically replace
    client2.otus.lan: this with a newly generated keypair for better security.
    client2.otus.lan:
    client2.otus.lan: Inserting generated public key within guest...
    client2.otus.lan: Removing insecure key from the guest if it's present...
    client2.otus.lan: Key inserted! Disconnecting and reconnecting using new SSH key...
==> client2.otus.lan: Machine booted and ready!
==> client2.otus.lan: Checking for guest additions in VM...
    client2.otus.lan: No guest additions were detected on the base box for this VM! Guest
    client2.otus.lan: additions are required for forwarded ports, shared folders, host only
    client2.otus.lan: networking, and more. If SSH fails on this machine, please install
    client2.otus.lan: the guest additions and repackage the box to continue.
    client2.otus.lan:
    client2.otus.lan: This is not an error message; everything may continue to work properly,
    client2.otus.lan: in which case you may ignore this message.
==> client2.otus.lan: Setting hostname...
==> client2.otus.lan: Configuring and enabling network interfaces...
==> client2.otus.lan: Rsyncing folder: /opt/otus/ldap/ => /vagrant
==> client2.otus.lan: Running provisioner: ansible...
    client2.otus.lan: Running ansible-playbook...
```
</details>
    
    
* **ipa.otus.lan** 
* **client1.otus.lan**
* **client2.otus.lan**    


<details>
<summary> Далее, запустится Ansible-playbook provision.yml, который сделает следующее: </summary>
    
```
PLAY [install base soft, setup time, disable selinux and filerwall] ************

TASK [Gathering Facts] *********************************************************
ok: [client2.otus.lan]
ok: [client1.otus.lan]
ok: [ipa.otus.lan]

TASK [forall : install softs on CentOS] ****************************************
changed: [client2.otus.lan]
changed: [client1.otus.lan]
changed: [ipa.otus.lan]

TASK [forall : disable firewalld] **********************************************
ok: [client2.otus.lan]
ok: [client1.otus.lan]
changed: [ipa.otus.lan]

TASK [forall : disable SElinux now] ********************************************
changed: [client2.otus.lan]
changed: [client1.otus.lan]
changed: [ipa.otus.lan]

TASK [forall : disable SElinux] ************************************************
[WARNING]: SELinux state change will take effect next reboot
changed: [client2.otus.lan]
changed: [client1.otus.lan]
changed: [ipa.otus.lan]

TASK [forall : Set up timezone] ************************************************
changed: [client2.otus.lan]
changed: [client1.otus.lan]
changed: [ipa.otus.lan]

TASK [forall : enable chrony] **************************************************
changed: [client2.otus.lan]
changed: [client1.otus.lan]
changed: [ipa.otus.lan]

TASK [forall : change /etc/hosts] **********************************************
changed: [client1.otus.lan]
changed: [client2.otus.lan]
changed: [ipa.otus.lan]

PLAY [Server setup] ************************************************************

TASK [Gathering Facts] *********************************************************
ok: [ipa.otus.lan]

TASK [server : Disable IPv6 via sysctl] ****************************************
changed: [ipa.otus.lan]

TASK [server : Restart networking] *********************************************
changed: [ipa.otus.lan]

TASK [server : install DL1 module, ipa-server, expect] *************************
changed: [ipa.otus.lan]

TASK [server : Run ipa-server-install in unattended mode] **********************
changed: [ipa.otus.lan]

TASK [server : Authenticate with Kerberos using kinit] *************************
changed: [ipa.otus.lan]

TASK [server : Check Kerberos ticket] ******************************************
changed: [ipa.otus.lan]

TASK [server : debug] **********************************************************
ok: [ipa.otus.lan] => {
    "msg": "Ticket cache: KCM:0\nDefault principal: admin@OTUS.LAN\n\nValid starting       Expires              Service principal\n03/20/2025 02:14:11  03/21/2025 02:14:11  krbtgt/OTUS.LAN@OTUS.LAN"
}

TASK [server : Create user otus-user] ******************************************
changed: [ipa.otus.lan]

PLAY [Clients setup] ***********************************************************

TASK [Gathering Facts] *********************************************************
ok: [client1.otus.lan]
ok: [client2.otus.lan]

TASK [clients : install module ipa-client] *************************************
changed: [client1.otus.lan]
changed: [client2.otus.lan]

TASK [clients : add client1.otus.lan to ipa-server using admin] ****************
skipping: [client2.otus.lan]
changed: [client1.otus.lan]

TASK [clients : add client2.otus.lan to ipa-server using otus-user] ************
skipping: [client1.otus.lan]
changed: [client2.otus.lan]

PLAY RECAP *********************************************************************
client1.otus.lan           : ok=11   changed=8    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
client2.otus.lan           : ok=11   changed=8    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
ipa.otus.lan               : ok=17   changed=14   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
</details>
    
    
    
* Установит все необходимые пакеты на сервера
* Отключит SELinux
* **ipa.otus.lan** - Выключит IPv6 на loopback-интерфейсе (необходимо для установки сервера FreeIPA)
* **ipa.otus.lan** - Установит сервер FreeIPA (пароль пользователя admin: Otus1234)
* **ipa.otus.lan** - Создаст пользователя otus-user на сервере FreeIPA (пароль пользователя otus-user: Otus1234)
* **ipa.otus.lan** - Создаст kerberos ticket for tickets
* **client1.otus.lan и client2.otus.lan** - Установит клиент FreeIPA
* **client1.otus.lan** - Подключит данный сервер к серверу FreeIPA используя пользователя **admin**  
* **client2.otus.lan** - Подключит данный сервер к серверу FreeIPA используя пользователя **otus-user**
---

### 2. Установить FreeIPA**    
    
### 3. Написать Ansible-playbook для конфигурации клиента    
    
* **Выполнено при помощи Vagrant+Ansible**

---

Итак, стенд развернут. Сервер FreeIPA настроен, клиенты подключены.    
     
      
**Попробуем зайти на web-интерфейс FreeIPA:**
     
![image](https://github.com/user-attachments/assets/b9cf17a2-c7a7-4ed8-a9f2-35613ea8ddac)

Видим пользователей: admin и otus-user     

---

Зайдем в виртуальную машину ipa.otus.lan и посмострим kerberos ticket for tickets:    
`vagrant ssh ipa.otus.lan` `sudo -i` `klist`

<details>
<summary> результат выполнения команд </summary>

```
root@deb4likh:/opt/otus/ldap# vagrant ssh client1.otus.lan
Last login: Thu Mar 20 02:14:59 2025 from 10.0.2.2
[vagrant@client1 ~]$ sudo -i
[root@client1 ~]# klist
klist: Credentials cache 'KCM:0' not found
[root@client1 ~]# exit
logout
[vagrant@client1 ~]$ exit
logout
root@deb4likh:/opt/otus/ldap# vagrant ssh ipa.otus.lan
Last login: Thu Mar 20 02:14:12 2025 from 10.0.2.2
[vagrant@ipa ~]$ sudo -i
[root@ipa ~]# klist
Ticket cache: KCM:0
Default principal: admin@OTUS.LAN

Valid starting       Expires              Service principal
03/20/2025 02:14:11  03/21/2025 02:14:11  krbtgt/OTUS.LAN@OTUS.LAN
03/20/2025 02:14:13  03/21/2025 02:14:11  HTTP/ipa.otus.lan@OTUS.LAN
```
</details>

**Видим 2 kerberos tickets for tickets**

---

Зайдем в виртуальную машину client1.otus.lan и попробуем получить kerberos ticket for tickets используя пользователя **admin** (пароль: Otus1234):      
`vagrant ssh client1.otus.lan` `sudo -i` `kinit admin` `klist`

<details>
<summary> результат выполнения команд </summary>

```
root@deb4likh:/opt/otus/ldap# vagrant ssh client1.otus.lan
Last login: Thu Mar 20 02:43:04 2025 from 10.0.2.2
[vagrant@client1 ~]$ sudo -i
[root@client1 ~]# kinit admin
Password for admin@OTUS.LAN:
[root@client1 ~]# klist
Ticket cache: KCM:0
Default principal: admin@OTUS.LAN

Valid starting       Expires              Service principal
03/20/2025 02:54:57  03/21/2025 02:43:54  krbtgt/OTUS.LAN@OTUS.LAN
```
</details> 

**Видим что kerberos tickets for tickets успешно создан**

---

Зайдем в виртуальную машину client2.otus.lan и попробуем получить kerberos ticket for tickets используя пользователя **otus-user** (пароль: Otus1234):     
`vagrant ssh client2.otus.lan` `sudo -i` `kinit otus-user` `klist`

<details>
<summary> результат выполнения команд </summary>

```
root@deb4likh:/opt/otus/ldap# vagrant ssh client2.otus.lan
Last login: Thu Mar 20 02:52:50 2025 from 10.0.2.2
[vagrant@client2 ~]$ sudo -i
[root@client2 ~]# kinit otus-user
Password for otus-user@OTUS.LAN:
Password expired.  You must change it now.
Enter new password:
Enter it again:
[root@client2 ~]# klist
Ticket cache: KCM:0
Default principal: otus-user@OTUS.LAN

Valid starting       Expires              Service principal
03/20/2025 02:56:49  03/21/2025 02:56:49  krbtgt/OTUS.LAN@OTUS.LAN
```
</details>

**Видим что kerberos tickets for tickets успешно создан**

