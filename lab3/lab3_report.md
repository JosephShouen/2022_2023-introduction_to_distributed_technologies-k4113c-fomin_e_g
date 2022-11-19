University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2022/2023  
Group: K4113c  
Author: Fomin Eugene Georgievich  
Lab: Lab3  
Date of create: 24.10.2022  
Date of finished:  

# 1. Создадим Configmap командой
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab3/1.png)

# 2. Создадим Replicaset посредством yaml файла
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab3/2.png)
''' yaml '''
apiVersion: apps/v1  
kind: ReplicaSet  
metadata:  
    name: ifilyaninitmo-replicaset  
    labels:  
        app: ifilyaninitmo-replicaset  
spec:  
    replicas: 2  
    selector:  
        matchLabels:  
            app: ifilyaninitmo-replicaset  
    template:  
        metadata:  
            labels:  
                app: ifilyaninitmo-replicaset  
        spec:  
            containers:  
            - name: ifilyaninitmo-replicaset  
              image: ifilyaninitmo/itdt-contained-frontend:master  
              ports:  
              - containerPort: 3000  
              env:  
                - name: REACT_APP_USERNAME  
                  valueFrom:  
                    configMapKeyRef:  
                      name: ifilyaninitmo  
                      key: REACT_APP_USERNAME  
                - name: REACT_APP_COMPANY_NAME  
                  valueFrom:  
                    configMapKeyRef:  
                      name: ifilyaninitmo  
                      key: REACT_APP_COMPANY_NAME  
''' yaml '''

# 3. Включим minikube addons enable ingress, сгенерируем TLS сертификат, импортировать сертификат в minikube. 
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab3/3.png)

![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab3/4.png)

# 4. Создадим ingress в minikube посредством yaml файла, где указан ранее импортированный сертификат, FQDN и имя сервиса.
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab3/5.png)

apiVersion: networking.k8s.io/v1  
kind: Ingress  
metadata:  
  name: ifilyaninitmo-ingress  
spec:  
  tls:  
  - hosts:  
      - sleepingjoshua.com  
    secretName: ifilyaninitmo-secret  
  rules:  
  - host: sleepingjoshua.com  
    http:  
      paths:  
      - path: /  
        pathType: Prefix  
        backend:  
          service:  
            name: ifilyaninitmo-replicaset  
            port:  
              number: 3000  

![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab3/6.png)

# 5. Зайдем в файл hosts, пропишем FQDN и IP адрес ingress

![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab3/8.png)

# 6. После этих действий не заходит на сайт. Исправим ситуацию с помощью minikube tunnel.

![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab3/10.png)

![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab3/11.png)

![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab3/12.png)

Все работает, ура
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab3/9.png)

![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab3/13.png)

