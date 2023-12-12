### Дёров Константин Павлович 
### Группа № 4719
### Семинар 2
---
### Задание 1:
1) Запустить контейнер с ubuntu, используя механизм LXC
2) Ограничить контейнер 256 Мб ОЗУ и проверить, что ограничение работает
3) Добавить автозапуск контейнеру, перезагрузить ОС и убедиться, что контейнер действительно запустился самостоятельно
--- 
### Решение .
### Установка LXC.
* konstantin@konstantin-LinuxVirtualBox:~$ sudo su  
[sudo] пароль для konstantin: 
* root@konstantin-LinuxVirtualBox:/home/konstantin# cd /
* root@konstantin-LinuxVirtualBox:/# apt update  
Сущ:1 http://ru.archive.ubuntu.com/ubuntu jammy InRelease  
Игн:2 https://packages.medibuntu.org jammy InRelease   
Пол:3 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]  
-//-  
W: https://download.docker.com/linux/ubuntu/dists/jammy/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.
* root@konstantin-LinuxVirtualBox:/# apt install cgroup-tools  
Чтение списков пакетов… Готово  
Построение дерева зависимостей… Готово  
Чтение информации о состоянии… Готово   
Будут установлены следующие дополнительные пакеты:  
-//-   
Обрабатываются триггеры для man-db (2.10.2-1) …  
* root@konstantin-LinuxVirtualBox:/# cd /sys/fs/cgroup/
* root@konstantin-LinuxVirtualBox:/sys/fs/cgroup# ls -al  
итого 0  
dr-xr-xr-x 12 root root 0 дек 12 10:40 .  
drwxr-xr-x  8 root root 0 дек 12 10:40 ..  
-r--r--r--  1 root root 0 дек 12 10:40 cgroup.controllers  
-rw-r--r--  1 root root 0 дек 12 10:40 cgroup.max.depth  
-rw-r--r--  1 root root 0 дек 12 10:40 cgroup.max.descendants  
-rw-r--r--  1 root root 0 дек 12 10:40 cgroup.pressure  
-rw-r--r--  1 root root 0 дек 12 10:40 cgroup.procs  
-r--r--r--  1 root root 0 дек 12 10:40 cgroup.stat  
-rw-r--r--  1 root root 0 дек 12 10:45 cgroup.subtree_control  
-rw-r--r--  1 root root 0 дек 12 10:40 cgroup.threads  
-rw-r--r--  1 root root 0 дек 12 10:40 cpu.pressure  
-r--r--r--  1 root root 0 дек 12 10:40 cpuset.cpus.effective  
-r--r--r--  1 root root 0 дек 12 10:40 cpuset.mems.effective  
-r--r--r--  1 root root 0 дек 12 10:40 cpu.stat  
drwxr-xr-x  2 root root 0 дек 12 10:40 dev-hugepages.mount  
drwxr-xr-x  2 root root 0 дек 12 10:40 dev-mqueue.mount  
drwxr-xr-x  2 root root 0 дек 12 10:40 init.scope  
-rw-r--r--  1 root root 0 дек 12 10:40 io.cost.model  
-rw-r--r--  1 root root 0 дек 12 10:40 io.cost.qos  
-rw-r--r--  1 root root 0 дек 12 10:40 io.pressure  
-rw-r--r--  1 root root 0 дек 12 10:40 io.prio.class  
-r--r--r--  1 root root 0 дек 12 10:40 io.stat  
-r--r--r--  1 root root 0 дек 12 10:40 memory.numa_stat  
-rw-r--r--  1 root root 0 дек 12 10:40 memory.pressure  
--w-------  1 root root 0 дек 12 10:40 memory.reclaim  
-r--r--r--  1 root root 0 дек 12 10:40 memory.stat  
-r--r--r--  1 root root 0 дек 12 10:40 misc.capacity  
drwxr-xr-x  2 root root 0 дек 12 10:40 proc-sys-fs-binfmt_misc.mount  
drwxr-xr-x  2 root root 0 дек 12 10:40 sys-fs-fuse-connections.mount  
drwxr-xr-x  2 root root 0 дек 12 10:40 sys-kernel-config.mount  
drwxr-xr-x  2 root root 0 дек 12 10:40 sys-kernel-debug.mount  
drwxr-xr-x  2 root root 0 дек 12 10:40 sys-kernel-tracing.mount  
drwxr-xr-x 71 root root 0 дек 12 11:09 system.slice  
drwxr-xr-x  3 root root 0 дек 12 10:52 user.slice    
* root@konstantin-LinuxVirtualBox:/sys/fs/cgroup# apt-get install lxc debootstrap bridge-utils lxc-templates -y   
Чтение списков пакетов… Готово  
Построение дерева зависимостей… Готово  
Чтение информации о состоянии… Готово  
-//-  
брабатываются триггеры для libc-bin (2.35-0ubuntu3.4) …
* root@konstantin-LinuxVirtualBox:/sys/fs/cgroup# apt update  
Сущ:1 http://ru.archive.ubuntu.com/ubuntu jammy InRelease  
Игн:2 https://packages.medibuntu.org jammy InRelease  
Пол:3 http://ru.archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]  
-//-  
N: Информацию о создании репозитория и настройках пользователя смотрите в справочной странице apt-secure(8).
* root@konstantin-LinuxVirtualBox:/sys/fs/cgroup# cd /
### Запустить контейнер с ubuntu, используя механизм LXC.
*  root@konstantin-LinuxVirtualBox:/# lxc-ls -f
* root@konstantin-LinuxVirtualBox:/# find /usr -name lxc-veth.conf  
/usr/share/doc/liblxc-common/examples/lxc-veth.conf
* lxc-create -n test1 -t ubuntu -f /usr/share/doc/liblxc-common/examples/lxc-veth.conf
* root@konstantin-LinuxVirtualBox:/# lxc-create -n test1 -t ubuntu -f /usr/share/doc/liblxc-common/examples/lxc-veth.conf  
Checking cache download in /var/cache/lxc/jammy/rootfs-amd64 ...  
Installing packages in template: apt-transport-https,ssh,vim,language-pack-en,language-pack-ru  
Downloading ubuntu jammy minimal ...  
 -//-  
 * root@konstantin-LinuxVirtualBox:/# lxc-ls -f  
