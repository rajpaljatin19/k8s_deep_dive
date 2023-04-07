# k8s_deep_dive session

1. Cloning to Git Repo:

```
jatinrajpal@Jatins-MacBook-Air demo % git clone https://github.com/rajpaljatin19/k8s_deep_dive.git
Cloning into 'k8s_deep_dive'...
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 22 (delta 1), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (22/22), 7.22 KiB | 7.22 MiB/s, done.
Resolving deltas: 100% (1/1), done.
```

2. Navigate to ```k8s_deep_dive/docker``` directory and run ```docker build``` command to build the app
    
```
jatinrajpal@Jatins-MacBook-Air demo % cd k8s_deep_dive/docker
jatinrajpal@Jatins-MacBook-Air docker % docker build -t demoapp .
[+] Building 10.9s (9/9) FINISHED
 => [internal] load build definition from Dockerfile                                                         0.0s
 => => transferring dockerfile: 230B                                                                         0.0s
 => [internal] load .dockerignore                                                                            0.0s
 => => transferring context: 2B                                                                              0.0s
 => [internal] load metadata for docker.io/library/python:3.9-slim-buster                                    4.5s
 => [internal] load build context                                                                            0.0s
 => => transferring context: 747B                                                                            0.0s
 => [1/4] FROM docker.io/library/python:3.9-slim-buster@sha256:0ac1e4889c90d3117680b33cbdd397e3d95098b4c6bc  3.2s
 => => resolve docker.io/library/python:3.9-slim-buster@sha256:0ac1e4889c90d3117680b33cbdd397e3d95098b4c6bc  0.0s
 => => sha256:0ac1e4889c90d3117680b33cbdd397e3d95098b4c6bcff8600035fa5343527ed 988B / 988B                   0.0s
 => => sha256:a9fe8430888821244e2f80688dfa0d5bddb233c65160977326a54235b383fc8f 1.37kB / 1.37kB               0.0s
 => => sha256:89a1e068e9a2a7fbec57ccd79a1455269208c430c275b9d5c933b0dd121454ae 6.90kB / 6.90kB               0.0s
 => => sha256:cfe0f778e53ce031ef449bc02975a07f2568716dea807ed2f390d352c0001972 25.92MB / 25.92MB             1.8s
 => => sha256:a8e11a7826c6ce9b3366e1d2c4dfbdec39133e62fe15bca830f2947fd1064b6c 2.65MB / 2.65MB               1.8s
 => => sha256:b1c45cd4de1898925eafc2e4113923f17a98fff25b028f350aab970ed5f411e9 11.19MB / 11.19MB             1.2s
 => => sha256:f47ec9e5e8c3402a60c0f6194af85b8a96af980ae1368d7afcfd98cabc9348f4 242B / 242B                   1.5s
 => => sha256:8fcba38b80a230d869a3f1e8053026976cd8f63bc8c02bb86e558d0566e57c7e 3.19MB / 3.19MB               2.0s
 => => extracting sha256:cfe0f778e53ce031ef449bc02975a07f2568716dea807ed2f390d352c0001972                    0.7s
 => => extracting sha256:a8e11a7826c6ce9b3366e1d2c4dfbdec39133e62fe15bca830f2947fd1064b6c                    0.1s
 => => extracting sha256:b1c45cd4de1898925eafc2e4113923f17a98fff25b028f350aab970ed5f411e9                    0.3s
 => => extracting sha256:f47ec9e5e8c3402a60c0f6194af85b8a96af980ae1368d7afcfd98cabc9348f4                    0.0s
 => => extracting sha256:8fcba38b80a230d869a3f1e8053026976cd8f63bc8c02bb86e558d0566e57c7e                    0.1s
 => [2/4] WORKDIR /app                                                                                       0.1s
 => [3/4] RUN pip install flask                                                                              2.9s
 => [4/4] COPY . .                                                                                           0.0s
 => exporting to image                                                                                       0.1s
 => => exporting layers                                                                                      0.1s
 => => writing image sha256:0523a04ecfee671e5f06753534f5125e18a5b2f084baa5b2f4f5bd541d425d1a                 0.0s
 => => naming to docker.io/library/demoapp                                                                   0.0s
```

3. Verify of the image is created post running ```docker build``` command

    jatinrajpal@Jatins-MacBook-Air docker % docker image ls
    REPOSITORY                                 TAG       IMAGE ID       CREATED         SIZE
    demoapp                                    latest    0523a04ecfee   8 seconds ago   122MB

4. Run ```docker run``` command to run your containerized application locally on the machine

```
jatinrajpal@Jatins-MacBook-Air docker % docker run -p 8000:8000 demoapp
 * Serving Flask app 'prog.py'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:8000
 * Running on http://172.17.0.2:8000
Press CTRL+C to quit
```

5. Now that your flask webservice is up and running, call your service to make sure you are getting the desired output (via seperate terminal)

