# ДЗ 1
	Установлен Minikube, kubectl, k9s. dashboard. Настроено автодополнение. Minikube работает.
	Minikube проверен на отказоустойчивость. Pod в namespace --kube-system восстановились после удаления. Кластер кубера управляет core-dns через ReplicaSet. Сам Kubelet восстанавливает pod, т.к. он отвечает за выполнение POD на узле.
	
	Описан Dockerfile, который запускает web-сервер на порту 8000.
	Собираем образ:
		docker build -t nginx:01
	Для запуска контейнера используем команду:
		docker run -d -p 8000:80 --name nginx1 nginx:01
	URL http://localhost:8000/homework.html доступен.
	Описал манифест web-pod.yaml
	В манифест web-pod.yaml добавил описание init-контейнер
	В манифест web-pod.yaml добавил описние volume
