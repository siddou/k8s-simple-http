https://medium.com/@jandavid.staerk/why-kubernetes-is-awesome-9f7ff0186996

## Usage

```shell
minikube ssh
mkdir /mnt/data
wget -P mnt/data https://gist.github.com/jdstaerk/7a4c43ea94cefd682c12f20d95e9b0a8/raw/dd44b4f8f32ddf6c6959f5926009dceacc37b01d/index.js
```

```shell
kubectl create -f pv-volume.yaml
kubectl create -f pv-claim.yaml
kubectl create -f pv-pod.yaml
kubectl create -f pv-service.yaml
kubectl get pod,services
kubectl describe service/task-pv-service

```


## test exposed ports
```shell
export POD_IP=$(kubectl describe service/task-pv-service | grep "Endpoints:" | awk '{print $2}')
curl $POD_IP

export CLUSTER_IP=$(kubectl describe service/task-pv-service | grep "IP:" | awk '{print $2}')
curl $CLUSTER_IP:8080
```

## check volume storage inside pod
```shell
kubectl exec -it task-pv-pod -- /bin/bash
ls /usr/share/nginx/html
exit
```