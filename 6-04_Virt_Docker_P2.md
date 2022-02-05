# Домашнее задание к занятию "6.4. Docker (ч.2)"

**

**Правила выполнения заданий к занятию "6.4. Docker (ч.2)"**

- Все задания делайте на основе конфигов из лекции. 
- В заданиях описаны те параметры которые необходимо изменить. 
- Если параметр не упомянут вообще, значит его нужно оставить таким, какой он был в лекции. 
- Если в каком-то задании, например №2, нужно изменить параметр, подразумевается, что во всех последующих заданиях будет использоваться  уже изменённый параметр.
- Выполнив все задания без звёздочки, вы должны получить полнофункциональный сервис

Любые вопросы по решению задач задавайте в чате Slack.

---

### Задание 1. 

Установите Docker Compose. Опишите, для чего нужен Docker Compose и как он может улучшить вашу жизнь.

*Приведите ответ в свободной форме на 1 абзац текста*

Docker Compose — это инструментальное средство, входящее в состав Docker. Оно разработано для помощи в определении и совместном использовании многоконтейнерных приложений. С помощью Docker Compose можно создать 1 YAML-файл для определения служб и с помощью одной команды запустить и остановить все, что нужно при развертывании многоконтейнерных приложений.

---

### Задание 2. 

Создайте файл docker-compose.yml и внесите туда первичные настройки. 
version
services

networks

При выполнении задания используйте подсеть 172.22.0.0
Ваша подсеть должна называться <ваши фамилия и инициалы>-my-netology-hw

*Приложите текст конфига на этом этапе*

<a href="https://ibb.co/KhMs7Kz"><img src="https://i.ibb.co/h9PdHY1/2-1.png" alt="2-1" border="0"></a>

---

### Задание 3. 

Установите PostgreSQL с именем контейнера <ваши фамилия и инициалы>-netology-db. 
Предсоздайте БД <ваши фамилия и инициалы>-db
Задайте пароль пользователя postgres как <ваши фамилия и инициалы>12!3!!
Пример названия контейнера: ivanovii-netology-db.

Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

*Приложите текст конфига текущего сервиса*

<a href="https://ibb.co/CbxFBZS"><img src="https://i.ibb.co/T4z5cSN/3-1.png" alt="3-1" border="0"></a>

---

### Задание 4. 

Установите pgAdmin с именем контейнера <ваши фамилия и инициалы>-pgadmin. 
Задайте логин администратора pgAdmin <ваши фамилия и инициалы>@ilove-netology.com и пароль на выбор.

Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.
Прокиньте на 80й порт контейнера порт 61231

*Приложите текст конфига текущего сервиса*
*Приложите скриншот админки pgAdmin*

<a href="https://ibb.co/J355C8C"><img src="https://i.ibb.co/PDwwtJt/4-1.png" alt="4-1" border="0"></a>

<a href="https://ibb.co/sJckgtz"><img src="https://i.ibb.co/Xk6NWVM/4-2.png" alt="4-2" border="0"></a>

---

### Задание 5. 

Установите Zabbix Server с именем контейнера <ваши фамилия и инициалы>-zabbix-netology. 
Настройте его подключение к вашему СУБД

Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

*Приложите текст конфига текущего сервиса*

<a href="https://ibb.co/9ZFWpvg"><img src="https://i.ibb.co/QD2MJcQ/5-1.png" alt="5-1" border="0"></a>

---

### Задание 6. 

Установите Zabbix Frontend с именем контейнера <ваши фамилия и инициалы>-netology-zabbix-frontend. 
Настройте его подключение к вашему СУБД.

Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

*Приложите текст конфига текущего сервиса

<a href="https://ibb.co/CJPt05B"><img src="https://i.ibb.co/28Z6K7q/6-1.png" alt="6-1" border="0"></a>

---

### Задание 7. 

Настройте линки, чтобы контейнеры запускались только, когда запущены контейнеры, от которых они зависят.

*Приложите текст конфига ЦЕЛИКОМ
Пришлите скриншот команды docker ps
Приложите скриншот авторизации в админке Zabbix*


start docker-compose.yaml 

version: '3'

