# ДЗ 1
	Установлен Minikube, kubectl, k9s. dashboard. Настроено автодополнение. Minikube работает.
	Описан Dockerfile, который запускает web-сервер на порту 8000.
	Собираем образ:
		docker build -t nginx:01
	Для запуска контейнера используем команду:
		docker run -d -p 8000:80 --name nginx1 nginx:01
	URL http://localhost:8000/homework.html доступен.
