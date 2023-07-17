# AlexInnRu_platform
AlexInnRu Platform repository

1.Создан dockerfile контейнера web
2. Создан минимальный конфиг запуска nginx.conf
3. Собран образ из dockerfile и выложен на Docker Hub
   docker build -t alexinnru/otus_k8s:centos_nginx
   docker push alexinnru/otus_k8s:centos_nginx
4. Создан манифест web-pod.yaml и развернут под.
   kubectl apply -f web-pod.yaml
5. В манифест web-pod.yaml добавлен контейнер init и прописаны volumes
6. Создан образ сервиса frontend
   docker build -t alexinnru/otus_k8s:frontend -f ~/demo/microservices-demo/src/frontend/Dockerfile .
7. Выложен образ сервиса в Docker Hub 
   docker push alexinnru/otus_k8s:frontend
8. Диагностика пода frontend 
   kubectl logs frontend
   {"message":"Tracing disabled.","severity":"info","timestamp":"2023-07-17T18:09:36.44627712Z"}
{"message":"Profiling disabled.","severity":"info","timestamp":"2023-07-17T18:09:36.446482071Z"}
panic: environment variable "PRODUCT_CATALOG_SERVICE_ADDR" not set

goroutine 1 [running]:
main.mustMapEnv(0xc000114480, {0xc0758d, 0x1c})
        /src/main.go:208 +0xb9
main.main()
        /src/main.go:124 +0x5be
9. Создаем новый манифест с указанием всех переменных frontend-pod-healthy.yaml
10. Запускаем новый под из этого манифеста
    kubectl apply -f frontend-pod-healthy.yaml
   
   
