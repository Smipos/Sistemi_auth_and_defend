# Сбор и аналитическая обработка информации о сетевом трафике
## Цель работы
+ Развить практические навыки использования современного стека инструментов сбора и аналитической обработки информации о сетвом трафике
+ Освоить базовые подходы блокировки нежелательного сетевого трафика
+ Закрепить знания о современных сетевых протоколах прикладного уровня
## Ход работы
+ Собрал сетевой трафик с помощью Wireshark

![](https://github.com/Smipos/Sistemi_auth_and_defend/blob/main/lab2/img_lab2/trafic.jpg)

+ С помощью утилиты Zeek выделяю метаинформацию сетевого трафика (http.log/dns.log)

+ Загрузил и соединил файлы, содержащие списки источников нежелательного трафика:

```
mkdir hosts
cd hosts
wget -q https://github.com/StevenBlack/hosts/raw/master/data/add.2o7Net/hosts
wget -q https://raw.githubusercontent.com/StevenBlack/hosts/master/data/KADhosts/hosts
wget -q https://raw.githubusercontent.com/StevenBlack/hosts/master/data/yoyo.org/hosts
wget -q https://raw.githubusercontent.com/StevenBlack/hosts/master/data/tiuxo/hosts
wget -q https://raw.githubusercontent.com/StevenBlack/hosts/master/data/URLHaus/hosts
sort hosts* | grep "^[^#;]" | uniq > final_hosts
mv final_hosts ../hosts.data
cd ..
rm -rf hosts
```
В результате получил файл hosts.data