services:

  bakarasag-netology-db:
    image: postgres:13 # Образ, который мы будем использовать
    container_name: bakarasag-netology-db # Имя, которым будет называться наш контейнер
    ports: # Порты, которые мы пробрасываем с нашего докер сервера внутрь контейнера
      - 5432:5432
    volumes: # Папка, которую мы пробросим с докер сервера внутрь контейнера
      - ./pg_data:/var/lib/postgresql/data/pgdata
    environment: # Переменные среды
      POSTGRES_PASSWORD: bakarasag12!3!! # Задаём пароль от пользователя postgres
      POSTGRES_DB: bakarasag_db # БД которая сразу же будет создана
      PGDATA: /var/lib/postgresql/data/pgdata # Путь внутри контейнера, где будет папка pgdata
    networks:
      bakarasag-my-netology-hw:
        ipv4_address: 172.22.0.2
    restart: always # Режим перезапуска контейнера. Контейнер всегда будет перезапускаться

  pgadmin:
    image: dpage/pgadmin4
    links:
      - bakarasag-netology-db
    container_name: bakarasag-pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: bakarasag@ilove-netology.com
      PGADMIN_DEFAULT_PASSWORD: bakarasag12!3!!
    ports:
      - 61231:80
    networks:
      bakarasag-my-netology-hw:
        ipv4_address: 172.22.0.3
    restart: always

  zabbix-server:
    image: zabbix/zabbix-server-pgsql
    links:
      - bakarasag-netology-db
      - pgadmin
    container_name: bakarasag-zabbix-netology
    environment:
      DB_SERVER_HOST: '172.22.0.2'
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: bakarasag12!3!!
    ports:
      - 10051:10051
    networks:
      bakarasag-my-netology-hw:
        ipv4_address: 172.22.0.4
    restart: always

  zabbix_wgui:
    image: zabbix/zabbix-web-apache-pgsql
    links:
      - bakarasag-netology-db
      - pgadmin
      - zabbix-server
    container_name: bakarasag-netology-zabbix-frontend
    environment:
      DB_SERVER_HOST: '172.22.0.2'
      POSTGRES_USER: 'postgres'
      POSTGRES_PASSWORD: bakarasag12!3!!
      ZBX_SERVER_HOST: "zabbix_wgui"
      PHP_TZ: "Europe/Moscow"
    ports:
      - 80:8080
      - 443:8443
    networks:
      bakarasag-my-netology-hw:
        ipv4_address: 172.22.0.5
    restart: always

networks:
  bakarasag-my-netology-hw:
    driver: bridge
    ipam:
      config:
      - subnet: 172.22.0.0/24

end docker-compose.yaml


<a href="https://ibb.co/YjzHPkJ"><img src="https://i.ibb.co/5cNbG8C/7-1.png" alt="7-1" border="0"></a>


<a href="https://ibb.co/LNvXLcg"><img src="https://i.ibb.co/vXD67Td/7-2.png" alt="7-2" border="0"></a>

---

### Задание 8. 

Убейте все контейнеры и потом удалите их

*Приложите скриншот консоли с проделанными действиями*

<a href="https://ibb.co/nfvFyfS"><img src="https://i.ibb.co/tct96c5/8-1.png" alt="8-1" border="0"></a>

---

**

## Дополнительные задания (со звездочкой*)
Эти задания дополнительные (не обязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. Вы можете их выполнить, если хотите глубже и/или шире разобраться в материале.

### Задание 9*. 

Запустите свой сценарий на чистом железе без предзагруженных образов.

*Скажите сколько ушло времени на то, чтобы развернуть на чистом железе написанный вами сценарий.
Опишите то, чем вы занимались в процессе создания сценария так, как это видите вы.
Скажите, что бы вы улучшили в сценарии развёртывания*

1. На "чистом железе" для того, чтобы развернуть написанный сценарий ушло порядка 7 минут. Это вместе с загрузкой образов и запуском контейнеров;

2. Редактировал скрипт под свои задачи. Проверял все ли корректно запускается. Просматривал описание образов на docker-hub. Не все сразу получалось, но если чаще практиковаться с созданием сценариев, все пойдет гораздо быстрее;

3. Проверил бы образы на совместимость друг с другом, чтобы в процессе развертывания совсем не возникало ошибок. Пожалуй, больше ничего. Мало опыта в работе с контейнерами.