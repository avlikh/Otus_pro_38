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
<summary> Далее, запустится Ansible-playbook provision.yml, который сделает сдедующее: </summary>
    
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
* **ipa.otus.lan** Выключит IPv6 на loopback-интерфейсе (необходимо для установки сервера FreeIPA)
* **ipa.otus.lan** Установит сервер FreeIPA (пароль пользователя admin: Otus1234)
* **ipa.otus.lan** Создаст пользователя otus-user на сервере FreeIPA (пароль пользователя otus-user: Otus1234)
* **ipa.otus.lan** Создаст kerberos ticket for tickets
* **client1.otus.lan и client2.otus.lan** Установит клиент FreeIPA
* **client1.otus.lan** Подключит данный сервер к серверу FreeIPA используя пользователя **admin**  
* **client2.otus.lan** Подключит данный сервер к серверу FreeIPA используя пользователя **otus-user**
---

## Выполнение задания:
**2. Настроить стенд Vagrant с двумя виртуальными машинами: backup_server и client. (Студент самостоятельно настраивает Vagrant)**    
    
* Выполнено при помощи Vagrant+Ansible

---

**2.1 Настроить удаленный бэкап каталога /etc c сервера client при помощи borgbackup.**
    
В результате подготовки рабочего места (пункт 1) были созданы 2 виртуальные машины, к которым были применены необходимые настройки.    
Был выполнен playbook Ansible https://github.com/avlikh/Otus_pro_27/blob/main/provision.yaml.    

---
     
**Зайдем на **client** и повысим полномочия до root:** `vagrant ssh testClient1` `sudo -i`
<details>
<summary> результат выполнения команд </summary>

```
root@deb4likh:/opt/otus/backup# vagrant ssh client
Linux client 6.1.0-25-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.106-3 (2024-08-26) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue Mar 18 10:28:59 2025 from 192.168.56.1
vagrant@client:~$ sudo -i
```
</details>
    
    
**Убедимся что служба таймера работает:** `systemctl status borg-backup-client.timer` 
<details>
<summary> результат выполнения команды  </summary>

```
root@client:~# systemctl status borg-backup-client.timer
● borg-backup-client.timer - Borg Service client Backup
     Loaded: loaded (/etc/systemd/system/borg-backup-client.timer; enabled; preset: enabled)
     Active: active (waiting) since Tue 2025-03-18 10:28:59 UTC; 6h ago
    Trigger: Tue 2025-03-18 10:44:35 UTC; 44s left
   Triggers: ● borg-backup-client.service

Mar 18 10:28:59 client systemd[1]: Started borg-backup-client.timer - Borg Service client Backup.
```
</details>
    
Видим что таймер работает.   
    
* Далее, оставим наш стенд работать на несколько часов.    

---

**Посмотрим на репозитории borg:** `borg list borg@192.168.56.11:/var/backup`
* Примечание: для доступа к репозиториям borg потребуется пароль: **Otus1234**

<details>
<summary> результат выполнения команды </summary>

```
root@client:~# borg list borg@192.168.56.11:/var/backup
Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Enter passphrase for key ssh://borg@192.168.56.11/var/backup:
client-2025-03-18_10:29:00           Tue, 2025-03-18 10:29:01 [0a95c2fe0c01a5947c15d31631dd32a5902dc6a70ad6531c95a564740b874e72]
client-2025-03-18_16:50:54           Tue, 2025-03-18 16:50:55 [53f2196e51b7af3ab09516be7b988c658f2217b367a0b0f38ba759c5cf6806e2]
```
</details>

---

**Посмотрим логи borg:** `journalctl -t borg`


<details>
<summary> результат выполнения команды </summary>

