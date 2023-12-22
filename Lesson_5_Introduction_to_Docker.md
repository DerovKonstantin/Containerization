### Дёров Константин Павлович 
### Группа № 4719
### Семинар 5
---
### Задание
Задание 1:
Развернуть и запустить контейнер с любым вашим образом(сайт на CMS, образ ОС, образ базы данных, что угодно). Использовать строго Compose!
--- 
### Решение.
Для выполнения задания используем Docker Compose, для развертывания и запуска контейнера с нашим образом.  
* konstantin@konstantin-LinuxVirtualBox:~$ ls  
 mysql-apt-config_0.8.10-1_all.deb   Видео         Общедоступные  
 project.local                       Документы    'Рабочий стол'  
 shared                              Загрузки      Шаблоны  
 snap                                Изображения  
 zip_3.0-12build2_amd64.deb          Музыка  
* konstantin@konstantin-LinuxVirtualBox:~$ cd  project.local  
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ ls  
Dockeralpine  Dockerfile  dockertest  index.php  testdirectory  while_sample.sh  
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ mkdir Dockercomopse  
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ cd Dockercomopse  
* konstantin@konstantin-LinuxVirtualBox:~/project.local/Dockercomopse$ nano docker-compose.yml  
---
version: '3.1'  
services:  
  wordpress:  
    image: wordpress  
    restart: unless-stopped  
    ports:  
      - 8580:80  
    environment:  
      WORDPRESS_DB_HOST: db  
      WORDPRESS_DB_USER: exampleuser  
      WORDPRESS_DB_PASSWORD: examplepass  
      WORDPRESS_DB_NAME: exampledb  
    volumes:  
      - wordpress:/var/www/html  
  db:  
    image: mysql:8.0  
    restart: unless-stopped  
environment:  
      MYSQL_DATABASE: exampledb  
      MYSQL_USER: exampleuser  
      MYSQL_PASSWORD: examplepass  
      MYSQL_RANDOM_ROOT_PASSWORD: '1'  
    volumes:  
      - db:/var/lib/mysql  
volumes:  
  wordpress:  
  db:  

---
* konstantin@konstantin-LinuxVirtualBox:~/project.local/Dockercomopse$ docker ps -a  
CONTAINER ID   IMAGE         COMMAND           CREATED      STATUS                  PORTS     NAMES  
634d043164ab   sumofdigits   "python app.py"   3 days ago   Exited (0) 3 days ago             funny_lehmann  
* konstantin@konstantin-LinuxVirtualBox:~/project.local/Dockercomopse$ docker system df  
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE  
Images          1         1         48.2MB    0B (0%)  
Containers      1         0         255.6kB   255.6kB (100%) 
Local Volumes   2         0         142.1MB   142.1MB (100%)  
Build Cache     10        0         943B      943B  
* konstantin@konstantin-LinuxVirtualBox:~/project.local/Dockercomopse$ sudo apt update  
[sudo] пароль для konstantin:   
-//-   
* konstantin@konstantin-LinuxVirtualBox:~/project.local/Dockercomopse$ sudo apt install docker-compose  
Чтение списков пакетов… Готово  
Построение дерева зависимостей… Готово  
Чтение информации о состоянии… Готово  
-//-  
* onstantin@konstantin-LinuxVirtualBox:~/project.local/Dockercomopse$ docker-compose up -d  
Creating network "dockercomopse_default" with the default driver  
Creating volume "dockercomopse_wordpress" with default driver  
Creating volume "dockercomopse_db" with default driver  
-//-  
Creating dockercomopse_wordpress_1 ... done  
Creating dockercomopse_db_1        ... done  
* kkonstantin@konstantin-LinuxVirtualBox:~/project.local/Dockercomopse$ docker ps -a  
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                  PORTS                                   NAMES  
eda7993a6be5   mysql:8.0     "docker-entrypoint.s…"   43 seconds ago   Up 40 seconds           3306/tcp, 33060/tcp                     dockercomopse_db_1  
5de8e2b8aad2   wordpress     "docker-entrypoint.s…"   43 seconds ago   Up 41 seconds           0.0.0.0:8580->80/tcp, :::8580->80/tcp   dockercomopse_wordpress_1  
634d043164ab   sumofdigits   "python app.py"          3 days ago       Exited (0) 3 days  ago                                           funny_lehmann  
---
в браузере пишем - http://localhost:8580 и заходим на страничьку.
![Это мой Wordpress!](DiorOFF_Wordpress$$$.bmp)

