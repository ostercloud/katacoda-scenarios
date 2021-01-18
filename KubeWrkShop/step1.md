In this scenario you will create a manifest that will deploy a single pod that will:
- Depoy an nginx container
- Create a data volume and mount it to the default nginx directory
- Start a container that will clone a git repo to the volume before starting the nginx container

Click the code below to copy to the file editor:
<pre class="file" data-filename="pod.yml" data-target="insert">
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
    
 Next, you can apply that manifest file with this command:
 `kubectl apply -f pod.yml`{{execute}}
 
 If you want to check that the containers are running and that the git repo was cloned sucessfully, 
 This command will show the IP address for the pod: `kubectl get pod myapp -o wide`{{execute}}
 
 A simple curl request to that address should return "This is test data from Github Repo"
 
