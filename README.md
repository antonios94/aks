# Khazna Task

## Level1

``` bash
kubectl get cm -n level1 readme -oyaml

kubectl config set-context --current --namespace=level1

 #Welcome to Level 1!\n\nTask: Create a simple Pod running nginx in this namespace.\n\nTo reveal the next level, create a ConfigMap named 'next-level' with the data: \nkey: 'unlock'\nvalue: 'level2'\n"}
 #Sol
 kubectl run nginx --image=nginx:latest -n level1

#GOTO Level2
cat <<EOF | kubectl apply -n level1 -f -
apiVersion: v1
kind: ConfigMap
metadata:
  name: next-level
data:
  unlock: "level2"
EOF

```

## Level2

``` bash
kubectl get cm -n level2 readme -oyaml

# Task: Create a Deployment with 3 replicas of a simple web application.
# Sol
kubectl create deployment web --image=nginx:latest --replicas=3 -n level2

#GOTO Level2
#To reveal the next level, create a Secret named 'next-level' with the data:
#key: 'unlock'
#value: 'level3' (base64 encoded)

kubectl create secret generic next-level --from-literal=unlock=level3 -n level2

```

## Level3

``` bash
kubectl get cm -n level3 readme -oyaml

#Task: Create a StatefulSet with a persistent volume for a database application.


kubectl apply -f mongo-sts.yaml -n level3

#GOTO Level4
#    To reveal the next level, create a Service named 'next-level' with the label:
#    key: 'unlock'
#    value: 'level4'

cat <<EOF | kubectl apply -n level3 -f -
apiVersion: v1
kind: Service
metadata:
  name: next-level
  labels:
    unlock: level4
spec:
  ports:
    - port: 27017
      targetPort: 27017
  selector:
    unlock: level4
EOF


```

## Level4

``` bash
#Task: Set up a complete application with frontend, backend, and database services,
#including proper network policies and resource limits.

kubectl apply -f -n level4 sql.yaml backend.yaml frontend.yaml

kubectl apply -f -n level4 network-policy.yaml

#GOTO Level5
#To reveal the next level, create an Ingress resource named 'next-level' with the annotation:
#key: 'kubernetes.io/ingress.class'
#value: 'level5'

kubectl apply -f ingress.yaml

```

## Level5

``` bash
#deploy Jenkins and create a pipeline that deploy a simple html page to be displayed by nginx

#First Tried install heml with helm but got errors related to rolebinding for jenkins to can create pod for agent 
helm upgrade -i jenkins jenkins/jenkins -n level5  \
--set nameOverride=jenkins \
--set controller.admin.password=P@ssw0rd \
--set controller.nodePort=8080 \
--set controller.ingress.enabled=true \
--set controller.ingress.hostName=jenkins.test.local \
--set controller.ingress.ingressClassName=nginx \
--set controller.image.tag=2.479.2-lts

#After helm failed i installed jenkins with yaml file
kubectl apply -f jenkins.yaml
Jenkins External IP:57.152.97.71:80
admin:P@ssw0rd
#Got jenkins password and installed plugins

#can't run agents as pods because of permissions issues for roles so i took a workaround to can run the pipeline on jenkins server itself [ installed podman & kubectl inside  jenkins]

Create pipeline to build docker image for the application [ compy a test html file inside nginx image and build it with podman ]
Then push the new image to docker hub 
update deployment yaml 
Deploy on aks cluster with config file 
Note : i faced an issue with kubectl apply because of kubelogin is not installed which is required to connect to AAD enabled cluster so i needed to install it or try a jenkins plugin to connect to aks cluster .
at the end i applied the deployment with the builded image and it works 
application service ip : 135.234.192.200:80

```
