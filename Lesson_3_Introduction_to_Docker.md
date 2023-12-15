### Дёров Константин Павлович 
### Группа № 4719
### Семинар 3
---
### Задание
 1) запустить контейнер с БД, отличной от mariaDB, используя инструкции на сайте: https://hub.docker.com/
2) добавить в контейнер hostname такой же, как hostname системы через переменную
3) заполнить БД данными через консоль
4) запустить phpmyadmin (в контейнере) и через веб проверить, что все введенные данные доступны.
УЧТИТЕ, что если используете Postgres, то вместо phpmyadmin придется ставить adminer, он универсальный и минималистичный, совместим со всеми СУБД, а вот графическая оболочка PhpMyAdmin - только с Mysql и MariaDB! А так все остальное - по условиям выше!
--- 
### Решение.
### Установка Docker.
* https://docs.docker.com/engine/install/ubuntu/
* konstantin@konstantin-LinuxVirtualBox:~$ 
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done  
[sudo] пароль для konstantin:  
Чтение списков пакетов… Готово    
-//-  
* konstantin@konstantin-LinuxVirtualBox:~$ 
   >*#* Add Docker's official GPG key:   
   sudo apt-get update  
   sudo apt-get install ca-certificates curl gnupg  
   sudo install -m 0755 -d /etc/apt/keyrings  
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg  
   sudo chmod a+r /etc/apt/keyrings/docker.gpg  
   *#* Add the repository to Apt sources:  
   echo \ 
   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
   $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   sudo apt-get update

   Игн:1 https://packages.medibuntu.org jammy InRelease  
   Сущ:2 http://ru.archive.ubuntu.com/ubuntu jammy InRelease  
   Пол:3 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]  
   Пол:4 http://ru.archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]  
   Сущ:5 https://download.docker.com/linux/ubuntu jammy InRelease   
   Сущ:6 http://ru.archive.ubuntu.com/ubuntu jammy-backports InRelease   
   Пол:7 http://repo.mysql.com/apt/ubuntu artful InRelease [16,8 kB]      
   -//-  
* konstantin@konstantin-LinuxVirtualBox:~$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin  
Чтение списков пакетов… Готово  
-//-  
* konstantin@konstantin-LinuxVirtualBox:~$  sudo docker run hello-world  
docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.  
See 'docker run --help'. 
* konstantin@konstantin-LinuxVirtualBox:~$ sudo systemctl status docker  
[sudo] пароль для konstantin:  
× docker.service - Docker Application Container Engine   
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset>  
     Active: failed (Result: exit-code) since Tue 2023-12-12 19:22:23 +07; 25mi>  
TriggeredBy: × docker.socket  
       Docs: https://docs.docker.com  
    Process: 5881 ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/contain>  
   Main PID: 5881 (code=exited, status=1/FAILURE)  
        CPU: 170ms  
-//-  
* konstantin@konstantin-LinuxVirtualBox:~$ sudo systemctl start docker
* konstantin@konstantin-LinuxVirtualBox:~$ sudo systemctl status docker  
● docker.service - Docker Application Container Engine  
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset>  
     Active: active (running) since Tue 2023-12-12 19:56:02 +07; 25s ago  
TriggeredBy: ● docker.socket  
       Docs: https://docs.docker.com  
   Main PID: 6248 (dockerd)  
      Tasks: 12  
     Memory: 26.2M  
        CPU: 622ms  
     CGroup: /system.slice/docker.service  
             └─6248 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/cont>  
-//-   
* konstantin@konstantin-LinuxVirtualBox:~$ sudo docker run hello-world  
Unable to find image 'hello-world:latest' locally  
latest: Pulling from library/hello-world  
719385e32844: Pull complete  
Digest: sha256:c79d06dfdfd3d3eb04cafd0dc2bacab0992ebc243e083cabe208bac4dd7759e0  
Status: Downloaded newer image for hello-world:latest  
Hello from Docker!  
This message shows that your installation appears to be working correctly.  
-//-  
 
* konstantin@konstantin-LinuxVirtualBox:~$ sudo su
[sudo] пароль для konstantin: 
* root@konstantin-LinuxVirtualBox:/home/konstantin# cd project.local
### Создадим папку, которую мы будем монтировать в контейнер - dockertest.
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# mkdir dockertest
### В этой папке создадим файл test.txt и наполним данными - text
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# echo text >>/home/konstantin/project.local/dockertest/test.txt
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# cat /home/konstantin/project.local/dockertest/test.txt  
text
### В домашней директории создадим файл test.txt, который также смонтируем в контейнер и наполним другими данными -  secondtext 
* root@konstantin-LinuxVirtualBox:/home/konstantin/project.local# cd ~
* root@konstantin-LinuxVirtualBox:~# pwd  
/root
* root@konstantin-LinuxVirtualBox:~# echo secondtext >> test.txt
* root@konstantin-LinuxVirtualBox:~# cat test.txt  
secondtext  

