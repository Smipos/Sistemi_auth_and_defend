---
title: "lab1"
format: html
editor: visual
---

## Цель работы 
Получить сведения о системе.

## Исходные данные
1. ПО Windows 10
2. RStudio

## План работы
1. Версия ядра
2. Полные сведения о ядре
3. Используемый дистрибутив
4. Модель процессора
5. Информация о скрытых файлах


##

1. Получить версию ядра можно с помощью команды: uname -r

{bash}
uname -r

![Версия ядра](https://github.com/Smipos/Sistemi_auth_and_defend/blob/main/img_lab1/uname_r.png)

2. Полные сведения о ядре: uname -a

{bash}
uname -a


![Полные сведения о ядре](https://github.com/Smipos/Sistemi_auth_and_defend/blob/main/img_lab1/uname_a.png)

3. Используемый дистрибутив: lsb_release -a

{bash}
lsb_release -a

![Полные сведения о ядре](https://github.com/Smipos/Sistemi_auth_and_defend/blob/main/img_lab1/lsb_release_a.png)

4. Информация о процессоре: cat /proc/cpuinfo

{bash}
cat /proc/cpuinfo

![ИНформация о проце](https://github.com/Smipos/Sistemi_auth_and_defend/blob/main/img_lab1/cpu_info.png)

![Инфомарция о проце](https://github.com/Smipos/Sistemi_auth_and_defend/blob/main/img_lab1/cpu_info_2.png)
