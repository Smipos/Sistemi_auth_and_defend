# Развертывание Threat intelligence Platform OpenCTI

## Цель работы 

1. Освоить базовые подходы процессов Threat Intelligence
2. Освоить современные инструменты развертывания контейнеризованных приложений
3. Получить навыки поиска информации об угрозах ИБ

## Ход выполнения работы

1. Подготовка файлов кофигураций

    * Клонируем репозитурий с GitHub
        ```
        sudo git clone https://github.com/OpenCTI-Platform/docker.git 
        ```

        ![](./pics/s1.png)

    * Генерируем UUID

        ```
        cat /proc/sys/kernel/random/uuid
        ``` 
    
    * Увеличиваем виртуальную память

        ```
        sudo sysctl -w vm.max_map_count=1048575
        ```

        ![](./pics/s3.png)

    * Конфигурируем файл `.env`

        ![](./pics/s2.png)

    * Дополнительная установка ELK

        ```
        docker pull elastic/elasticsearch:8.6.2
        ```

2. Запуск OpenCTI

    * Запуск контейнера в фоновом режиме

    ```
    sudo docker-compose up -d
    ```

    * Запуск проводиться в течении 8 минут, и после этого, можно перейти на `localhost:8088`

    ![](./pics/s4.png)

3. Перейдем в веб-интерфейс OpenCTI

    ![](./pics/s5.png)

4. Зайдем на сайт с документацией по импорту файлов и увидем, что существует библиотека python для работы с OpenCTI. Напишем код для импорта файла в opencti:

    ```
    import pycti
    from datetime import datetime

    date = datetime.today().strftime("%Y-%m-%dT%H:%M:%SZ")

    api_url = 'http://localhost:8080'
    api_token = '88ce2416-b1d8-468e-bd2f-974737cf3876'
    client = pycti.OpenCTIApiClient(api_url, api_token)

    with open('hosts.txt', 'r') as f:
        domains = f.read().splitlines()
    k = 1
    for domain in domains:
        indicator = client.indicator.create(
        name="HOST number  {}".format(k),
        description="For pr5 RTU MEMREA",
        pattern_type="stix",
        pattern="[domain-name:value = '{}']".format(domain),
        x_opencti_main_observable_type="IPv4-Addr",
        valid_from=date,
        update=True,
        score=75,
        )
        print("Created indicator with ID:", indicator["id"])
        k += 1


    ```

    Ответ:
    
    ![](./pics/s6.png)

    ![](./pics/s7.png)

5. Преобразуем все индикаторы в Observables

    ![](./pics/s8.png)

    ![](./pics/s9.png)

6. Импортируем сетевой трафик, полученный в lab_2 в OpenCTI

    ![](./pics/s10.png)

7. Добавим этот файл в рабочую область

    ![](./pics/s11.png)

8. Перейдем в раздел с анализом и отфильтруем поиск по нежелательному траффику

    ![](./pics/s12.png)

    Отсюда выясняем, что пользователь посетил 40 нежелательных доменов

## Оценка результата

С помощью платформы OpenCTI удалось проанализировать трафик на предмет перехода по нежелательным доменам.


# Выводы

Таким образом, были изучены возможности работы с платформой threat intelligence OpenCTI


