# Пример технической документации
Александра Тимощенко
Контакт: alekslajt658@gmail.com | @Andra_Tim
## О документе

Данный документ — пример моей работы над тестовым заданием для позиции технического писателя. Он включает:
- Анализ исходных данных (выводы команд `kubectl`)
- Полное руководство для разработчиков по работе с Kubernetes-кластером
- Структурированную документацию с инструкциями, примерами команд и разделом по отладке
## Контекст

Представьте, что вы работаете техничесĸим писателем в ĸомпании, ĸоторая разрабатывает платформу для запусĸа приложений в Kubernetes. Ваша задача — написать доĸументацию для разработчиĸов, ĸоторые будут разворачивать свои приложения на платформе.

Задание

Используя результаты ĸоманд kubectl ниже, напишите доĸументацию, ĸоторая вĸлючает:

1. Обзор ĸластера — ĸратĸое описание струĸтуры ĸластера

2. Инструĸцию по работе с namespaces — ĸаĸ просмотреть, создать и переĸлючиться между namespaces

3. Описание деплойментов — ĸаĸие приложения запущены, их статус

4. Инструĸцию по проверĸе статуса подов — ĸаĸ посмотреть поды и их состояние

5. Инструĸцию по просмотру логов — ĸаĸ посмотреть логи приложения

Результаты ĸоманд kubectl
```
Kubernetes control plane is running at https://cluster.example.com:6443
CoreDNS is running at https://cluster.example.com:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```
Команда: kubectl get namespaces
```
NAME              STATUS   AGE
default           Active   365d
kube-system       Active   365d
kube-public       Active   365d
kube-node-lease   Active   365d
development       Active   180d
staging           Active   180d
production        Active   180d
```
Команда: kubectl get deployments -n development
```
NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
auth-service         2/2     2            2           45d
api-gateway          3/3     3            3           90d
user-service         2/2     2            2           60d
payment-service      1/1     1            1           30d
notification-service 1/1     1            1           15d
```
Команда: kubectl get pods -n development
```
NAME                                  READY   STATUS    RESTARTS   AGE
auth-service-5f8d9c7b4d-x7k2m         1/1     Running   0          2d
auth-service-5f8d9c7b4d-p9n3q         1/1     Running   0          2d
api-gateway-6c7b8d9f5c-m4k8p          1/1     Running   0          5d
api-gateway-6c7b8d9f5c-n2w7r          1/1     Running   0          5d
api-gateway-6c7b8d9f5c-q1t5s          1/1     Running   0          5d
user-service-7d8e9f0a6d-k3j6h         1/1     Running   0          1d
user-service-7d8e9f0a6d-l5m9n         1/1     Running   0          1d
payment-service-8e9f0a1b7e-o2p4q      1/1     Running   0          12h
notification-service-9f0a1b2c8f-r3s5t 1/1     Running   0          6h
```
Команда: kubectl get services -n development
```
NAME                 TYPE        CLUSTER-IP      PORT(S)    AGE
auth-service         ClusterIP   10.96.100.10    8080/TCP   45d
api-gateway          ClusterIP   10.96.100.20    8080/TCP   90d
user-service         ClusterIP   10.96.100.30    8080/TCP   60d
payment-service      ClusterIP   10.96.100.40    8080/TCP   30d
notification-service ClusterIP   10.96.100.50    8080/TCP   15d
```
Команда: kubectl logs auth-service-5f8d9c7b4d-x7k2m -n development --tail=20
```
2024-01-15 10:23:45 INFO [main] Starting AuthService application
2024-01-15 10:23:46 INFO [main] Loading configuration from environment
2024-01-15 10:23:47 INFO [main] Connecting to database at postgres://db:5432/auth
2024-01-15 10:23:48 INFO [main] Database connection established 
2024-01-15 10:23:49 INFO [main] Starting HTTP server on port 8080
2024-01-15 10:23:50 INFO [main] AuthService started successfully
2024-01-15 10:25:12 INFO [http-handler] POST /api/v1/login - 200 OK - 45ms
2024-01-15 10:25:45 INFO [http-handler] POST /api/v1/login - 200 OK - 38ms
2024-01-15 10:26:01 INFO [http-handler] POST /api/v1/refresh - 200 OK - 12ms
2024-01-15 10:26:33 INFO [http-handler] POST /api/v1/login - 401 Unauthorized - 5ms
2024-01-15 10:27:15 INFO [http-handler] POST /api/v1/login - 200 OK - 42ms
2024-01-15 10:28:00 INFO [http-handler] POST /api/v1/logout - 200 OK - 3ms
2024-01-15 10:28:45 INFO [http-handler] POST /api/v1/login - 200 OK - 51ms
2024-01-15 10:29:12 INFO [http-handler] POST /api/v1/refresh - 200 OK - 15ms
2024-01-15 10:29:55 INFO [http-handler] POST /api/v1/login - 200 OK - 39ms
2024-01-15 10:30:22 INFO [http-handler] POST /api/v1/login - 401 Unauthorized - 4ms
2024-01-15 10:31:00 INFO [http-handler] POST /api/v1/login - 200 OK - 44ms
2024-01-15 10:31:30 INFO [http-handler] POST /api/v1/logout - 200 OK - 2ms
2024-01-15 10:32:15 INFO [http-handler] POST /api/v1/login - 200 OK - 47ms
```

