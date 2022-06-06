# Домашнее задание к занятию "6.5. Kubernetes ч.1"

**

### Задание 1.

Запустите кубернетес локально, используя k3s или minikube на свой выбор.
Добейтесь стабильной работы всех системных контейнеров.

*В качестве ответа пришлите скриншот результата выполнения команды kubectl get po -n kube-system*

<a href="https://ibb.co/5sJWbnb"><img src="https://i.ibb.co/kqPgN9N/1-1.png" alt="1-1" border="0"></a>

------
### Задание 2.

Есть файл с деплоем:

```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  selector:
    matchLabels:
      app: redis
  replicas: 1
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: master
        image: bitnami/redis
        env:
         - name: REDIS_PASSWORD
           value: password123
        ports:
        - containerPort: 6379
```

------

Измените файл так, чтобы:

- redis запускался без пароля;
- создайте сервис, который будет направлять трафик на этот деплоймент;
- версия образа redis была зафиксирована на 6.0.13.

Запустите деплоймент в своем кластере и добейтесь его стабильной работы.

  *Приведите ответ в виде получившегося файла.*

```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  selector:
    matchLabels:
      app: redis
  replicas: 1
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: master
        image: bitnami/redis:6.0.13
        env:
         - name: ALLOW_EMPTY_PASSWORD
           value: "yes"
        ports:
         - containerPort: 6379
```
```
---
apiVersion: v1
kind: Service
metadata:
  name: redis
spec:
  selector:
    app: redis
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379

```
Работающие pod и service:

<a href="https://ibb.co/HGWpDXW"><img src="https://i.ibb.co/RQx4p7x/2-1.png" alt="2-1" border="0"></a>

Описание service:

<a href="https://ibb.co/njWBQ7R"><img src="https://i.ibb.co/pdmQ4fn/2-2.png" alt="2-2" border="0"></a>

Доступ к pod'у redis извне:

<a href="https://ibb.co/qypLFYm"><img src="https://i.ibb.co/JBHWFjr/2-3.png" alt="2-3" border="0"></a>

------
### Задание 3.
Напишите команды kubectl для контейнера из предыдущего задания:
- выполнения команды ps aux внутри контейнера;
- просмотра логов контейнера за последние 5 минут;
- удаления контейнера;
- проброса порта локальной машины в контейнер для отладки.

*Приведите ответ в виде получившихся команд.*

sudo kubectl exec -it redis-79c8d44f9c-hwnsp -- ps aux

sudo kubectl logs --since=5m redis-79c8d44f9c-hwnsp

sudo kubectl delete -f redis.yaml && sudo kubectl delete -f redis_service.yaml

sudo kubectl port-forward pod/redis-79c8d44f9c-hwnsp 54321:6379

<a href="https://ibb.co/c8VDWNP"><img src="https://i.ibb.co/qgVkHRz/3-1.png" alt="3-1" border="0"></a>

------
## Дополнительные задания (со звездочкой*)

Эти задания дополнительные (не обязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. Вы можете их выполнить, если хотите глубже и/или шире разобраться в материале.

### Задание 4*.
Есть конфигурация nginx
```
location / {
    add_header Content-Type text/plain;
    return 200 'Hello from k8s';
}
```
Напишите yaml файлы для развертки nginx в которых будут присутствовать:
- ConfigMap с конфигом nginx;
- Deployment который бы подключал этот configmap;
- Ingress который будет направлять запросы по префиксу /test на наш сервис.

*Приведите ответ в виде получившегося файла.*