### Создадим контейнер из образа ubuntu:22.04. Зададим ему имя, зададим hostname, смонтируем созданную ранее папку с хоста в контейнер, смонтируем созданный ранее текстовый файл  внутрь смонтированной папки, чтобы он пересекался с созданным ранее файлом в этой папке.  
* root@konstantin-LinuxVirtualBox:~# docker run -it --name testcontainer -h hostcontainername -v /dockertest/:/cont/ -v /root/test.txt:/cont/test.txt ubuntu:22.04 /bin/bash  
Unable to find image 'ubuntu:22.04' locally  
22.04: Pulling from library/ubuntu  
5e8117c0bd28: Pull complete   
Digest: sha256:8eab65df33a6de2844c9aefd19efe8ddb87b7df5e9185a4ab73af936225685bb  
Status: Downloaded newer image for ubuntu:22.04
### Просмотрим этот файл  
* root@hostcontainername:/# ll /cont/test.txt  
-rw-r--r-- 1 root root 11 Dec 15 12:50 /cont/test.txt  
* root@hostcontainername:/# cat /cont/test.txt  
secondtext

### Запустим в контейнере базу данных MariaDB.
* root@konstantin-LinuxVirtualBox:~# docker run --detach --name testmariadb --env MARIADB_ROOT_PASSWORD=1  mariadb:10.10.2  
Unable to find image 'mariadb:10.10.2' locally  
10.10.2: Pulling from library/mariadb  
10ac4908093d: Pull complete  
44779101e748: Pull complete   
a721db3e3f3d: Pull complete   
1850a929b84a: Pull complete   
397a918c7da3: Pull complete   
806be17e856d: Pull complete   
634de6c90876: Pull complete   
cd00854cfb1a: Pull complete   
Digest: sha256:bfc25a68e113de43d0d112f5a7126df8e278579c3224e3923359e1c1d8d5ce6e  
Status: Downloaded newer image for mariadb:10.10.2  
4dadf13faab25e5510d4169f99a361723c944a94bb834bd5f7e00a98732836d1 
*  root@konstantin-LinuxVirtualBox:~# docker exec -it  testmariadb /bin/bash  
root@4dadf13faab2:/# mysql -u root -p  
Enter password:   
Welcome to the MariaDB monitor.  Commands end with ; or \g.  
Your MariaDB connection id is 3  
Server version: 10.10.2-MariaDB-1:10.10.2+maria~ubu2204 mariadb.org binary distribution  
Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.  
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.  
* MariaDB [(none)]> show databases;  
+--------------------+  
| Database           |  
+--------------------+  
| information_schema |  
| mysql              |  
| performance_schema |  
| sys                |  
+--------------------+  
4 rows in set (0.002 sec)  
### Добавим новую базу данных.
* MariaDB [(none)]> create database newdatabase;  
Query OK, 1 row affected (0.000 sec)  
* MariaDB [(none)]> show databases;  
+--------------------+  
| Database           |  
+--------------------+  
| information_schema |  
| mysql              |  
| newdatabase        |  
| performance_schema |  
| sys                |  
+--------------------+  
5 rows in set (0.001 sec)  
### Заполним БД данными через консоль(Создадим таблицу студентов).
* MariaDB [(none)]> use newdatabase;  
Database changed  
* MariaDB [newdatabase]> create table Students (id int not null auto_increment primary key, name varchar(15), age int);  
Query OK, 0 rows affected (0.022 sec)  
* MariaDB [newdatabase]> INSERT INTO Students (name, age) VALUES ('Fedor', 30),('Alexandr', 40);  
Query OK, 2 rows affected (0.005 sec)  
Records: 2  Duplicates: 0  Warnings: 0 
*  MariaDB [newdatabase]> select * from Students;  
+----+----------+------+  
| id | name     | age  |  
+----+----------+------+  
|  1 | Fedor    |   30 |  
|  2 | Alexandr |   40 |  
+----+----------+------+  
2 rows in set (0.000 sec)
* MariaDB [newdatabase]> exit  
Bye  
* root@4dadf13faab2:/# exit  
exit
### Запустить phpmyadmin (в контейнере) и через веб проверить, что все введенные данные доступны.
* root@konstantin-LinuxVirtualBox:~# docker run --name testphpmyadmin -h hostnamephpmyadmin -d --link testmariadb:db -p 3307:80 phpmyadmin:latest 
Unable to find image 'phpmyadmin:latest' locally 
latest: Pulling from library/phpmyadmin 
1f7ce2fa46ab: Already exists  
48824c101c6a: Pull complete  
249ff3a7bbe6: Pull complete  
aa5d47f22b64: Pull complete  
851cb5d3b62c: Pull complete  
090f07e09d3e: Pull complete  
74f97600920f: Pull complete  
f48a9f994636: Pull complete  
108b4c091efa: Pull complete  
94f753607622: Pull complete  
5d0ec11ef45d: Pull complete  
87757e6fac28: Pull complete  
899a04597fc2: Pull complete   
923a5af9f066: Pull complete   
535910abb92a: Pull complete   
b7435a0c2c73: Pull complete   
0185f3846389: Pull complete   
632258113114: Pull complete   
Digest: sha256:f84a1dc2fc90f8e55f03d8ba4144da4fc8d4b90952b9a3571ed918a533b9a40e  
Status: Downloaded newer image for phpmyadmin:latest  
cb536fa6baeabdfc9a557ce8cb2be6cb4edafd1dfff7f2988ec7efe2ab21f346 

в браузере пишем - lokalhost:3307
и смотрим данные нашей таблици



  











