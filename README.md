# k8s task lec2 answer
## Part1:

### 1- create pod nginx with name my nginx direct from command don't use yaml file

  - **explaination:** create pod with name of my-nginx
  - **command** :

        kubectl run my-nginx --image=nginx
  - **output of command** :

        pod/my-nginx created
    
  - **Screenshot** :
    
    <img src="images/1.png">
---
        
### 2- create pod nginx with name my nginx command and use Image nginx123  direct from command don't use yaml file

  - **explaination:** create pod with name of my-nginx using image called nginx123
  - **command** :

        kubectl run mynginx --image=nginx123
  - **output of command** :

        pod/mynginx created
    
  - **Screenshot** :
    
    <img src="images/2.png">
---
### 3- check the status and why it doesn't work 

  - **explaination:** can't find nginx123 on Docker hub
  - **command** :

        kubectl get pod mynginx
  - **output of command** :

        NAME      READY   STATUS             RESTARTS   AGE
        mynginx   0/1     ImagePullBackOff   0          9m15s
    
  - **Screenshot** :
    
    <img src="images/3.png">
---
### 4- I need to know node name - IP - Image Of the POD

  - **explaination:** get info about node name - IP - Image Of the POD
  - **command** :

        kubectl describe pod mynginx
  - **output of command** :

        Name:             mynginx
        Namespace:        default
        Priority:         0
        Service Account:  default
        Node:             minikube/192.168.49.2
        Start Time:       Mon, 01 Sep 2025 09:58:33 +0000
        Labels:           run=mynginx
        Annotations:      <none>
        Status:           Pending
        IP:               10.244.0.6
        IPs:
          IP:  10.244.0.6
        Containers:
          mynginx:
            Container ID:   
            Image:          nginx123
            Image ID:       
            Port:           <none>
            Host Port:      <none>
            State:          Waiting
              Reason:       ImagePullBackOff
            Ready:          False
            Restart Count:  0
            Environment:    <none>
            Mounts:
              /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-qrsfn (ro)
        Conditions:
          Type                        Status
          PodReadyToStartContainers   True 
          Initialized                 True 
          Ready                       False 
          ContainersReady             False 
          PodScheduled                True 
        Volumes:
          kube-api-access-qrsfn:
            Type:                    Projected (a volume that contains injected data from multiple sources)
            TokenExpirationSeconds:  3607
            ConfigMapName:           kube-root-ca.crt
            Optional:                false
            DownwardAPI:             true
        QoS Class:                   BestEffort
        Node-Selectors:              <none>
        Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                                     node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
        Events:
          Type     Reason     Age                   From               Message
          ----     ------     ----                  ----               -------
          Normal   Scheduled  13m                   default-scheduler  Successfully assigned default/mynginx to minikube
          Normal   Pulling    10m (x5 over 13m)     kubelet            Pulling image "nginx123"
          Warning  Failed     10m (x5 over 13m)     kubelet            Failed to pull image "nginx123": Error response from daemon: pull access denied for nginx123, repository does not exist or may require 'docker login': denied: requested access to the resource is denied
          Warning  Failed     10m (x5 over 13m)     kubelet            Error: ErrImagePull
          Normal   BackOff    3m38s (x42 over 13m)  kubelet            Back-off pulling image "nginx123"
          Warning  Failed     3m38s (x42 over 13m)  kubelet            Error: ImagePullBackOff
    
---
### 5- delete the pod 

  - **explaination:** delete the pod 
  - **command** : 

        kubectl delete pod mynginx
  - **output of command** :

        pod "mynginx" deleted from default namespace
    
  - **Screenshot** :
    
    <img src="images/5.png">
---
### 6- create another one with yaml file and use label

  - **explaination:** create a pod in yaml file
  - **command** :

        kubectl apply -f pod.yaml
  - **output of command** :

        pod/mynginx created
    
  - **Screenshot** :
    
    <img src="images/6.png">
---
### 7-create Riplicaset with 3 replicas using nginx Image 

  - **explaination:** create Riplicaset with 3 replicas using nginx Image
  - **command** :

        kubectl apply -f replica.yaml
  - **output of command** :

        replicaset.apps/mynginx-rs created
    
  - **Screenshot** :
    
    <img src="images/7.png">
---
### 8-scale the replicas to 5 without edit in the Yaml file
 
  - **explaination:** scale the replicas to 5 without edit in the Yaml file
  - **command** :

        kubectl scale rs mynginx-rs --replicas=5
  - **output of command** :

        replicaset.apps/mynginx-rs scaled
    
  - **Screenshot** :
    
    <img src="images/8.png">
