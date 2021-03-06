In this scenario we will create a replica set that will allow us to scale our pod across multiple nodes. This will also cause our app to be more fault tolerant as a pod will be recreated if it goes down. 

Here is the manifest file for our ReplicaSet, click copy to editor to create the file. 
<pre class="file" data-filename="replica.yml" data-target="insert">
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-frontend
  labels:
    app: myapp
    tier: frontend
spec:
  replicas: 2
  selector:
    matchLabels: 
      app: myapp
  template:
    metadata: 
      labels: 
        app: myapp
        tier: frontend
    spec:
      containers:
        - image: nginx
          name: nginx-app
          ports:
          - containerPort: 80
          volumeMounts:
          - mountPath: "/usr/share/nginx/html"
            name: www-data
      initContainers:
        - name: git-cloner
          image: alpine/git
          args:
          - clone
          - https://github.com/ostercloud/kubeappdatashop
          - /data
          volumeMounts:
            - mountPath: /data
              name: www-data
      volumes:
         - name: www-data
           emptyDir: {}
</pre>

 Next, you can apply that manifest file with this command:
 `kubectl apply -f replica.yml`{{execute}}
 
 Try exploring th running pods and get information about your newly created ReplicaSet
 `kubectl get pods`{{execute}}
 `kubectl get rs/myapp-frontend -o wide`{{execute}}
 `kubectl describe rs/myapp-frontend`{{execute}}
