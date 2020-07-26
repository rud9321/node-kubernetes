# node-kubernetes
kubernetes with node-express docker repository


### Usage

```python
Docker run only...
docker run -d -p 21000:8080 --name app rud9321/express-app
runnig port now go to (http://localhost:21000)
Congratulations you done that
```

## what it is 
nothing much but rud9321/express-app is a repo at dockerhub just get it and run it as container 



## But I think we are here for kubernetes 
we need only deployment.yaml and service.yaml code is already there as image, if you need your own code and docker image... buddy you are smart enoght to create it by yourself just add these 2 kubernetes files and rock the code...

so just skip the upper things...  
just follow me from here


get clone this repo

here is deployment.yaml
```kubernetes
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
spec:
  selector:
    matchLabels:
      app: app
  template:
    metadata:
      labels:
        app: app
    spec:
      containers:
      - name: express-app
        image: rud9321/express-app:latest
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 8080


```
I have used here my docker image repo rud9321/express-app:latest

```python
kubectl apply -f deployment.yaml

PS C:\Users\Rudra\Desktop\kubernetes> kubectl describe deploy
Name:                   app-deployment
Namespace:              default
CreationTimestamp:      Mon, 27 Jul 2020 01:18:32 +0530
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
                        kubectl.kubernetes.io/last-applied-configuration:
                          {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"app-deployment","namespace":"default"},"spec":{"selector"...
Selector:               app=app
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=app
  Containers:
   express-app:
    Image:      rud9321/express-app:latest
    Port:       8080/TCP
    Host Port:  0/TCP
    Limits:
      cpu:        500m
      memory:     128Mi
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   app-deployment-c4f59bdf5 (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  48m   deployment-controller  Scaled up replica set app-deployment-c4f59bdf5 to 1
PS C:\Users\Rudra\Desktop\kubernetes>




PS C:\Users\Rudra\Desktop\kubernetes> kubectl get pods   
NAME                             READY   STATUS    RESTARTS   AGE
app-deployment-c4f59bdf5-dgglj   1/1     Running   0          49m
PS C:\Users\Rudra\Desktop\kubernetes>


here is service.yaml

apiVersion: v1
kind: Service
metadata:
  name: app-service
spec:
  selector:
    app: app
  ports:
  - port: 21000
    targetPort: 8080
  type: LoadBalancer



PS C:\Users\Rudra\Desktop\kubernetes> kubectl apply -f service.yaml
service/app-service created


PS C:\Users\Rudra\Desktop\kubernetes> kubectl get all 
NAME                                 READY   STATUS    RESTARTS   AGE
pod/app-deployment-c4f59bdf5-dgglj   1/1     Running   0          52m

NAME                  TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)           AGE
service/app-service   LoadBalancer   10.101.224.177   localhost     21000:32628/TCP   46m
service/kubernetes    ClusterIP      10.96.0.1        <none>        443/TCP           133m

NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/app-deployment   1/1     1            1           52m

NAME                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/app-deployment-c4f59bdf5   1         1         1       52m
PS C:\Users\Rudra\Desktop\kubernetes>


runnig port now go to (http://localhost:21000)
 ```