In this scenario you will create a manifest that will deploy a single pod that will:
- Depoy an nginx container
- Create a data volume and mount it to teh default nginx directory
- Start a container that will clone a git repo to the volume before starting the nginx container

Click the code below to copy to the file editor:
<pre class="file" data-target="clipboard">
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: myapp 
  name: myapp
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
  Save this as a file named pod.yml
    
 Next, you can apply that manifest file with this command:
 `kubectl apply -f pod.yml` {{execute}}
