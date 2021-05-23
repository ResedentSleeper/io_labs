# Лабораторная работа 2

**Название:** Разработка драйверов блочных устройств

**Цель работы:** Получить знания и навыки разработки драйверов блочных устройств для операционной системы Linux.

## Описание функциональности драйвера
При загрузке модуля создается блочное устройство с одними первичным разделом размером 10Мбайт и одним расширенным разделом, содержащих два логических раздела размером 20Мбайт каждый.

## Инструкция по сборке
Для сборки драйвера выполнить:
```
make
```

## Инструкция пользователя
После успешной сборки загрузить полученный модуль:
```
sudo insmod lab2.ko
```
Проверить, что драйвер загрузился без ошибок с помощью команды `dmesg | tail -n 30` получаем:
```
...
lab2: successfully loaded: disk=lab2, major=251
lab2: disk open
lab2: disk release
```

## Примеры использования
После загрузки проверяем, что драйвер создал блочное устройство согласно заданию
c помощью `sudo fdisk -l /dev/lab2`:
```
Disk /dev/lab2: 50 MiB, 52428800 bytes, 102400 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x00000000

Device      Boot Start    End Sectors Size Id Type
/dev/lab2p1          1  20479   20479  10M 83 Linux
/dev/lab2p2      20480 102399   81920  40M  5 Extended
/dev/lab2p5      20481  61439   40959  20M 83 Linux
/dev/lab2p6      61441 102399   40959  20M 83 Linux

```
После можно отформатировать новые разделы и смонтировать их в директорию `/mnt`. Выполним команду `sudo make setup` и убедимся что все сработало без ошибок выполнив `sudo lsblk -l /dev/lab2`:
```
NAME   MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
lab2   251:0    0  50M  0 disk 
lab2p1 251:1    0  10M  0 part /mnt/lab2p1
lab2p2 251:2    0   1K  0 part 
lab2p5 251:5    0  20M  0 part /mnt/lab2p5
lab2p6 251:6    0  20M  0 part /mnt/lab2p6
```
Далее можно провести бенчмарки, которые измеряют скорость передачи файла 9MB между различными разделами созданного блочного устройства и реального SSD выполнив `sudo make bench`:
```
src=/mnt/lab2p1, dst=/mnt/lab2p5, median speed=243.0, units=MB/s
src=/mnt/lab2p1, dst=/mnt/lab2p6, median speed=247.0, units=MB/s
src=/mnt/lab2p1, dst=/root, median speed=272.0, units=MB/s
src=/mnt/lab2p5, dst=/mnt/lab2p1, median speed=249.0, units=MB/s
src=/mnt/lab2p5, dst=/mnt/lab2p6, median speed=244.0, units=MB/s
src=/mnt/lab2p5, dst=/root, median speed=281.0, units=MB/s
src=/mnt/lab2p6, dst=/mnt/lab2p1, median speed=242.0, units=MB/s
src=/mnt/lab2p6, dst=/mnt/lab2p5, median speed=240.0, units=MB/s
src=/mnt/lab2p6, dst=/root, median speed=275.0, units=MB/s
src=/root, dst=/mnt/lab2p1, median speed=237.0, units=MB/s
src=/root, dst=/mnt/lab2p5, median speed=233.0, units=MB/s
src=/root, dst=/mnt/lab2p6, median speed=239.0, units=MB/s
```
После звершения работы можно отмонтировать разделы и выгрузить модуль из ядра:
```
sudo make umount
sudo rmmod lab2
```
