### Дёров Константин Павлович 
### Группа № 4719
### Семинар 1
---
### **Задание: необходимо продемонстрировать изоляцию одного и того же приложения (как решено на семинаре - командного интерпретатора) в различных пространствах имен.**
--- 
### Решение.
### Смена корневого каталога .
* konstantin@konstantin-LinuxVirtualBox:~$ cd project.local
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ mkdir testdirectory
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ mkdir testdirectory/bin
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ ldd /bin/bash  
	linux-vdso.so.1 (0x00007ffde11d5000)  
	libtinfo.so.6 => /lib/x86_64-linux-gnu/libtinfo.so.6 (0x00007f387ba3d000)  
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f387b800000)  
	/lib64/ld-linux-x86-64.so.2 (0x00007f387bbe0000)  
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ mkdir testdirectory/lib
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ mkdir testdirectory/lib64
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ cp /lib/x86_64-linux-gnu/libtinfo.so.6 testdirectory/lib
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ cp /lib/x86_64-linux-gnu/libc.so.6 testdirectory/lib
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ cp /lib64/ld-linux-x86-64.so.2 testdirectory/lib64
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ cp /bin/bash testdirectory/bin
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ sudo chroot testdirectory/  
[sudo] пароль для konstantin:   
bash-5.1# ls  
bash: ls: command not found  
bash-5.1# exit  
exit 
* **Добавляем команду ls в наш каталог.**
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ whereis ls  
ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ cp /usr/bin/ls testdirectory/bin
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ ldd /usr/bin/ls  
	linux-vdso.so.1 (0x00007ffe6f394000)  
	libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1 (0x00007fd448909000)  
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fd448600000)  
	libpcre2-8.so.0 => /lib/x86_64-linux-gnu/libpcre2-8.so.0 (0x00007fd448872000)  
	/lib64/ld-linux-x86-64.so.2 (0x00007fd448969000)  
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ cp /lib/x86_64-linux-gnu/libselinux.so.1 testdirectory/lib
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ cp /lib/x86_64-linux-gnu/libc.so.6 testdirectory/lib
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ cp /lib/x86_64-linux-gnu/libpcre2-8.so.0 testdirectory/lib
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ cp /lib64/ld-linux-x86-64.so.2 testdirectory/lib64
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ sudo chroot   testdirectory/  
bash-5.1# ls  
bin  lib  lib64  
bash-5.1# exit  
exit  
### Изоляция файловой системы.
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ ls -l /proc/$$/ns  
итого 0  
lrwxrwxrwx 1 konstantin konstantin 0 дек  6 13:19 cgroup -> 'cgroup:[4026531835]'  
lrwxrwxrwx 1 konstantin konstantin 0 дек  6 13:19 ipc -> 'ipc:[4026531839]'  
lrwxrwxrwx 1 konstantin konstantin 0 дек  6 13:19 mnt -> 'mnt:[4026531841]'  
lrwxrwxrwx 1 konstantin konstantin 0 дек  6 13:19 net -> 'net:[4026531840]'  
lrwxrwxrwx 1 konstantin konstantin 0 дек  6 13:19 pid -> 'pid:[4026531836]'  
lrwxrwxrwx 1 konstantin konstantin 0 дек  6 13:19 pid_for_children -> 'pid:[4026531836]'  
lrwxrwxrwx 1 konstantin konstantin 0 дек  6 13:19 time -> 'time:[4026531834]'  
lrwxrwxrwx 1 konstantin konstantin 0 дек  6 13:19 time_for_children -> 'time:[4026531834]'  
lrwxrwxrwx 1 konstantin konstantin 0 дек  6 13:19 user -> 'user:[4026531837]'  
lrwxrwxrwx 1 konstantin konstantin 0 дек  6 13:19 uts -> 'uts:[4026531838]' 
* **Изоляция сетевого стека хоста**
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ ip a  
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
    inet 127.0.0.1/8 scope host lo  
       valid_lft forever preferred_lft forever  
    inet6 ::1/128 scope host   
       valid_lft forever preferred_lft forever  
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000  
    link/ether 08:00:27:3e:d2:a9 brd ff:ff:ff:ff:ff:ff  
    inet 192.168.1.52/24 brd 192.168.1.255 scope global dynamic noprefixroute   enp0s3  
       valid_lft 19584sec preferred_lft 19584sec  
    inet6 fd70:b366:7a6:0:f68e:6c3d:46ad:ce92/64 scope global temporary dynamic   
       valid_lft 599185sec preferred_lft 80464sec  
    inet6 fd70:b366:7a6:0:f220:60fe:7b10:986e/64 scope global dynamic mngtmpaddr noprefixroute   
       valid_lft 4294966491sec preferred_lft 4294966491sec  
    inet6 fe80::2e3a:8f30:b5c0:7d9b/64 scope link noprefixroute   
       valid_lft forever preferred_lft forever  
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default   
    link/ether 02:42:6d:e3:04:9d brd ff:ff:ff:ff:ff:ff  
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0  
       valid_lft forever preferred_lft forever  
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ sudo unshare --net  
[sudo] пароль для konstantin: 
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ip a  
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# exit  
выход  
* **Изоляция сетевого стека хоста и изоляция дерева системных процессов**
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ sudo unshare --net --pid --fork --mount-proc
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ps aux     
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND  
root           1  0.0  0.1  11268  5376 pts/1    S    14:44   0:00 -bash  
root          14  0.0  0.0  12708  3456 pts/1    R+   14:45   0:00 ps aux 
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# exit  
выход  
### Добавляем сетевое взаимодействие (в ручьную).
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ ip netns list
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ sudo ip netns add net1
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ sudo ip netns exec net1 /bin/bash
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ip a  
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ip link set dev lo up
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ip a  
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
    inet 127.0.0.1/8 scope host lo  
       valid_lft forever preferred_lft forever  
    inet6 ::1/128 scope host   
       valid_lft forever preferred_lft forever 
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ip link add veth0 type veth peer name veth1  
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ip a  
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
    inet 127.0.0.1/8 scope host lo  
       valid_lft forever preferred_lft forever  
    inet6 ::1/128 scope host   
       valid_lft forever preferred_lft forever  
