# Сбор и аналитическая обработка информации о сетевом трафике
## Цель работы
+ Развить практические навыки использования современного стека инструментов сбора и аналитической обработки информации о сетвом трафике
+ Освоить базовые подходы блокировки нежелательного сетевого трафика
+ Закрепить знания о современных сетевых протоколах прикладного уровня
## Ход работы
1. Собрал сетевой трафик с помощью Wireshark

![](https://github.com/Smipos/Sistemi_auth_and_defend/blob/main/lab2/img_lab2/trafic.jpg)

2. С помощью утилиты Zeek выделяю метаинформацию сетевого трафика (http.log / dns.log)

3. Загрузил и соединил файлы, содержащие списки источников нежелательного трафика:

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

4. Установил библиотеку ZAT (Zeek Analysis Toolkit) для преобразоваия метаинформации сетевого трафика в формате log-файлов в датафрейм Pandas:

```
pip install zat
```

5. После установки библиотеки преобразуем файл dns.log в датафрейм Pandas

```
import warnings
warnings.filterwarnings("ignore", category=RuntimeWarning)
import numpy as np
import pandas as pd
from zat.log_to_dataframe import LogToDataFrame
df = LogToDataFrame()
z_df = df.create_dataframe('dns.log')
domains = z_df['query']
domains.name = 'CNAME'
```

6. Преобразуем файл со списком источников нежелательного трафика
```
df = pd.read_csv('hosts.data', sep="\s+", names=['redirect_to','CNAME'])
bd_domains = df['CNAME']
```

7. Объединим два полученных датафрейма и получим процент нежелательного трафика
```
merged = pd.merge(domains, bd_domains, on='CNAME', how='left', indicator='exists')
merged['exists'] = np.where(merged.exists == 'both', True,False)
count = merged['exists'].value_counts()[1]
percentile = round(merged['exists'].value_counts(normalize=True)[1]*100, 2)
print("DNS имен из списков трафика: {}.".format(count), "Процент нежелательного трафика: {}%.".format(percentile), sep='\n')
```
```
Вхождений DNS имён из списков в собранном трафике: 94.
Процент нежелательного трафика: 2.47%.
```
## Оценка результата
В результате лабораторной работы научился определять нежелательный трафик.

## Вывод
Таким образом, мы научились анализировать сетевой трафик.