```
root@client:~# journalctl -t borg
Mar 18 10:29:00 client borg[16107]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 10:29:01 client borg[16107]: ------------------------------------------------------------------------------
Mar 18 10:29:01 client borg[16107]: Repository: ssh://borg@192.168.56.11/var/backup
Mar 18 10:29:01 client borg[16107]: Archive name: client-2025-03-18_10:29:00
Mar 18 10:29:01 client borg[16107]: Archive fingerprint: 0a95c2fe0c01a5947c15d31631dd32a5902dc6a70ad6531c95a564740b874e72
Mar 18 10:29:01 client borg[16107]: Time (start): Tue, 2025-03-18 10:29:01
Mar 18 10:29:01 client borg[16107]: Time (end):   Tue, 2025-03-18 10:29:01
Mar 18 10:29:01 client borg[16107]: Duration: 0.44 seconds
Mar 18 10:29:01 client borg[16107]: Number of files: 452
Mar 18 10:29:01 client borg[16107]: Utilization of max. archive size: 0%
Mar 18 10:29:01 client borg[16107]: ------------------------------------------------------------------------------
Mar 18 10:29:01 client borg[16107]:                        Original size      Compressed size    Deduplicated size
Mar 18 10:29:01 client borg[16107]: This archive:                1.52 MB            666.97 kB            665.25 kB
Mar 18 10:29:01 client borg[16107]: All archives:                1.52 MB            666.40 kB            715.23 kB
Mar 18 10:29:01 client borg[16107]:                        Unique chunks         Total chunks
Mar 18 10:29:01 client borg[16107]: Chunk index:                     433                  443
Mar 18 10:29:01 client borg[16107]: ------------------------------------------------------------------------------
Mar 18 10:29:02 client borg[16109]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 10:29:04 client borg[16111]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 10:34:08 client borg[16120]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 10:34:09 client borg[16120]: ------------------------------------------------------------------------------
Mar 18 10:34:09 client borg[16120]: Repository: ssh://borg@192.168.56.11/var/backup
Mar 18 10:34:09 client borg[16120]: Archive name: client-2025-03-18_10:34:08
Mar 18 10:34:09 client borg[16120]: Archive fingerprint: 46271845421a8440ca4a2bf11abb93c96f8b5aaf0cb793f093e384d6eaeaf476
Mar 18 10:34:09 client borg[16120]: Time (start): Tue, 2025-03-18 10:34:09
Mar 18 10:34:09 client borg[16120]: Time (end):   Tue, 2025-03-18 10:34:09
Mar 18 10:34:09 client borg[16120]: Duration: 0.13 seconds
Mar 18 10:34:09 client borg[16120]: Number of files: 452
Mar 18 10:34:09 client borg[16120]: Utilization of max. archive size: 0%
Mar 18 10:34:09 client borg[16120]: ------------------------------------------------------------------------------
Mar 18 10:34:09 client borg[16120]:                        Original size      Compressed size    Deduplicated size
Mar 18 10:34:09 client borg[16120]: This archive:                1.52 MB            666.97 kB                568 B
Mar 18 10:34:09 client borg[16120]: All archives:                3.04 MB              1.33 MB            715.80 kB
Mar 18 10:34:09 client borg[16120]:                        Unique chunks         Total chunks
Mar 18 10:34:09 client borg[16120]: Chunk index:                     434                  886
Mar 18 10:34:09 client borg[16120]: ------------------------------------------------------------------------------
Mar 18 10:34:10 client borg[16122]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 10:34:12 client borg[16124]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 10:39:28 client borg[16127]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.

...............................

Mar 18 16:13:41 client borg[16742]: Repository: ssh://borg@192.168.56.11/var/backup
Mar 18 16:13:41 client borg[16742]: Archive name: client-2025-03-18_16:13:40
Mar 18 16:13:41 client borg[16742]: Archive fingerprint: f5d6ade224e6f8a85dc7910281ffeac1e42669b35019963172bc516c933f43e5
Mar 18 16:13:41 client borg[16742]: Time (start): Tue, 2025-03-18 16:13:41
Mar 18 16:13:41 client borg[16742]: Time (end):   Tue, 2025-03-18 16:13:41
Mar 18 16:13:41 client borg[16742]: Duration: 0.14 seconds
Mar 18 16:13:41 client borg[16742]: Number of files: 452
Mar 18 16:13:41 client borg[16742]: Utilization of max. archive size: 0%
Mar 18 16:13:41 client borg[16742]: ------------------------------------------------------------------------------
Mar 18 16:13:41 client borg[16742]:                        Original size      Compressed size    Deduplicated size
Mar 18 16:13:41 client borg[16742]: This archive:                1.52 MB            666.97 kB                568 B
Mar 18 16:13:41 client borg[16742]: All archives:                4.57 MB              2.00 MB            716.37 kB
Mar 18 16:13:41 client borg[16742]:                        Unique chunks         Total chunks
Mar 18 16:13:41 client borg[16742]: Chunk index:                     435                 1329
Mar 18 16:13:41 client borg[16742]: ------------------------------------------------------------------------------
Mar 18 16:13:42 client borg[16744]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:13:44 client borg[16746]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:18:54 client borg[16755]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:18:55 client borg[16755]: ------------------------------------------------------------------------------
Mar 18 16:18:55 client borg[16755]: Repository: ssh://borg@192.168.56.11/var/backup
Mar 18 16:18:55 client borg[16755]: Archive name: client-2025-03-18_16:18:54
Mar 18 16:18:55 client borg[16755]: Archive fingerprint: af256e23a06b67174f57ae10db91e1094d52a1850eafd7107b47b5eea9180c23
Mar 18 16:18:55 client borg[16755]: Time (start): Tue, 2025-03-18 16:18:55
Mar 18 16:18:55 client borg[16755]: Time (end):   Tue, 2025-03-18 16:18:55
Mar 18 16:18:55 client borg[16755]: Duration: 0.14 seconds
Mar 18 16:18:55 client borg[16755]: Number of files: 452
Mar 18 16:18:55 client borg[16755]: Utilization of max. archive size: 0%
Mar 18 16:18:55 client borg[16755]: ------------------------------------------------------------------------------
Mar 18 16:18:55 client borg[16755]:                        Original size      Compressed size    Deduplicated size
Mar 18 16:18:55 client borg[16755]: This archive:                1.52 MB            666.97 kB                568 B
Mar 18 16:18:55 client borg[16755]: All archives:                4.57 MB              2.00 MB            716.37 kB
Mar 18 16:18:55 client borg[16755]:                        Unique chunks         Total chunks
Mar 18 16:18:55 client borg[16755]: Chunk index:                     435                 1329
Mar 18 16:18:55 client borg[16755]: ------------------------------------------------------------------------------
Mar 18 16:18:56 client borg[16757]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:18:59 client borg[16759]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:24:15 client borg[16762]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:24:16 client borg[16762]: ------------------------------------------------------------------------------
Mar 18 16:24:16 client borg[16762]: Repository: ssh://borg@192.168.56.11/var/backup
Mar 18 16:24:16 client borg[16762]: Archive name: client-2025-03-18_16:24:15
Mar 18 16:24:16 client borg[16762]: Archive fingerprint: 821a0c567f66b61eae6262d17c3cd16d1417634af5d5fb08e976083b3fe230af
Mar 18 16:24:16 client borg[16762]: Time (start): Tue, 2025-03-18 16:24:16
Mar 18 16:24:16 client borg[16762]: Time (end):   Tue, 2025-03-18 16:24:16
Mar 18 16:24:16 client borg[16762]: Duration: 0.14 seconds
Mar 18 16:24:16 client borg[16762]: Number of files: 452
Mar 18 16:24:16 client borg[16762]: Utilization of max. archive size: 0%
Mar 18 16:24:16 client borg[16762]: ------------------------------------------------------------------------------
Mar 18 16:24:16 client borg[16762]:                        Original size      Compressed size    Deduplicated size
Mar 18 16:24:16 client borg[16762]: This archive:                1.52 MB            666.97 kB                568 B
Mar 18 16:24:16 client borg[16762]: All archives:                4.57 MB              2.00 MB            716.37 kB
Mar 18 16:24:16 client borg[16762]:                        Unique chunks         Total chunks
Mar 18 16:24:16 client borg[16762]: Chunk index:                     435                 1329
Mar 18 16:24:16 client borg[16762]: ------------------------------------------------------------------------------
Mar 18 16:24:17 client borg[16764]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:24:19 client borg[16767]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:29:35 client borg[16803]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:29:37 client borg[16803]: ------------------------------------------------------------------------------
Mar 18 16:29:37 client borg[16803]: Repository: ssh://borg@192.168.56.11/var/backup
Mar 18 16:29:37 client borg[16803]: Archive name: client-2025-03-18_16:29:35
Mar 18 16:29:37 client borg[16803]: Archive fingerprint: 6bc2586fd4f38167331e92f949b69fdfeecbaae7ae6912b81f43e9f49b8d1cc3
Mar 18 16:29:37 client borg[16803]: Time (start): Tue, 2025-03-18 16:29:37
Mar 18 16:29:37 client borg[16803]: Time (end):   Tue, 2025-03-18 16:29:37
Mar 18 16:29:37 client borg[16803]: Duration: 0.14 seconds
Mar 18 16:29:37 client borg[16803]: Number of files: 452
Mar 18 16:29:37 client borg[16803]: Utilization of max. archive size: 0%
Mar 18 16:29:37 client borg[16803]: ------------------------------------------------------------------------------
Mar 18 16:29:37 client borg[16803]:                        Original size      Compressed size    Deduplicated size
Mar 18 16:29:37 client borg[16803]: This archive:                1.52 MB            666.97 kB                568 B
Mar 18 16:29:37 client borg[16803]: All archives:                4.57 MB              2.00 MB            716.37 kB
Mar 18 16:29:37 client borg[16803]:                        Unique chunks         Total chunks
Mar 18 16:29:37 client borg[16803]: Chunk index:                     435                 1329
Mar 18 16:29:37 client borg[16803]: ------------------------------------------------------------------------------
Mar 18 16:29:38 client borg[16805]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:29:40 client borg[16807]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:34:54 client borg[16843]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:34:55 client borg[16843]: ------------------------------------------------------------------------------
Mar 18 16:34:55 client borg[16843]: Repository: ssh://borg@192.168.56.11/var/backup
Mar 18 16:34:55 client borg[16843]: Archive name: client-2025-03-18_16:34:54
Mar 18 16:34:55 client borg[16843]: Archive fingerprint: 4807f48cf68966024c6c1e24901042be161e4f1761795ce2f1b03b3279c08eb4
Mar 18 16:34:55 client borg[16843]: Time (start): Tue, 2025-03-18 16:34:55
Mar 18 16:34:55 client borg[16843]: Time (end):   Tue, 2025-03-18 16:34:55
Mar 18 16:34:55 client borg[16843]: Duration: 0.15 seconds
Mar 18 16:34:55 client borg[16843]: Number of files: 452
Mar 18 16:34:55 client borg[16843]: Utilization of max. archive size: 0%
Mar 18 16:34:55 client borg[16843]: ------------------------------------------------------------------------------
Mar 18 16:34:55 client borg[16843]:                        Original size      Compressed size    Deduplicated size
Mar 18 16:34:55 client borg[16843]: This archive:                1.52 MB            666.97 kB                568 B
Mar 18 16:34:55 client borg[16843]: All archives:                4.57 MB              2.00 MB            716.37 kB
Mar 18 16:34:55 client borg[16843]:                        Unique chunks         Total chunks
Mar 18 16:34:55 client borg[16843]: Chunk index:                     435                 1329
Mar 18 16:34:55 client borg[16843]: ------------------------------------------------------------------------------
Mar 18 16:34:56 client borg[16845]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:34:58 client borg[16847]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:40:14 client borg[16855]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:40:15 client borg[16855]: ------------------------------------------------------------------------------
Mar 18 16:40:15 client borg[16855]: Repository: ssh://borg@192.168.56.11/var/backup
Mar 18 16:40:15 client borg[16855]: Archive name: client-2025-03-18_16:40:14
Mar 18 16:40:15 client borg[16855]: Archive fingerprint: ca1776ea622523c40977387e669cb53ba570788750090aec558081fc13cd8d6f
Mar 18 16:40:15 client borg[16855]: Time (start): Tue, 2025-03-18 16:40:15
Mar 18 16:40:15 client borg[16855]: Time (end):   Tue, 2025-03-18 16:40:15
Mar 18 16:40:15 client borg[16855]: Duration: 0.14 seconds
Mar 18 16:40:15 client borg[16855]: Number of files: 452
Mar 18 16:40:15 client borg[16855]: Utilization of max. archive size: 0%
Mar 18 16:40:15 client borg[16855]: ------------------------------------------------------------------------------
Mar 18 16:40:15 client borg[16855]:                        Original size      Compressed size    Deduplicated size
Mar 18 16:40:15 client borg[16855]: This archive:                1.52 MB            666.97 kB                568 B
Mar 18 16:40:15 client borg[16855]: All archives:                4.57 MB              2.00 MB            716.37 kB
Mar 18 16:40:15 client borg[16855]:                        Unique chunks         Total chunks
Mar 18 16:40:15 client borg[16855]: Chunk index:                     435                 1329
Mar 18 16:40:15 client borg[16855]: ------------------------------------------------------------------------------
Mar 18 16:40:16 client borg[16857]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:40:18 client borg[16860]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:45:33 client borg[16868]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:45:35 client borg[16868]: ------------------------------------------------------------------------------
Mar 18 16:45:35 client borg[16868]: Repository: ssh://borg@192.168.56.11/var/backup
Mar 18 16:45:35 client borg[16868]: Archive name: client-2025-03-18_16:45:33
Mar 18 16:45:35 client borg[16868]: Archive fingerprint: 36d6150f000e2c5320ffb85d7a95276159b51a4fb0baa3ed94910a79f21618fc
Mar 18 16:45:35 client borg[16868]: Time (start): Tue, 2025-03-18 16:45:35
Mar 18 16:45:35 client borg[16868]: Time (end):   Tue, 2025-03-18 16:45:35
Mar 18 16:45:35 client borg[16868]: Duration: 0.14 seconds
Mar 18 16:45:35 client borg[16868]: Number of files: 452
Mar 18 16:45:35 client borg[16868]: Utilization of max. archive size: 0%
Mar 18 16:45:35 client borg[16868]: ------------------------------------------------------------------------------
Mar 18 16:45:35 client borg[16868]:                        Original size      Compressed size    Deduplicated size
Mar 18 16:45:35 client borg[16868]: This archive:                1.52 MB            666.97 kB                568 B
Mar 18 16:45:35 client borg[16868]: All archives:                4.57 MB              2.00 MB            716.37 kB
Mar 18 16:45:35 client borg[16868]:                        Unique chunks         Total chunks
Mar 18 16:45:35 client borg[16868]: Chunk index:                     435                 1329
Mar 18 16:45:35 client borg[16868]: ------------------------------------------------------------------------------
Mar 18 16:45:36 client borg[16872]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:45:38 client borg[16872]: Failed to create/acquire the lock /var/backup/lock (timeout).
Mar 18 16:50:54 client borg[16889]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:50:55 client borg[16889]: ------------------------------------------------------------------------------
Mar 18 16:50:55 client borg[16889]: Repository: ssh://borg@192.168.56.11/var/backup
Mar 18 16:50:55 client borg[16889]: Archive name: client-2025-03-18_16:50:54
Mar 18 16:50:55 client borg[16889]: Archive fingerprint: 53f2196e51b7af3ab09516be7b988c658f2217b367a0b0f38ba759c5cf6806e2
Mar 18 16:50:55 client borg[16889]: Time (start): Tue, 2025-03-18 16:50:55
Mar 18 16:50:55 client borg[16889]: Time (end):   Tue, 2025-03-18 16:50:55
Mar 18 16:50:55 client borg[16889]: Duration: 0.15 seconds
Mar 18 16:50:55 client borg[16889]: Number of files: 453
Mar 18 16:50:55 client borg[16889]: Utilization of max. archive size: 0%
Mar 18 16:50:55 client borg[16889]: ------------------------------------------------------------------------------
Mar 18 16:50:55 client borg[16889]:                        Original size      Compressed size    Deduplicated size
Mar 18 16:50:55 client borg[16889]: This archive:                1.52 MB            666.97 kB                568 B
Mar 18 16:50:55 client borg[16889]: All archives:                6.09 MB              2.67 MB            766.98 kB
Mar 18 16:50:55 client borg[16889]:                        Unique chunks         Total chunks
Mar 18 16:50:55 client borg[16889]: Chunk index:                     437                 1772
Mar 18 16:50:55 client borg[16889]: ------------------------------------------------------------------------------
Mar 18 16:50:56 client borg[16891]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Mar 18 16:50:59 client borg[16893]: Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.

```
</details>
    
