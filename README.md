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