NAME  STATE   AUTOSTART GROUPS IPV4 IPV6 UNPRIVILEGED  
test1 STOPPED 0         -      -    -    false   
* root@konstantin-LinuxVirtualBox:/# lxc-start -n test1  
lxc-start: test1: lxccontainer.c: wait_on_daemonized_start: 877 Received container state "ABORTING" instead of "RUNNING"  
lxc-start: test1: tools/lxc_start.c: main: 306 The container failed to start  
lxc-start: test1: tools/lxc_start.c: main: 309 To get more details, run the container in foreground mode  
lxc-start: test1: tools/lxc_start.c: main: 311 Additional information can be obtained by setting the --logfile and --logpriority options  
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# nano config
---  
Network configuraton  
lxc.net.0.type = veth  
lxc.net.0.flags = up  
lxc.net.0.link = br0 - здесь нужно дописать перед br0 - lxc  
сохраняем выходим
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# lxc-start -n test1
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# lxc-ls -f     
NAME  STATE   AUTOSTART GROUPS IPV4                 IPV6                            UNPRIVILEGED          
test1 RUNNING 0         -      10.0.3.253, 10.2.3.5   2003:db8:1:0:214:1234:fe0b:3597 false    
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# lxc-attach -n test1  
root@test1:/# ip a  
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
    inet 127.0.0.1/8 scope host lo  
       valid_lft forever preferred_lft forever  
    inet6 ::1/128 scope host   
       valid_lft forever preferred_lft forever  