```
jatinrajpal@Jatins-MacBook-Air ~ % curl -v http://127.0.0.1:8000
*   Trying 127.0.0.1:8000...
* Connected to 127.0.0.1 (127.0.0.1) port 8000 (#0)
> GET / HTTP/1.1
> Host: 127.0.0.1:8000
> User-Agent: curl/7.86.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Server: Werkzeug/2.2.3 Python/3.9.16
< Date: Mon, 03 Apr 2023 06:40:10 GMT
< Content-Type: text/html; charset=utf-8
< Content-Length: 157
< Connection: close
<
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>FlaskBlog</title>
</head>
<body>
   <h1>Welcome to my Blog</h1>
</body>
* Closing connection 0
</html>%
```

6. Now that you've tested the app locally, lets run this on```kubernetes```. First step towards that is to run ```docker tag``` to tag your image

```
$ docker image tag pythonflask-demoapp:latest rajpaljatin19/pythonflask-demoapp:latest
```

7. Now run ```docker push``` command to push your docker image to docker hub or any other image registry. For our example, i am using docker hub.

```
jatinrajpal@Jatins-MacBook-Air docker % docker push <docker_hub_userid>/pythonflask-demoapp:latest
The push refers to repository [docker.io/rajpaljatin19/pythonflask-demoapp]
de8bb64b3075: Pushed
0aaf8cc6a65a: Pushed
cf99cfa2e6a8: Pushed
cbb6f10d19ff: Mounted from library/python
26f80e5f8eac: Mounted from library/python
407a0715f2df: Mounted from library/python
ccd22ec8669c: Mounted from library/python
3bea4e7d7b00: Mounted from library/python
latest: digest: sha256:0499a50032a24be27e420f864422e63ebc1ea712a57ffe7a362b3b699bde339a size: 1995
```

8. Now that you have pushed the image to docker hub, lets start our ```minikube``` instance. [Minikube](https://minikube.sigs.k8s.io/docs/start/) is the local version of kubernetes cluster that you can run on your system

```
jatinrajpal@Jatins-MacBook-Air ~ % minikube start
😄  minikube v1.25.2 on Darwin 13.3 (arm64)
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔄  Restarting existing docker container for "minikube" ...
🐳  Preparing Kubernetes v1.23.3 on Docker 20.10.12 ...
    ▪ kubelet.housekeeping-interval=5m
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

9. Then navigate to ```k8s_configs ``` directory and run ```kubectl apply -f``` to your deployment ```YAML``` config file

```
jatinrajpal@Jatins-MacBook-Air k8s % kubectl apply -f deployment.yaml
deployment.apps/demoapp created
service/demoapp-svc created
```

10. Check the deployment and services created as per your YAML
   
```
jatinrajpal@Jatins-MacBook-Air k8s_deep_dive % kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
demoapp-5b9b96569-dcrq7   1/1     Running   0          67m
demoapp-5b9b96569-k7rxd   1/1     Running   0          67m
demoapp-5b9b96569-p68p4   1/1     Running   0          67m

jatinrajpal@Jatins-MacBook-Air k8s_deep_dive % kubectl get deployment
NAME      READY   UP-TO-DATE   AVAILABLE   AGE 
demoapp   3/3          3        3          67m

jatinrajpal@Jatins-MacBook-Air k8s % kubectl get svc
NAME          TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
demoapp-svc   LoadBalancer   10.104.106.253   <pending>     6000:31494/TCP   3s
kubernetes    ClusterIP      10.96.0.1        <none>        443/TCP          2d
```

11.  Run ```minikube tunnel```. Tunnel creates a route to services deployed with type LoadBalancer and sets their Ingress to their ClusterIP

```
jatinrajpal@Jatins-MacBook-Air k8s % minikube tunnel
✅  Tunnel successfully started
📌  NOTE: Please do not close this terminal as this process must stay alive for the tunnel to be accessible ...
🏃  Starting tunnel for service demoapp-svc.
```

12. Once tunnel is started, you will notice ```EXTERNAL-IP``` will start  coming in ```kubectl get svc```  (via seperate terminal window)

```
jatinrajpal@Jatins-MacBook-Air ~ % kubectl get svc
NAME          TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
demoapp-svc   LoadBalancer   10.104.106.253   127.0.0.1     6000:31494/TCP   11m
kubernetes    ClusterIP      10.96.0.1        <none>        443/TCP          2d
jatinrajpal@Jatins-MacBook-Air ~ %
```

 13. You can then run ```curl``` to validate if your app is running or not. For our case, its very much up and running (via seperate terminal window)

```
jatinrajpal@Jatins-MacBook-Air ~ % curl -v "http://127.0.0.1:6000"
*   Trying 127.0.0.1:6000...
* Connected to 127.0.0.1 (127.0.0.1) port 6000 (#0)
> GET / HTTP/1.1
> Host: 127.0.0.1:6000
> User-Agent: curl/7.87.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Server: Werkzeug/2.2.3 Python/3.9.16
< Date: Wed, 05 Apr 2023 08:57:15 GMT
< Content-Type: text/html; charset=utf-8
< Content-Length: 157
< Connection: close
<
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>FlaskBlog</title>
</head>
<body>
   <h1>Welcome to my Blog</h1>
</body>
* Closing connection 0
</html>%
jatinrajpal@Jatins-MacBook-Air ~ %
```



