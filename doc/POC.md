## Тиждень 4. Модуль 4. Задача 5. Завдання 2.
*Виконання передбачає вже інстальованний сервіс k8s, в моєму випадку це minicube*

Для розгортання ArgoCD створимо однойменний неймспейс у нашому кластері та запустимо розгортання з готового маніфесту, вказавши шлях до нього. Після чого перевіряємо статус наших подів.

> kubectl create namespace argocd 
> kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
> kubectl get pods -n argocd

Ключ `-n` вказує на назву неймспейсу, а `-f` на файл, а саме його url
![enter image description here](https://i.imgur.com/dgMvJU0.png)

Після вдалого запуску усіх подів змінимо тип сервісу **argocd-server** з ClusterIP на LoadBalancer, щоб відкрити до нього доступ ззовні

> kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

Також, нам потрібно згенерувати пароль для доступу у webui

> kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

Вивід з результатом зберігаємо

Далі запустимо форвардінг портів з нашого кластеру на хост машину

> kubectl port-forward svc/argocd-server -n argocd 8080:443

Де 8080 порт на хості, а 443 порт сервісу
Для того щоб відкрити доступ ззовні, додамо до команди ключ `--address=` і символом `&` для запуску процессу у фоні

> kubectl port-forward svc/argocd-server -n argocd 8080:443 --address="0.0.0.0"
![enter image description here](https://i.imgur.com/e9IvYbZ.png)

Переходимо по адресі `ip_address:8080` і логінимось як користувач admin з раніше отриманим паролем
![enter image description here](https://i.imgur.com/VBD0fbt.png)
![enter image description here](https://i.imgur.com/r82M5JV.png)