2: eth0@if6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000  
    link/ether 4a:49:43:49:79:bf brd ff:ff:ff:ff:ff:ff link-netnsid 0  
    inet 10.2.3.5/24 brd 10.2.3.255 scope global eth0  
       valid_lft forever preferred_lft forever  
    inet 10.0.3.253/24 metric 100 brd 10.0.3.255 scope global dynamic eth0  
       valid_lft 3308sec preferred_lft 3308sec  
    inet6 2003:db8:1:0:214:1234:fe0b:3597/64 scope global  
       valid_lft forever preferred_lft forever  
    inet6 fe80::4849:43ff:fe49:79bf/64 scope link   
       valid_lft forever preferred_lft forever 
### Ограничить контейнер 256 Мб ОЗУ и проверить, что ограничение работает.
* root@test1:/# free -m  
               total        used        free      shared  buff/cache   available  
Память:       3907          27        3876           0           4        3880  
Подкачка:          0           0           0  
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# exit  
exit
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# lxc-stop -n test1  
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# lxc-ls -f  
NAME  STATE   AUTOSTART GROUPS IPV4 IPV6 UNPRIVILEGED  
test1 STOPPED 0         -      -    -    false   
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# nano config  
---
Сontainer specific configuration  
lxc.uts.name = beta  
lxc.rootfs.path = dir:/var/lib/lxc/test1/rootfs  
lxc.uts.name = test1  
lxc.arch = amd64  
lxc.cgroup2.memory.max = 512M  - на своодной строчьке прописываем ограничение по памяти 512М  
сохраняем выходим
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# lxc-start -n test1
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# lxc-ls -f  
NAME  STATE   AUTOSTART GROUPS IPV4                 IPV6                            UNPRIVILEGED   
test1 RUNNING 0         -      10.0.3.253, 10.2.3.5 2003:db8:1:0:214:1234:fe0b:3597 false
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# lxc-attach -n test1 
* root@test1:/# free -m  
               total        used        free      shared  buff/cache   available  
Память:        512          26         483           0           1         485  
Подкачка:          0           0           0    
* root@test1:/# exit  
exit
### Добавить автозапуск контейнеру, перезагрузить ОС и убедиться, что контейнер действительно запустился самостоятельно.
* root@konstantin-LinuxVirtualBox:/var/lib/lxc# ll  
итого 12  
drwx------  3 root root 4096 дек 12 11:33 ./  
drwxr-xr-x 82 root root 4096 дек 12 11:17 ../  
drwxrwx---  3 root root 4096 дек 12 16:40 test1/  
* root@konstantin-LinuxVirtualBox:/var/lib/lxc# cd test1
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# nano config
---
Container specific configuration  
lxc.rootfs.path = dir:/var/lib/lxc/test2/rootfs  
lxc.uts.name = test2  
lxc.arch = amd64  
lxc.start.auto = 1 - добавляем запись, чтобы контейнер стартовал автоматически после перезагрузки (0 - выключен, 1 - включен)  
схраняем выходим  
* root@konstantin-LinuxVirtualBox:/var/lib/lxc/test1# reboot
* konstantin@konstantin-LinuxVirtualBox:~$ sudo su 
[sudo] пароль для konstantin:  
* root@konstantin-LinuxVirtualBox:/home/konstantin# lxc-ls -f  
NAME  STATE   AUTOSTART GROUPS IPV4                 IPV6                            UNPRIVILEGED  
test1 RUNNING 1         -      10.0.3.253, 10.2.3.5 2003:db8:1:0:214:1234:fe0b:3597 false   
* root@konstantin-LinuxVirtualBox:/home/konstantin# lxc-attach -n test1  
* root@test1:/# free -m  
               total        used        free      shared  buff/cache   available  
Память:        512          28         432           0          51         483  
Подкачка:          0           0           0  
* root@test1:/# exit  
exit
* root@konstantin-LinuxVirtualBox:/home/konstantin# exit  
exit
* konstantin@konstantin-LinuxVirtualBox:~$  


  











