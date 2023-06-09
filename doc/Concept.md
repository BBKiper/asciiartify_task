## Тиждень 4. Модуль 4. Задача 4. Завдання 1.
Після інсталювання 3х інструментів на базі k8s (minikube, kind та k3d) зупинився на minikube.

Після встановлення всіх необхідних пакетів запускаємо наш сервіс з ініціалізацією кластеру

> minikube start

![enter image description here](https://i.imgur.com/Fp0d9oR.png)
Перевірити статус нашого кластеру можемо виконавши команду
> minikube status

вивід має бути приблизно таким:

    minikube
    type: Control Plane
    host: Running
    kubelet: Running
    apiserver: Running
    kubeconfig: Configured

Створимо новий деплоймент у нашому кластері

> kubectl create deployment test-project --image=k8s.gcr.io/echoserver:1.4

де `test-project` ім'я нашого деплойменту, а ключ `--image=` вказує, який образ ми будемо використовувати
Щоб зробити наш об'єкт деплойменту доступним, виконаємо команду

> kubectl expose deployment test-project --type=NodePort --port=8080

Де `--type=` вказує тип сервісу, а `--port=` його порт.
Перевіримо успішність запуску та url за яким доступний наш проект

> kubectl get pod 
> minikube service test-project

![enter image description here](https://i.imgur.com/hxBsgIu.png)