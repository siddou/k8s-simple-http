https://medium.com/@jandavid.staerk/why-kubernetes-is-awesome-9f7ff0186996

## Usage

```shell
minikube ssh
mkdir /mnt/data
wget -P /mnt/data https://gist.github.com/jdstaerk/7a4c43ea94cefd682c12f20d95e9b0a8/raw/dd44b4f8f32ddf6c6959f5926009dceacc37b01d/index.js
```

cat index.js
```shell
const os = require('os');
const http = require('http');

const requestHandler = (req, res) => {
    console.log('Request incoming from ' + req.connection.remoteAddress);
    res.writeHead(200);
    res.end('Success! You\'ve hit ' + os.hostname() + ' on ' + os.platform() + '!');
};

const server = http.createServer(requestHandler)

server.listen(3000);
```

```shell
kubectl create -f simple-http-volume.yaml
kubectl create -f simple-http-claim.yaml
kubectl create -f simple-http-deployment.yaml
kubectl create -f simple-http-service.yaml
kubectl get pod,services
kubectl describe service/simple-http
```


## test exposed port
```shell
export CLUSTER_IP=$(kubectl describe service/simple-http | grep "IP:" | awk '{print $2}')
curl $CLUSTER_IP:3000
```

## check volume storage inside pod
```shell
kubectl exec -it simple-http-c48f7584d-pfhp4 -- /bin/bash
root@simple-http-c48f7584d-pfhp4:/# ls /js
exit
```