## Документация Kubernetes.

Данное руководство предназначено для разработчиков, разворачивающих и обслуживающих свои приложения на платформе Kubernetes.

Kubernetes — это портативная, расширяемая платформа с открытым исходным кодом для управления контейнеризированными рабочими нагрузками и сервисами, которая упрощает как декларативную конфигурацию, так и автоматизацию. 
Более детальную информацию можно почитать на [официальном сайте](https://kubernetes.io/docs/home/)
### 1. Обзор ĸластера

Данный кластер — это управляемая среда для запуска контейнеризированных приложений. Он автоматически следит за состоянием сервисов, перезапускает упавшие компоненты и распределяет нагрузку.

Основные параметры кластера:
- *Адрес API-сервера:* `https://cluster.example.com:6443` (через него вы управляете кластером)
- *DNS-сервер внутри кластера:* CoreDNS (обеспечивает связь между сервисами по именам)
- *Структура:* Кластер разделен на логические пространства — *namespaces* (подробнее в разделе 2)

Для диагностики проблем используйте команду:
```
kubectl cluster-info dump
```
### 2. Инструĸция по работе с namespaces 

**Namespace** — это способ изолировать разные окружения и команды друг от друга.

 Текущие namespaces в кластере:
	 - **`default`** — для ресурсов без указанного namespace (статус: Active, возраст: 365d)
	- **`kube-system`** — системные компоненты Kubernetes (статус: Active, возраст: 365d)
	- **`kube-public`** — для общедоступных ресурсов (статус: Active, возраст: 365d)
	- **`kube-node-lease`** — служебный для узлов кластера (статус: Active, возраст: 365d)
	- **`development`** — среда разработки (статус: Active, возраст: 180d)
	- **`staging`** — предпродакшен среда (статус: Active, возраст: 180d)
	- **`production`** — продакшен среда (статус: Active, возраст: 180d)

Основные команды:
1) Вывести список текущих пространств имен в кластере можно с помощью следующей команды:
```
kubectl get namespace
```
2) Команда создания нового namespace:
```
kubectl create namespace <insert-namespace-name-here>
```
3) Команда удаления  namespace:
```
kubectl delete namespaces <insert-some-namespace-name>
```
4) Переключиться для постоянной работы
```
kubectl config set-context --current --namespace=development
```
После этой команды все следующие операции будут по умолчанию выполняться в `development`.
5) Выполнить разовую команду в другом namespace
```
kubectl get pods -n production
```
Флаг `-n` (или `--namespace`) указывает, в каком пространстве выполнить команду.

