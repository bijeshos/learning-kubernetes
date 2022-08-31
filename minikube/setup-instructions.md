# Minikube setup instructions

## Pre-requisites
- Check whether virtualization is enabled or not
    - `$ grep -E --color 'vmx|svm' /proc/cpuinfo`
        - if the above command returns no values, ensure to turn on virtualization in BIOS settings

## Installation
- Install VirtualBox
    - Refer: https://www.virtualbox.org/wiki/Downloads
- Install minikube
    - Refer: https://minikube.sigs.k8s.io/docs/start/
- Start minikube 
    - $ `minikube start --driver virtualbox`

- Check minikube status
    - $ `minikube status`

- Check k8s cluster
    $ `kubectl get pods -A`

## Deployments
- Deploy sample application
    - $ `kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4`
    - $ `kubectl expose deployment hello-minikube --type=NodePort --port=8080`
- Check services
    - $ `kubectl get services hello-minikube`
- Launch service in browswer
    - $ `minikube service hello-minikube`
        - This launches the service default port
    - or 
    - $ `kubectl port-forward service/hello-minikube 7080:`
        - This launches service on http://localhost:7080/

## Cleanup 
- Delete sample services
    - $ `kubectl delete services hello-minikube`
- Delete sample deployment
    - `$ kubectl delete deployment hello-minikube`


## Reference:
- https://minikube.sigs.k8s.io/docs/start/