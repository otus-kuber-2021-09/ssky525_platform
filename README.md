# ДЗ 1
	Установлен Minikube, kubectl, k9s. dashboard. Настроено автодополнение. Minikube работает.
	Minikube проверен на отказоустойчивость.
	Причина почему восстановились PODs:
	Controller manager выполняет функции кластерного уровня. Одна из его функций следить за работой системных POD-ов. В случае удаления одного из них (core-dns), controller manager его восстановит.
	Static Pod управляется kubelet. Kubelet всегда следит за статическим подом и перезапускает его в случае сбоя. Манифесты для статических подов располагаются по пути /etc/kubernetes/manifests/ 	
	Описан Dockerfile, который запускает web-сервер на порту 8000.
	Собираем образ:
		docker build -t nginx:02
	Для запуска контейнера используем команду:
		docker run -d -p 8000:8000 --name nginx1 nginx:02
	URL http://localhost:8000/homework.html доступен.
	Dockerfile закинул на dockerhub
	Описал манифест web-pod.yaml
	В манифест web-pod.yaml добавил описание init-контейнер
	В манифест web-pod.yaml добавил описние volume
	После запуска команды kubectl port-forward --address 0.0.0.0 pod/web 8000:8000 nginx доступен по адресу: http://localhost:8000
	Собрал образ для frontend и закинул его на dockerhub
	При запуске пода frontend возникает ошибка. По логам пода видим что в манифесте не указаны переменные окружения.
	Создал новый манифест frontend-pod-healthy.yaml и указал в нем переменные:
	  env:
          - name: PORT
            value: "8080"
          - name: PRODUCT_CATALOG_SERVICE_ADDR
            value: "productcatalogservice:3550"
          - name: CURRENCY_SERVICE_ADDR
            value: "currencyservice:7000"
          - name: CART_SERVICE_ADDR
            value: "cartservice:7070"
          - name: RECOMMENDATION_SERVICE_ADDR
            value: "recommendationservice:8080"
          - name: SHIPPING_SERVICE_ADDR
            value: "shippingservice:50051"
          - name: CHECKOUT_SERVICE_ADDR
            value: "checkoutservice:5050"
          - name: AD_SERVICE_ADDR
            value: "adservice:9555"
	POD frontend находится в статусе running.

# ДЗ 2
1) Установил go, kind
2) Создал ветку kubernetes-controllers
3) Создал файл kind-config.yaml
4) Запустил создание кластера
kind create cluster --config kind-config.yaml
5) Создал манифест frontend-replicaset.yaml
Необходимо добавить selector
spec.selector.matchLabels.app: frontend
6) Изменил количество реплик
kubectl scale replicaset frontend --replicas=3
7) После ручного удаления POD-ов, PODs восстановились
8) Применил снова манифест frontend-replicaset.yaml, количество реплик уменьшилось
9) В манифесте поменял количество реплик на 3
10) На DockerHub выложил версию образа с новым тегом ssky525/otus:frontend.v0.0.2
11) Проверил образ, который указан в ReplicaSet (ssky525/otus:frontend.v0.0.2) и образ из которого сейчас запущены поды (ssky525/otus:frontend).
12) ReplicaSet следит, чтобы количество подов соответствовало заданому количеству, при этом не проверяет соответствие запущенных подов шаблону.
13) Собрал образ paymentservice. Навесил теги v0.0.1 и v0.0.2
14) Написал манифест paymentservice-replicaset.yaml, который создает 3 реплики из образа ssky525/otus:frontend.v0.0.1
15) Собрал образ paymentservice с двумя тегам:v0.0.1 и v0.0.2. Залил образы на dockerhub: ssky525/paymentservice:v0.0.1 и ssky525/paymentservice:v0.0.2
16) Написал манифест paymentservice-replicaset.yaml с тремя репликами, разворачивающими из образа версии v0.0.1.
17) Написал deployment манифест paymentservice-deployment.yaml с тремя репликами, разворачивающими из образа версии v0.0.1.
18) Обновил deployment манифест paymentservice-deployment.yaml на версию v0.0.2. Создались новые поды с версией образа v0.0.2. Создано две ReplicaSet: Одна управляет тремя репликами pod с образом v0.0.2, другая реплика управляет нулем реплик pod с версией v0.0.1.
19) Сделал откат deployment на версию 1
20) Написал paymentservice-deployment-bg.yaml со стратегией обновления blue\green: maxSurge: 3 maxUnavailable: 0
21) Написал paymentservice-deployment-reverse.yaml со стратегией обновления Reverse Rolling Update: maxSurge: 0 maxUnavailable: 1
22) Создал манифест frontend-deployment.yaml из которого можно развернуть три реплики pod с тегом образа v0.0.1. Добавил описание readnessProbe.
23) Создал манифест node-exporter-daemonset.yaml для развертывания DaemonSet с Node Exporter. Выполнил kubectl port-forward node-exporter-xqr56 9100:9100. curl localhost:9100/metrics отдает метрики. Для того, чтобы pod управляемый DaemonSet развернулся на master ноде, нужно добавить 
      tolerations:
      - operator: Exists
