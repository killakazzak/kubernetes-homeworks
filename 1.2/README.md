# Домашнее задание к занятию «Базовые объекты K8S»

### Цель задания

В тестовой среде для работы с Kubernetes, установленной в предыдущем ДЗ, необходимо развернуть Pod с приложением и подключиться к нему со своего локального компьютера. 

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключенным Git-репозиторием.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. Описание [Pod](https://kubernetes.io/docs/concepts/workloads/pods/) и примеры манифестов.
2. Описание [Service](https://kubernetes.io/docs/concepts/services-networking/service/).

------

### Задание 1. Создать Pod с именем hello-world

1. Создать манифест (yaml-конфигурацию) Pod.
2. Использовать image - gcr.io/kubernetes-e2e-test-images/echoserver:2.2.
3. Подключиться локально к Pod с помощью `kubectl port-forward` и вывести значение (curl или в браузере).

hello-world-pod.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world
spec:
  containers:
  - name: echoserver
    image: gcr.io/kubernetes-e2e-test-images/echoserver:2.2
    ports:
    - containerPort: 8080
```

```bash
microk8s kubectl apply -f hello-world-pod.yaml
microk8s kubectl get pods
```
![image](https://github.com/user-attachments/assets/c146bd5b-61c1-436d-9432-c7ef99f0048e)

```bash
microk8s kubectl port-forward pod/hello-world 8080:8080
```
![image](https://github.com/user-attachments/assets/0eee5eec-a439-46d6-9ede-12f37b1573ed)

```
curl http://localhost:8080
```
![image](https://github.com/user-attachments/assets/faa3ce14-0c06-4623-a4c6-5518961ec1b3)

------

### Задание 2. Создать Service и подключить его к Pod

1. Создать Pod с именем netology-web.
2. Использовать image — gcr.io/kubernetes-e2e-test-images/echoserver:2.2.
3. Создать Service с именем netology-svc и подключить к netology-web.
4. Подключиться локально к Service с помощью `kubectl port-forward` и вывести значение (curl или в браузере).

netology-web-pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: netology-web
spec:
  containers:
  - name: echoserver
    image: gcr.io/kubernetes-e2e-test-images/echoserver:2.2
    ports:
    - containerPort: 8080
```
netology-svc.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: netology-svc
spec:
  selector:
    app: netology-web
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```

```bash
microk8s kubectl apply -f netology-web-pod.yaml
microk8s kubectl apply -f netology-svc.yaml
```
![image](https://github.com/user-attachments/assets/f743f653-1a5e-44b1-a3e4-0a3b70d93942)

```bash
microk8s kubectl get pods
microk8s kubectl get services
```
![image](https://github.com/user-attachments/assets/ba157836-cd75-4b1a-8fc5-e97d6f589640)

```bash
microk8s kubectl port-forward service/netology-svc 8080:80
```
```bash
curl http://localhost:8080
```


------

### Правила приёма работы

1. Домашняя работа оформляется в своем Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода команд `kubectl get pods`, а также скриншот результата подключения.
3. Репозиторий должен содержать файлы манифестов и ссылки на них в файле README.md.

------

### Критерии оценки
Зачёт — выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку — задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
