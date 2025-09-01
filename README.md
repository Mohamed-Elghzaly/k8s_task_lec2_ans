# k8s task lec2 answer

###Part1:

**1- create pod nginx with name my nginx direct from command don't use yaml file**

  - **explaination**:         create pod with name of my-nginx and run on port 80
  - **command** :
                        `kubectl run my-nginx --image=nginx --port=80`
  - **output of command** :   `pod/my-nginx created`
  - **Screenshot** : 
        
2- create pod nginx with name my nginx command and use Image nginx123  direct from command don't use yaml file

3- check the status and why it doesn't work 

4- I need to know node name - IP - Image Of the POD

5- delete the pod 

6- create another one with yaml file and use label

7-create Riplicaset with 3 replicas using nginx Image 

8-scale the replicas to 5 without edit in the Yaml file
 
9-Delete any one of the 5 pods and check what happen and explain 

10-Scale down the pods aging to 2 without scale command use terminal  
