#docker build -t blue-webapp:v1 -f Dockerfile ./blue

E:\source_files\learn-k8s\k8s-deployment-strategies\blue-green-deployment>docker build -t blue-webapp:v1 -f Dockerfile ./blue
[+] Building 4.0s (7/7) FINISHED       


#docker build -t green-webapp:v2 -f Dockerfile ./green

E:\source_files\learn-k8s\k8s-deployment-strategies\blue-green-deployment>docker build -t green-webapp:v2 -f Dockerfile ./green
[+] Building 0.3s (7/7) FINISHED    


#kubectl apply -f k8s/blue-deployment.yaml

E:\source_files\learn-k8s\k8s-deployment-strategies>kubectl apply -f k8s/blue-deployment.yaml
deployment.apps/blue-deployment created

#kubectl apply -f k8s/green-deployment.yaml

E:\source_files\learn-k8s\k8s-deployment-strategies>kubectl apply -f k8s/green-deployment.yaml
deployment.apps/green-deployment created


#kubectl apply -f k8s/service.yaml

E:\source_files\learn-k8s\k8s-deployment-strategies>kubectl apply -f k8s/service.yaml
service/webapp-service created

#kubectl get pods -l app=webapp,version=green

E:\source_files\learn-k8s\k8s-deployment-strategies>kubectl get pods -l app=webapp,version=green
NAME                                READY   STATUS    RESTARTS   AGE
green-deployment-848fdd574b-j6mq7   1/1     Running   0          45s

#kubectl get pods -l app=webapp,version=blue

E:\source_files\learn-k8s\k8s-deployment-strategies>kubectl get pods -l app=webapp,version=blue 
NAME                               READY   STATUS    RESTARTS   AGE
blue-deployment-5d855df86f-pb4h9   1/1     Running   0          60s

#kubectl describe pod <green-deployment-pod-name>

#minikube service webapp-service — url


E:\source_files\learn-k8s\k8s-deployment-strategies>kubectl get services
NAME             TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes       ClusterIP      10.96.0.1       <none>        443/TCP        4h55m
webapp-service   LoadBalancer   10.97.145.111   localhost     80:30706/TCP   4m40s


C:\Windows\System32>minikube start --driver=hyperv  


E:\source_files\learn-k8s\k8s-deployment-strategies>kubectl get services
NAME             TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes       ClusterIP      10.96.0.1      <none>        443/TCP        15m
webapp-service   LoadBalancer   10.96.74.201   localhost     80:32066/TCP   5m30s

E:\source_files\learn-k8s\k8s-deployment-strategies>kubectl delete  services webapp-service 
service "webapp-service" deleted

E:\source_files\learn-k8s\k8s-deployment-strategies>kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
blue-deployment    1/1     1            1           10m
green-deployment   1/1     1            1           10m

E:\source_files\learn-k8s\k8s-deployment-strategies>kubectl delete deployments blue-deployment   
deployment.apps "blue-deployment" deleted

E:\source_files\learn-k8s\k8s-deployment-strategies>kubectl delete deployments green-deployment 
deployment.apps "green-deployment" deleted