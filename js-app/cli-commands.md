### These commands are used in the lecture "Deploying Images in Kubernetes from private Docker repository"

##### Print full docker login command for aws ecr

`aws ecr get-login-password`

##### Login to docker private repo

`docker login -u username -p password`

##### Base64 encode config file

`cat .docker/config.json | base64`

##### Create docker login secret from config.json file

```sh
kubectl create secret generic my-registry-key \
--from-file=.dockerconfigjson=.docker/config.json \
--type=kubernetes.io/dockerconfigjson
```
`kubectl create secret generic my-registry-key --from-file=.dockerconfigjson=.docker/config.json --type=kubernetes.io/dockerconfigjson`

###### Access generated secret

`kubectl get secret`

##### Create docker login secret with login credentials

```sh
kubectl create secret docker-registry my-registry-key \
--docker-server=https://private-repo \
--docker-username=user(AWS enter it) \
--docker-password=pwd 
```

`kubectl create secret docker-registry my-registry-key --docker-server=https://private-repo(enter it) --docker-username=user --docker-password=pwd(enter it)`
- get pwd by `aws ecr get-login-password`
- kubectl apply -f my-app-deployment.yaml
- kubectl get pod
- kubectl describe pod [pod-name]
## Succesfully-pulled
![k8s-Succesfully-pulled.png](/assets/k8s-Succesfully-pulled.png)
##### Access minikube console

`minikube ssh`

##### Copy config.json file from Minikube to my host

`minikube cp minikube:/home/docker/.docker/config.json /users/USERNAME/.docker/config.json`
