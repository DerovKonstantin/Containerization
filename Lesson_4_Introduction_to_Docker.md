### Дёров Константин Павлович 
### Группа № 4719
### Семинар 4
---
### Задание
Задание: необходимо создать Dockerfile, основанный на любом образе (вы в праве выбрать самостоятельно).  
В него необходимо поместить приложение, написанное на любом известном вам языке программирования (Python, Java, C, С#, C++).  
При запуске контейнера должно запускаться самостоятельно написанное приложение.
--- 
### Решение.
* konstantin@konstantin-LinuxVirtualBox:~$ ls  
 mysql-apt-config_0.8.10-1_all.deb   Видео         Общедоступные  
 project.local                       Документы    'Рабочий стол'  
 shared                              Загрузки      Шаблоны  
 snap                                Изображения  
 zip_3.0-12build2_amd64.deb          Музыка  
* konstantin@konstantin-LinuxVirtualBox:~$ cd project.local
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ mkdir Dockeralpine
* konstantin@konstantin-LinuxVirtualBox:~/project.local$ cd Dockeralpine 
* konstantin@konstantin-LinuxVirtualBox:~/project.local/Dockeralpine$ nano Dockerfile
---
FROM python:3.9-alpine  
WORKDIR /project.local/Dockeralpine  
COPY app.py /project.local/Dockeralpine/  
CMD ["python", "app.py"]  

---
* konstantin@konstantin-LinuxVirtualBox:~/project.local/Dockeralpine$ nano app.py
---
print("Найдите сумму цифр любого вещественного или целого числа.")  
from decimal import Decimal  
try:  
    num = Decimal(input("Введите вещественное или целое число: - "))  
    sum = 0  
    x = 10  
    if num < 0:  
        num *= -1  
        num_x = num  
        while num_x > int(num_x):  
            num_x = num  
            num_x *= x  
            x *= 10  
        while num_x > 0:  
            sum += int(num_x)%10  
            num_x //= 10  
    else:  
        num_x = num  
        while num_x > int(num_x):  
            num_x = num  

---
* konstantin@konstantin-LinuxVirtualBox:~/project.local/Dockeralpine$ docker build -t sumofdigits .  
[+] Building 11.1s (8/8) FINISHED                                docker:default  
 => [internal] load build definition from Dockerfile                       0.1s  
 => => transferring dockerfile: 167B                                       0.0s  
 => [internal] load .dockerignore                                          0.1s  
 => => transferring context: 2B                                            0.0s  
 => [internal] load metadata for docker.io/library/python:3.9-alpine       4.3s  
 => [1/3] FROM docker.io/library/python:3.9-alpine@sha256:5a8aef2661d7c9e  5.8s  
 => => resolve docker.io/library/python:3.9-alpine@sha256:5a8aef2661d7c9e  0.1s  
 => => sha256:0ee71d02257150b46d6e1dc2816f44d78064dad04 11.61MB / 11.61MB  1.9s  
 => => sha256:5a8aef2661d7c9e8a4c4fc6e79c6da926f3154aac43 1.65kB / 1.65kB  0.0s  
 => => sha256:fbbe16e8fecbedd7d1f9f8e2d86b813d8c85b73c80b 1.37kB / 1.37kB  0.0s  
 => => sha256:1aaf34e24c2d9a1fe3365ae5d1e0aa03070149210be 6.25kB / 6.25kB  0.0s  
 => => sha256:661ff4d9561e3fd050929ee5097067c34bafc523ee6 3.41MB / 3.41MB  2.9s  
 => => sha256:44cda88cd45dc00d3349d343b46e92f0d55daa0 621.69kB / 621.69kB  0.6s  
 => => sha256:2738858d84c014c88e38adb0b43c3f7d3d6271524e8f090 240B / 240B  1.0s  
 => => sha256:faa411d0fa85851e170f2734a1eeafb2c7dac140b27 2.85MB / 2.85MB  1.9s  
 => => extracting sha256:661ff4d9561e3fd050929ee5097067c34bafc523ee60f529  0.4s  
 => => extracting sha256:44cda88cd45dc00d3349d343b46e92f0d55daa059d276f3e  0.4s  
 => => extracting sha256:0ee71d02257150b46d6e1dc2816f44d78064dad0405c24f3  0.9s  
 => => extracting sha256:2738858d84c014c88e38adb0b43c3f7d3d6271524e8f090d  0.0s  
 => => extracting sha256:faa411d0fa85851e170f2734a1eeafb2c7dac140b276ecb2  0.4s  
 => [internal] load build context                                          0.1s  
 => => transferring context: 850B                                          0.0s  
 => [2/3] WORKDIR /project.local/Dockeralpine                              0.4s  
 => [3/3] COPY app.py /project.local/Dockeralpine/                         0.1s  
 => exporting to image                                                     0.1s  
 => => exporting layers                                                    0.1s  
 => => writing image sha256:b5e0f667ba7ee33f98dab85dd4fb4e392589f9d8a7116  0.0s  
 => => naming to docker.io/library/sumofdigits                             0.0s  
* konstantin@konstantin-LinuxVirtualBox:~/project.local/Dockeralpine$ docker run sumofdigits  
Найдите сумму цифр любого вещественного или целого числа.  
Введите вещественное или целое число: - Введены некорректные данные  
* konstantin@konstantin-LinuxVirtualBox:~/project.local/Dockeralpine$ docker ps -a    
CONTAINER ID   IMAGE         COMMAND           CREATED           STATUS                     PORTS     NAMES    
634d043164ab   sumofdigits   "python app.py"   8 seconds ago   Exited (0) 6   seconds ago             funny_lehmann    





  