2: veth1@veth0: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN group default qlen 1000  
    link/ether 9a:9d:b3:04:a1:e6 brd ff:ff:ff:ff:ff:ff  
3: veth0@veth1: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN group default qlen 1000  
    link/ether ea:64:85:54:c6:10 brd ff:ff:ff:ff:ff:ff
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ip link set veth1 netns net1
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ip addr add 10.0.0.2/16 dev veth0
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ip addr add 10.0.0.3/16 dev veth1
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ip a  
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
    inet 127.0.0.1/8 scope host lo  
       valid_lft forever preferred_lft forever  
    inet6 ::1/128 scope host   
       valid_lft forever preferred_lft forever  
2: veth1@veth0: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN group default qlen 1000  
    link/ether 9a:9d:b3:04:a1:e6 brd ff:ff:ff:ff:ff:ff  
    inet 10.0.0.3/16 scope global veth1  
       valid_lft forever preferred_lft forever  
3: veth0@veth1: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN group default qlen 1000  
    link/ether ea:64:85:54:c6:10 brd ff:ff:ff:ff:ff:ff  
    inet 10.0.0.2/16 scope global veth0  
       valid_lft forever preferred_lft forever
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ip link set dev veth0 up
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ip link set dev veth1 up
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ping 10.0.0.2  
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.  
64 bytes from 10.0.0.2: icmp_seq=1 ttl=64 time=0.030 ms  
64 bytes from 10.0.0.2: icmp_seq=2 ttl=64 time=0.050 ms  
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# ping 10.0.0.3  
PING 10.0.0.3 (10.0.0.3) 56(84) bytes of data.  
64 bytes from 10.0.0.3: icmp_seq=1 ttl=64 time=0.051 ms  
64 bytes from 10.0.0.3: icmp_seq=2 ttl=64 time=0.124 ms  
---
















 



 

 















 