Больше про команды namespace можно почитать в [Share a Cluster with Namespaces](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/#creating-a-new-namespace)
### 3. Описание деплойментов

В окружении `development` запущены микросервисы нашей платформы.
Список приложений:
 1) auth-service`
    - Статус:  **OK**
    - Количество реплик: 2 из 2   
    - Возраст: 45 дней    
    - Назначение: аутентификация, выдача JWT-токенов
 2) api-gateway
    - Статус:  **OK**
    - Количество реплик: 3 из 3
    - Возраст: 90 дней
    - Назначение: входная точка для всех внешних запросов (маршрутизация)
 3) user-service
    - Статус:  **OK**
    - Количество реплик: 2 из 2
    - Возраст: 60 дней
    - Назначение: управление пользователями, профилями и ролями
 4) payment-service
    - Статус:  **OK**
    - Количество реплик: 1 из 1
    - Возраст: 30 дней
    - Назначение: обработка платежей       
 5) notification-service
    - Статус:  **OK**
    - Количество реплик: 1 из 1
    - Возраст: 15 дней
    - Назначение: отправка email и уведомлений

Пояснения к статусам:
- `READY` — сколько подов работает / сколько должно работать (например, 2/2 значит «оба работают»)
- `UP-TO-DATE` — сколько подов обновлено до последней версии
- `AVAILABLE` — сколько подов доступны пользователям
### 4. Инструĸция по проверĸе статуса подов

Чтобы увидеть, какие конкретно поды (экземпляры) запущены для каждого сервиса, используйте команду:
```
kubectl get pods -n development
```
Расшифровка колонок из команды `kubectl get pods`:
- `READY` — сколько контейнеров в поде работают / сколько всего (обычно `1/1` — значит, всё хорошо)
- `STATUS` — `Running` (работает), `Pending` (ждет запуска), `CrashLoopBackOff` (падает при старте)
- `RESTARTS` — количество перезапусков (если число большое — есть проблема)
- `AGE` — время жизни пода
Больше о других командах [Официальная документация: kubectl get](https://kubernetes.io/docs/reference/kubectl/generated/kubectl_get/).
### 5. Инструĸция по просмотру логов

Чтобы вывести логи для контейнера в поде или указанном ресурсе, используйте команду:
```
kubectl logs [-f] [-p] (POD | TYPE/NAME) [-c CONTAINER]
```
Все необходимые дополнительные параметры команды можно посмотреть на [официальном сайте](https://kubernetes.io/docs/reference/kubectl/generated/kubectl_logs/)
Например, посмотреть последние 20 строк лога
Команда: `kubectl logs auth-service-5f8d9c7b4d-x7k2m -n development --tail=20`
```
2024-01-15 10:23:45 INFO [main] Starting AuthService application
2024-01-15 10:23:46 INFO [main] Loading configuration from environment
2024-01-15 10:23:47 INFO [main] Connecting to database at
postgres://db:5432/auth
2024-01-15 10:23:48 INFO [main] Database connection established
2024-01-15 10:23:49 INFO [main] Starting HTTP server on port 8080
2024-01-15 10:23:50 INFO [main] AuthService started successfully
2024-01-15 10:25:12 INFO [http-handler] POST /api/v1/login - 200 OK - 45ms
2024-01-15 10:25:45 INFO [http-handler] POST /api/v1/login - 200 OK - 38ms
2024-01-15 10:26:01 INFO [http-handler] POST /api/v1/refresh - 200 OK - 12ms
2024-01-15 10:26:33 INFO [http-handler] POST /api/v1/login - 401 Unauthorized - 5ms
2024-01-15 10:27:15 INFO [http-handler] POST /api/v1/login - 200 OK - 42ms
2024-01-15 10:28:00 INFO [http-handler] POST /api/v1/logout - 200 OK - 3ms
2024-01-15 10:28:45 INFO [http-handler] POST /api/v1/login - 200 OK - 51ms
2024-01-15 10:29:12 INFO [http-handler] POST /api/v1/refresh - 200 OK - 15ms
2024-01-15 10:29:55 INFO [http-handler] POST /api/v1/login - 200 OK - 39ms
2024-01-15 10:30:22 INFO [http-handler] POST /api/v1/login - 401 Unauthorized - 4ms
2024-01-15 10:31:00 INFO [http-handler] POST /api/v1/login - 200 OK - 44ms
2024-01-15 10:31:30 INFO [http-handler] POST /api/v1/logout - 200 OK - 2ms
2024-01-15 10:32:15 INFO [http-handler] POST /api/v1/login - 200 OK - 47ms
```
### 6. Частые проблемы и их решения

#### 6.1  Под не запускается (статус `Pending`)

*Симптом:* Под висит в статусе `Pending` и не переходит в `Running`.

*Как диагностировать:*
```
kubectl describe pod ${POD_NAME} -n development

