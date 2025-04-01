# tales-lessons
Here we will learn how to use k8s via tacos

Introduction How to install talos on hypervisors

On MacOS M2 we shoud install arm iso
We can find iso via link 

```
https://github.com/siderolabs/talos/releases
```

Now latest stable version is v1.9.5

-----
Позже переведём эту часть на английский
-----

Первым делом для запуска talos на Mac os ставим vmware fusion у меня версия 13.6.3

Тут мы добавляем машину 

И так после запуска iso образа видим следущее тут нам нужно установить операционную систему на наш выделенный диск

И так для работы с виртуалкой в моём случае - talos-work-master-1

нужно установить утилиту talosctl, ставится она просто через brew

```
brew install siderolabs/tap/talosctl

talosctl version
```

Скачав утилиту, нам нужно сгенерировать конфиг файл для машины
В моём случае - это 192.168.0.71

```
talosctl gen config test-cluster https://192.168.0.71:6443
```
В выводе терминала увидим следущее 

```
generating PKI and tokens
Created /Users/otabekhamdamov/projects/fusion/tales-lessons/configs/controlplane.yaml
Created /Users/otabekhamdamov/projects/fusion/tales-lessons/configs/worker.yaml
Created /Users/otabekhamdamov/projects/fusion/tales-lessons/configs/talosconfig
```

Он, talosctl сгенерирует конфиг файлы в директории, где вы находитесь

В зависимости какой диск вы выдали для виртуалки SATA или NVME нужно будет поменять в 
конфиг файле директорию для установки

Вот эта часть

```
    install:
        disk: /dev/sda # The disk used for installations.
        image: ghcr.io/siderolabs/installer:v1.9.5 # Allows for supplying the image used to perform the installation.
        wipe: false # Indicates if the installation disk should be wiped at installation time.
        
```

Далее для удобства экспортируем переменную окружения ip talos
```
export TALOS_MASTER=192.168.0.71
```

Можно проверить дирректорию выданной для виртуалки

```
talosctl get disks -n $TALOS_MASTER -e $TALOS_MASTER -i 
NODE   NAMESPACE   TYPE   ID      VERSION   SIZE     READ ONLY   TRANSPORT   ROTATIONAL   WWID                   MODEL              SERIAL
       runtime     Disk   loop0   2         66 MB    true                                                                           
       runtime     Disk   sda     2         27 GB    false       sata        true         naa.5000c295db3fa9b6   VMware Virtual S   
       runtime     Disk   sr0     2         287 MB   false       sata                                            VMware SATA CD01   
otabekhamdamov@Mac configs % 
```

как видно в моём случае название смонированной директории /disk/sda