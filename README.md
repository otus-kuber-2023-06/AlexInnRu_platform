# AlexInnRu_platform
AlexInnRu Platform repository

#Механика запуска и взаимодействия контейнеров в Kubernetes // ДЗ 
Руководствуясь материалами лекции опишите произошедшую ситуацию, почему обновление ReplicaSet не повлекло обновление запущенных pod?
 Потому что указано кол-во реплик 1 и оно поддерживается на этом уровне. У нас работает одна реплика, поэтому обнеовление версии image не повлекло создание нового пода.
 Для проверки поменяем манифест frontend-replicaset.yaml и укажем новую версию Image
 Применим манифест  kubectl apply -f frontend-replicaset.yaml
 Ничего не произошло
 Поменяем в манифесте реплик до 2.
 Применим манифест
 Новый созданный под будет собран из новой версии image, при этом старый под также будет работать, но при его удалении он создастся с уже новой версией.


DaemonSet | Задание со  

Применили манифест nodeexporter-daemonset.yaml

Развернуто 3 пода node-exporter на рабочих нодах кластера.

Проверим с помощью kubectl get pods -n kube-system -o wide 
node-exporter-867zw                           1/1     Running   0              46s    10.244.4.12   kind-worker3          
node-exporter-884q5                           1/1     Running   0              46s    10.244.5.11   kind-worker2          
node-exporter-hsbhb                           1/1     Running   0              46s    10.244.3.11   kind-worker


DaemonSet  

Чтобы поды применились к мастер нодам, необходимо добавить параметр tolerations. Т.к. изначально на мастер ноды назначен taint node-role.kubernetes.io/control-plane:NoSchedule

tolerations:      
      - key: node-role.kubernetes.io/control-plane
        operator: Exists
        effect: NoSchedule
      - key: node-role.kubernetes.io/master
        operator: Exists
        effect: NoSchedule

Применяем манифест
kubectl apply -f node-exporter-daemonset.yaml

Видим поды
node-exporter-fqh7z                           1/1     Running   0              96s    10.244.2.2    kind-control-plane3   
node-exporter-jkjw2                           1/1     Running   0              81s    10.244.4.13   kind-worker3          
node-exporter-mz6x4                           1/1     Running   0              96s    10.244.0.5    kind-control-plane    
node-exporter-r58sj                           1/1     Running   0              96s    10.244.1.2    kind-control-plane2   
node-exporter-rcj2z                           1/1     Running   0              84s    10.244.5.12   kind-worker2          
node-exporter-vcmbr                           1/1     Running   0              78s    10.244.3.12   kind-worker

