University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2022/2023  
Group: K4113c  
Author: Fomin Eugene Georgievich  
Lab: Lab4  
Date of create: 24.10.2022  
Date of finished:  

# 1. Запустим minikube с двумя нодами
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab4/1.png)
Проверим ноды и поды
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab4/2.png)

# 2. Создадим манифест для Calico 
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: calicoctl
  namespace: kube-system

---
apiVersion: v1
kind: Pod
metadata:
  name: calicoctl
  namespace: kube-system
spec:
  nodeSelector:
    kubernetes.io/os: linux
  hostNetwork: true
  serviceAccountName: calicoctl
  containers:
    - name: calicoctl
      image: calico/ctl:v3.20.0
      command:
        - /calicoctl
      args:
        - version
        - --poll=1m
      env:
        - name: DATASTORE_TYPE
          value: kubernetes

---

kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: calicoctl
rules:
  - apiGroups: [""]
    resources:
      - namespaces
      - nodes
    verbs:
      - get
      - list
      - update
  - apiGroups: [""]
    resources:
      - nodes/status
    verbs:
      - update
  - apiGroups: [""]
    resources:
      - pods
      - serviceaccounts
    verbs:
      - get
      - list
  - apiGroups: [""]
    resources:
      - pods/status
    verbs:
      - update
  - apiGroups: ["crd.projectcalico.org"]
    resources:
      - bgppeers
      - bgpconfigurations
      - clusterinformations
      - felixconfigurations
      - globalnetworkpolicies
      - globalnetworksets
      - ippools
      - ipreservations
      - kubecontrollersconfigurations
      - networkpolicies
      - networksets
      - hostendpoints
      - ipamblocks
      - blockaffinities
      - ipamhandles
      - ipamconfigs
    verbs:
      - create
      - get
      - list
      - update
      - delete
  - apiGroups: ["networking.k8s.io"]
    resources:
      - networkpolicies
    verbs:
      - get
      - list

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: calicoctl
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: calicoctl
subjects:
  - kind: ServiceAccount
    name: calicoctl
    namespace: kube-system
 ```
 ![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab4/3.png)
  ```yaml
  apiVersion: projectcalico.org/v3
kind: IPPool
metadata:
  name: zone-west-ippool
spec:
  cidr: 10.0.0.0/24
  ipipMode: Always
  natOutgoing: true
  nodeSelector: zone == "west"
---
apiVersion: projectcalico.org/v3
kind: IPPool
metadata:
  name: zone-east-ippool
spec:
  cidr: 192.168.17.0/24
  ipipMode: Always
  natOutgoing: true
  nodeSelector: zone == "east"
  ```
 ![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab4/4.png)
 
# 3. Создадим Deployment посредством yaml файла и сервер для него посредством команды
```yaml
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
                  value: sleepingjoshua
                - name: REACT_APP_COMPANY_NAME
                  value: SI_Company
```
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab4/5.png)
# 4. Зайдем в браузер 
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab4/6.png)
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab4/7.png)

# 5. Пропингуем
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab4/8.png)
# 6. Схема
![Альтернативный текст](https://github.com/JosephShouen/2022_2023-introduction_to_distributed_technologies-k4113c-fomin_e_g/blob/main/lab4/drawio.png)