---
### 9-Delete any one of the 5 pods and check what happen and explain 

  - **explaination:** when we delete one of the pods of replicas, replicaset will create another pod because replica does self healing
  - **command** :

        kubectl delete pod mynginx-rs-xrglt
  - **output of command** :

        pod "mynginx-rs-xrglt" deleted from default namespace
    
  - **Screenshot** :
    
    <img src="images/9.png">
---
### 10-Scale down the pods aging to 2 without scale command use terminal  

  - **explaination:** Scale down the pods aging to 2 without scale command use terminal
  - **command** :

        kubectl edit rs mynginx-rs
  - **output of command** :

        replicaset.apps/mynginx-rs edited
    
  - **Screenshot** :
    
    <img src="images/10-1.png">
    <img src="images/10-2.png">
---
### 11-find out the issue in the below Yaml (don't use AI)

    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: replicaset-2
    spec:
      replicas: 2
      selector:
        matchLabels:
          tier: frontend
      template:
        metadata:
          labels:
            tier: nginx
        spec:
          containers:
          - name: nginx
            image: nginx

  - **explaination:** labels must be the same as selector
  - **after correction** :

        apiVersion: apps/v1
        kind: ReplicaSet
        metadata:
          name: replicaset-2
        spec:
          replicas: 2
          selector:
            matchLabels:
              tier: frontend
          template:
            metadata:
              labels:
                tier: frontend
            spec:
              containers:
              - name: nginx
                image: nginx

---
### 12-find out the issue in the below Yaml (don't use AI)
    
    apiVersion: apps/v1
    kind: deployment
    metadata:
      name: deployment-1
    spec:
      replicas: 2
      selector:
        matchLabels:
          name: busybox-pod
      template:
        metadata:
          labels:
            name: busybox-pod
        spec:
          containers:
          - name: busybox-container
            image: busybox
            command:
            - sh
            - "-c"
            - echo Hello Kubernetes! && sleep 3600
            
  - **explaination:** kubernates is a case sensitive because it written in YAML
  - **after correction** :
    
        kind: Deployment
---
### 13- find out the issue in the below Yaml (don't use AI)

    apiVersion: v1
    kind: Deployment
    metadata:
      name: deployment-1
    spec:
      replicas: 2
      selector:
        matchLabels:
          name: busybox-pod
      template:
        metadata:
          labels:
            name: busybox-pod
        spec:
          containers:
          - name: busybox-container
            image: busybox
            command:
            - sh
            - "-c"
            - echo Hello Kubernetes! && sleep 3600

  - **explaination:** in deployment apiVersion must be app/v1
  - **after correction** :

        apiVersion: apps/v1

---
### 14- what's command you use to know what Image name that running the deployment 

  - **command** :

        kubectl get deploy -o wide
    
  - **Screenshot** :

    <img src="images/14.png">
---
### 15- create deployment using following data :
  - Name: httpd-frontend;
  - Replicas: 3;
  - Image: httpd:2.4-alpine

  - **command** :

        kubectl apply -f deploy.yaml 
  - **output of command** :

        deployment.apps/httpd-frontend created
    
  - **Screenshot** :
    
    <img src="images/15.png">
---
### 16- replace the image to nginx777 with command directly 

  - **command** :

        kubectl set image deployment/httpd-frontend httpd=nginx777 
  - **output of command** :

        deployment.apps/httpd-frontend image updated
    
  - **Screenshot** :
    
    <img src="images/16.png">
---
### 17- rollback to pervious version

  - **command** :

        kubectl rollout undo deployment httpd-frontend --to-revision=1
  - **output of command** :

        deployment.apps/httpd-frontend rolled back
    
  - **Screenshot** :
    
    <img src="images/17-1.png">
    <img src="images/17-2.png">
---
### 18- Create a Simple Web Application:
* Use a Dockerfile to create a simple web application (e.g., an Nginx server serving an HTML page).
* Build the Docker image and push it to DockerHub your private Account.
    
  - **Screenshot** :
    
    <img src="images/18-1.png">
    <img src="images/18-2.png">
### 19- Create a Deployment Using This Image:
* Deploy the Docker image from DockerHub to Kubernetes with a Deployment that has 3 replicas.
    
  - **Screenshot** :
    
    <img src="images/19.png">