Видим, что каждые 5 минут создавались архивы.    

---
     
**Посмотрим содержимое архива:** `borg list borg@192.168.56.11:/var/backup::client-2025-03-18_17:01:34`
* Пароль все тот же ;-) : **Otus1234**

<details>
<summary> результат выполнения команды </summary>

```
root@client:~# borg list borg@192.168.56.11:/var/backup::client-2025-03-18_17:01:34
Remote: Warning: Permanently added '192.168.56.11' (ED25519) to the list of known hosts.
Enter passphrase for key ssh://borg@192.168.56.11/var/backup:
drwxr-xr-x root   root          0 Tue, 2025-03-18 16:49:20 etc
-rw-r--r-- root   root       3040 Thu, 2023-05-25 15:54:35 etc/adduser.conf
-rw-r--r-- root   root       1706 Thu, 2023-05-25 15:54:35 etc/deluser.conf
drwxr-xr-x root   root          0 Thu, 2024-09-05 04:34:04 etc/apt
drwxr-xr-x root   root          0 Thu, 2024-09-05 04:34:19 etc/apt/apt.conf.d
-rw-r--r-- root   root        399 Thu, 2023-05-25 14:11:37 etc/apt/apt.conf.d/01autoremove
-rw-r--r-- root   root        182 Sun, 2023-01-08 21:50:51 etc/apt/apt.conf.d/70debconf
-rw-r--r-- root   root        307 Sun, 2021-03-28 11:06:44 etc/apt/apt.conf.d/20listchanges
-rw-r--r-- root   root         80 Sat, 2022-12-31 20:59:00 etc/apt/apt.conf.d/20auto-upgrades
-rw-r--r-- root   root       7338 Sat, 2022-12-31 20:59:00 etc/apt/apt.conf.d/50unattended-upgrades
drwxr-xr-x root   root          0 Thu, 2023-05-25 14:11:37 etc/apt/auth.conf.d
drwxr-xr-x root   root          0 Thu, 2023-05-25 14:11:37 etc/apt/keyrings
drwxr-xr-x root   root          0 Thu, 2023-05-25 14:11:37 etc/apt/preferences.d
drwxr-xr-x root   root          0 Thu, 2023-05-25 14:11:37 etc/apt/sources.list.d
drwxr-xr-x root   root          0 Thu, 2024-09-05 04:33:36 etc/apt/trusted.gpg.d
-rw-r--r-- root   root      11861 Sun, 2023-07-30 19:30:54 etc/apt/trusted.gpg.d/debian-archive-bookworm-automatic.asc
-rw-r--r-- root   root      11873 Sun, 2023-07-30 19:30:54 etc/apt/trusted.gpg.d/debian-archive-bookworm-security-automatic.asc
-rw-r--r-- root   root        461 Sun, 2023-07-30 19:30:54 etc/apt/trusted.gpg.d/debian-archive-bookworm-stable.asc
-rw-r--r-- root   root      11861 Sun, 2023-07-30 19:30:54 etc/apt/trusted.gpg.d/debian-archive-bullseye-automatic.asc
-rw-r--r-- root   root      11873 Sun, 2023-07-30 19:30:54 etc/apt/trusted.gpg.d/debian-archive-bullseye-security-automatic.asc
-rw-r--r-- root   root       3403 Sun, 2023-07-30 19:30:54 etc/apt/trusted.gpg.d/debian-archive-bullseye-stable.asc
-rw-r--r-- root   root      11093 Sun, 2023-07-30 19:30:54 etc/apt/trusted.gpg.d/debian-archive-buster-automatic.asc
-rw-r--r-- root   root      11105 Sun, 2023-07-30 19:30:54 etc/apt/trusted.gpg.d/debian-archive-buster-security-automatic.asc
-rw-r--r-- root   root       1704 Sun, 2023-07-30 19:30:54 etc/apt/trusted.gpg.d/debian-archive-buster-stable.asc
-rw-r--r-- root   root        482 Thu, 2024-09-05 04:34:56 etc/apt/sources.list
drwxr-xr-x root   root          0 Sun, 2021-03-28 11:06:44 etc/apt/listchanges.conf.d
-rw-r--r-- root   root        150 Thu, 2024-09-05 04:34:04 etc/apt/listchanges.conf
drwxr-xr-x root   root          0 Thu, 2024-09-05 04:34:00 etc/cron.daily
-rwxr-xr-x root   root       1478 Thu, 2023-05-25 14:11:37 etc/cron.daily/apt-compat
-rwxr-xr-x root   root        123 Mon, 2023-03-27 00:41:09 etc/cron.daily/dpkg
-rw-r--r-- root   root        102 Thu, 2023-03-02 07:33:55 etc/cron.daily/.placeholder
-rwxr-xr-x root   root        377 Wed, 2022-12-14 18:16:50 etc/cron.daily/logrotate
-rwxr-xr-x root   root       1395 Sun, 2023-03-12 22:23:59 etc/cron.daily/man-db
drwxr-xr-x root   root          0 Thu, 2024-09-05 04:34:12 etc/kernel
drwxr-xr-x root   root          0 Thu, 2024-09-05 04:34:37 etc/kernel/postinst.d
-rwxr-xr-x root   root        863 Mon, 2020-08-31 23:59:17 etc/kernel/postinst.d/initramfs-tools
-rwxr-xr-x root   root        646 Mon, 2023-10-02 14:11:34 etc/kernel/postinst.d/zz-update-grub
-rwxr-xr-x root   root        364 Sat, 2022-12-31 20:59:00 etc/kernel/postinst.d/unattended-upgrades
drwxr-xr-x root   root          0 Sun, 2024-08-25 17:35:39 etc/kernel/install.d
drwxr-xr-x root   root          0 Thu, 2024-09-05 04:34:37 etc/kernel/postrm.d
-rwxr-xr-x root   root        816 Mon, 2020-08-31 23:59:17 etc/kernel/postrm.d/initramfs-tools
-rwxr-xr-x root   root        646 Mon, 2023-10-02 14:11:34 etc/kernel/postrm.d/zz-update-grub
drwxr-xr-x root   root          0 Thu, 2024-09-05 04:34:18 etc/logrotate.d
-rw-r--r-- root   root        173 Thu, 2023-05-25 14:11:37 etc/logrotate.d/apt
-rw-r--r-- root   root        120 Tue, 2023-02-14 22:56:51 etc/logrotate.d/alternatives
-rw-r--r-- root   root        112 Tue, 2023-02-14 22:56:51 etc/logrotate.d/dpkg
-rw-r--r-- root   root        130 Mon, 2019-10-14 12:10:31 etc/logrotate.d/btmp
-rw-r--r-- root   root        145 Mon, 2019-10-14 12:10:31 etc/logrotate.d/wtmp
-rw-r--r-- root   root        160 Mon, 2023-05-08 20:05:00 etc/logrotate.d/chrony
-rw-r--r-- root   root        248 Wed, 2023-02-22 19:43:00 etc/logrotate.d/rsyslog
-rw-r--r-- root   root        235 Sat, 2022-12-31 20:59:00 etc/logrotate.d/unattended-upgrades
-rw-r--r-- root   root        681 Tue, 2023-01-17 22:34:38 etc/xattr.conf
-rw-r--r-- root   root        191 Thu, 2023-02-09 09:36:04 etc/libaudit.conf
-rw-r--r-- root   root          5 Wed, 2024-08-14 16:10:00 etc/debian_version
drwxr-xr-x root   root          0 Tue, 2025-03-18 10:27:46 etc/default
-rw-r--r-- root   root       1756 Tue, 2024-07-30 22:16:44 etc/default/nss
-rw-r--r-- root   root       1117 Fri, 2022-11-11 08:28:15 etc/default/useradd
-rw-r--r-- root   root         81 Thu, 2024-03-28 09:52:12 etc/default/hwclock
-rw-r--r-- root   root        955 Sun, 2022-07-17 08:49:40 etc/default/cron
-rw-r--r-- root   root        297 Sat, 2023-09-16 10:03:58 etc/default/dbus
-rw-r--r-- root   root       1032 Tue, 2021-09-14 13:27:00 etc/default/networking

............

drwxr-xr-x root   root          0 Tue, 2025-03-18 10:28:00 etc/apache2/conf-available
-rw-r--r-- root   root        127 Fri, 2020-12-18 19:33:11 etc/apache2/conf-available/javascript-common.conf
drwxr-xr-x root   root          0 Tue, 2025-03-18 10:28:00 etc/lighttpd
drwxr-xr-x root   root          0 Tue, 2025-03-18 10:28:00 etc/lighttpd/conf-available
-rw-r--r-- root   root         56 Sun, 2013-07-28 21:48:58 etc/lighttpd/conf-available/90-javascript-alias.conf
drwxr-xr-x root   root          0 Tue, 2025-03-18 10:28:00 etc/lighttpd/conf-enabled
lrwxrwxrwx root   root         42 Tue, 2025-03-18 10:28:00 etc/lighttpd/conf-enabled/90-javascript-alias.conf -> ../conf-available/90-javascript-alias.conf
-rw-r--r-- root   root      20015 Tue, 2025-03-18 10:28:06 etc/ld.so.cache
drwxr-xr-x root   root          0 Tue, 2025-03-18 16:49:44 etc/test
-rw-r--r-- root   root          0 Tue, 2025-03-18 16:49:44 etc/test/1
```
</details>
     
    
**Итак, видим что borgbackup работает и резервные копии папки /etc создаются каждые 5 минут (в состветствии с настроками таймера).**

