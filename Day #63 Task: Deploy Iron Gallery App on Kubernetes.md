# Day #63 task: Deploy Iron Gallery App on Kubernetes

There is an iron gallery app that the Nautilus DevOps team was developing. They have recently customized the app and are going to deploy the same on the Kubernetes cluster. Below you can find more details:

1. Create a namespace iron-namespace-xfusion


2. Create a deployment iron-gallery-deployment-xfusion for iron gallery under the same namespace you created.

:- Labels run should be iron-gallery.

:- Replicas count should be 1.

:- Selector's matchLabels run should be iron-gallery.

:- Template labels run should be iron-gallery under metadata.

:- The container should be named as iron-gallery-container-xfusion, use kodekloud/irongallery:2.0 image ( use exact image name / tag ).

:- Resources limits for memory should be 100Mi and for CPU should be 50m.

:- First volumeMount name should be config, its mountPath should be /usr/share/nginx/html/data.

:- Second volumeMount name should be images, its mountPath should be /usr/share/nginx/html/uploads.

:- First volume name should be config and give it emptyDir and second volume name should be images, also give it emptyDir.


3. Create a deployment iron-db-deployment-xfusion for iron db under the same namespace.

:- Labels db should be mariadb.

:- Replicas count should be 1.

:- Selector's matchLabels db should be mariadb.

:- Template labels db should be mariadb under metadata.

:- The container name should be iron-db-container-xfusion, use kodekloud/irondb:2.0 image ( use exact image name / tag ).

:- Define environment, set MYSQL_DATABASE its value should be database_blog, set MYSQL_ROOT_PASSWORD and MYSQL_PASSWORD value should be with some complex passwords for DB connections, and MYSQL_USER value should be any custom user ( except root ).

:- Volume mount name should be db and its mountPath should be /var/lib/mysql. Volume name should be db and give it an emptyDir.

4. Create a service for iron db which should be named iron-db-service-xfusion under the same namespace. Configure spec as selector's db should be mariadb. Protocol should be TCP, port and targetPort should be 3306 and its type should be ClusterIP.

5. Create a service for iron gallery which should be named iron-gallery-service-xfusion under the same namespace. Configure spec as selector's run should be iron-gallery. Protocol should be TCP, port and targetPort should be 80, nodePort should be 32678 and its type should be NodePort.


**Note:**
1. We don't need to make connection b/w database and front-end now, if the installation page is coming up it should be enough for now.
2. The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.



## step1: Create the yaml file, iron-gallery-setup.yaml
```
vi iron-gallery-setup.yaml
```

```
apiVersion: v1
kind: Namespace
metadata:
  name: iron-namespace-xfusion
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-gallery-deployment-xfusion
  namespace: iron-namespace-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      run: iron-gallery
  template:
    metadata:
      labels:
        run: iron-gallery
    spec:
      containers:
        - name: iron-gallery-container-xfusion
          image: kodekloud/irongallery:2.0
          resources:
            limits:
              memory: "100Mi"
              cpu: "50m"
          volumeMounts:
            - name: config
              mountPath: /usr/share/nginx/html/data
            - name: images
              mountPath: /usr/share/nginx/html/uploads
      volumes:
        - name: config
          emptyDir: {}
        - name: images
          emptyDir: {}
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-db-deployment-xfusion
  namespace: iron-namespace-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      db: mariadb
  template:
    metadata:
      labels:
        db: mariadb
    spec:
      containers:
        - name: iron-db-container-xfusion
          image: kodekloud/irondb:2.0
          env:
            - name: MYSQL_DATABASE
              value: database_blog
            - name: MYSQL_ROOT_PASSWORD
              value: "R@@tP@ssw0rd123!"
            - name: MYSQL_USER
              value: devuser
            - name: MYSQL_PASSWORD
              value: "D3v0ps@321"
          volumeMounts:
            - name: db
              mountPath: /var/lib/mysql
      volumes:
        - name: db
          emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  name: iron-db-service-xfusion
  namespace: iron-namespace-xfusion
spec:
  type: ClusterIP
  selector:
    db: mariadb
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
---
apiVersion: v1
kind: Service
metadata:
  name: iron-gallery-service-xfusion
  namespace: iron-namespace-xfusion
spec:
  type: NodePort
  selector:
    run: iron-gallery
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 32678
```
<img width="636" height="496" alt="image" src="https://github.com/user-attachments/assets/7b7551bc-51f7-44ab-8543-e46dfce4fd09" />

<img width="631" height="513" alt="image" src="https://github.com/user-attachments/assets/e2298814-c596-40af-b151-3c328aed00ab" />

<img width="527" height="498" alt="image" src="https://github.com/user-attachments/assets/202920ea-7f99-4a0b-9221-c405fc6deb92" />

<img width="390" height="446" alt="image" src="https://github.com/user-attachments/assets/05f348be-1a4c-43d0-b9a9-40174f57c341" />

## step2: apply the manifest file
```
k apply -f iron-gallery-setup.yaml
```
<img width="505" height="104" alt="image" src="https://github.com/user-attachments/assets/794a043b-a0f4-4366-bd4c-12d36ec41546" />


##  step3: get all resources which are the tings are created in given namespace
```
k get all -n iron-namespace-xfusion
```
<img width="786" height="259" alt="image" src="https://github.com/user-attachments/assets/052acc1e-f12a-4167-9c14-59f2361e970e" />


## step4: Get all pods in given namespace
```
k get pods -n iron-namespace-xfusion
```
<img width="1129" height="266" alt="image" src="https://github.com/user-attachments/assets/fd1dda14-483d-4150-b6ec-d09437bbe5cb" />

## step5: validate with curl
```
curl http://localhost:32678
```