```
В секции `Events` внизу вывода будет указана причина.

*Что делать:*

| Причина                                                                       | Решение                                                                         |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Не хватает ресурсов (ошибки Insufficient memory или Insufficient cpu) | Уменьшите запросы ресурсов в манифесте или обратитесь к администратору кластера |
| Не может скачать образ (ошибка Failed to pull image)                    | Проверьте правильность имени образа и его доступность в registry                |

#### 6.2 Под постоянно перезапускается (статус `CrashLoopBackOff`)

*Симптом:* Под запускается, но сразу падает, Kubernetes перезапускает его, и это повторяется.
*Как диагностировать:*
1) Посмотреть логи текущего пода
```
 kubectl logs ${POD_NAME} -n development
```
2) Если под уже перезапустился — логи предыдущей версии
```
kubectl logs ${POD_NAME} -n development --previous
```

*Типичные причины:*
- Ошибка в коде приложения (смотрите стектрейс в логах)
- Не может подключиться к базе данных (проверьте переменные окружения)
- Не хватает памяти (в `describe` будет `OOMKilled`)
#### 6.3 Не могу достучаться до сервиса
*Симптом:* Запросы к сервису не проходят, хотя поды работают.
*Как диагностировать:*
1. Проверить, есть ли у сервиса эндпоинты:
```
kubectl get endpoints -n development
```
Если в колонке `ENDPOINTS` пусто — сервис не видит поды.
2. Проверьте, совпадают ли метки:
	 2.1 Посмотрите метки у подов
    ```
    kubectl get pods -n development --show-labels
    ```
    2.2 Посмотрите селектор в сервисе
```
    kubectl describe service <имя-сервиса> -n development
```
Метки на подах должны совпадать с селектором в сервисе.
3. Проверьте, на том ли порту слушает приложение:
```
  kubectl logs <имя-пода> -n development | grep "listening on"
```
Порт в контейнере должен совпадать с `targetPort` в сервисе.
*Что делать:*
- Если метки не совпадают — исправьте их в манифесте пода или сервиса
- Если порты не совпадают — приведите их к одному значению
#### 6.4 После обновления пода ничего не изменилось
*Симптом:* Вы обновили манифест и применили его, но под продолжает работать со старой версией.
*Причина:* Вы обновили сам под, а нужно обновлять Deployment. Поды, созданные через Deployment, не обновляются напрямую — их пересоздаёт Deployment при изменении шаблона.
*Что делать:*
1. Убедитесь, что вы меняете манифест Deployment, а не пода
2. После изменения Deployment выполните:
```
kubectl rollout status deployment/<имя-деплоймента> -n development
```
3. Если нужно перезапустить поды без изменения образа:
```
kubectl rollout restart deployment/<имя-деплоймента> -n development
```
Больше о проблемах можно посмотреть в [Debug Pods](https://kubernetes.io/docs/tasks/debug/debug-application/debug-pods/#debugging-pods)

**Подготовлено:** Александра  
**Дата:** март 2026
