# Лабораторная работа 2

**"Разработка драйверов блочных устройств"**

**Цель работы:** получить знания и навыки разработки драйверов блочных устройств для операционной системы Linux.

## Описание функциональности

Драйвер блочного устройства создает виртуальный жесткий диск в оперативной памяти с размером 50 Мбайт. Созданный виртуальный диск содержит один первичный раздел размером 10Мбайт и один расширенный раздел, содержащий два логических раздела размером 20Мбайт каждый.

## Инструкция по сборке и использованию

Собирается с помощью команды ```make```.

Для запуска драйвера необходимо ввести команду ```insmod bl_driver.ko``` , которая загружает модуль ядра в систему. Для выгрузки модуля драйвера нужно ввести команду ```sudo rmmod bl_driver```.

## Вывод разделов виртуального диска

```
eve@eve-pc:~/Desktop/lab2$ sudo parted /dev/mydisk
GNU Parted 3.2
Using /dev/mydisk
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) unit mib print                                                   
Model: Unknown (unknown)
Disk /dev/mydisk: 50,0MiB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags: 

Number  Start    End      Size     Type      File system  Flags
 1      0,00MiB  10,0MiB  10,0MiB  primary
 2      10,0MiB  50,0MiB  40,0MiB  extended
 5      10,0MiB  30,0MiB  20,0MiB  logical
 6      30,0MiB  50,0MiB  20,0MiB  logical
```
