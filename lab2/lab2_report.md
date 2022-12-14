University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2022/2023  
Group: K4113c  
Author: Fomin Eugene Georgievich  
Lab: Lab2  
Date of create: 24.10.2022  
Date of finished:  

# 1. Запустим Minikube
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab2/2.png)

# 2. Создадим yaml файл для Deployment
apiVersion: apps/v1  
kind: Deployment  
metadata:  
    name: ifilyaninitmo-deployment  
    labels:  
        app: ifilyaninitmo-deployment  
spec:  
    replicas: 2  
    selector:  
        matchLabels:  
            app: ifilyaninitmo-deployment  
    template:  
        metadata:  
            labels:  
                app: ifilyaninitmo-deployment  
        spec:  
            containers:  
            - name: ifilyaninitmo-deployment  
              image: ifilyaninitmo/itdt-contained-frontend:master  
              ports:  
              - containerPort: 3000  
              env:  
                - name: REACT_APP_USERNAME  
                  value: value_1  
                - name: REACT_APP_COMPANY_NAME  
                  value: value_2  

# 3. Создадим Deployment на основе yaml файла. 
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab2/5.1.png)

# 4. Создадим сервис командной expose и прокинем порт
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab2/5.2.png)

# 5. Зайдем на localhost:3000
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab2/6.png)
Потом заново запустим сервис. Имя контейнера поменялось, имя переменных - нет
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab2/7.png)

# 6. Получим логи для обоих экземпляров подов
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab2/8.png)

# 7. Схема
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab2/%D0%94%D0%B8%D0%B0%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B0%20%D0%B1%D0%B5%D0%B7%20%D0%BD%D0%B0%D0%B7%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F.drawio.png)
