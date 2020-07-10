### Kubernetes For Application Deployment

Kubernetes is a tool that deploys and manages applications. In every Waggle/SAGE node, one Kubernetes cluster exists and each device in the node runs one kubernetes node. A Kubernetes master (or server) runs on the device that talks to the outside world (in other words, the device that directly reaches out to Internet). The lightweight version of Kubernetes, as known as k3s, is the Kubernetes master in Waggle/SAGE nodes.

#### Setting Up A k3s Server

To install k3d,
```
$ apt-get update \
  && apt-get install -y curl \
  && wget -q -O - https://raw.githubusercontent.com/rancher/k3d/master/install.sh | bash
```

To run a k3s cluster in Docker,
```
$ k3d create cluster \
  --name waggle-k3s \
  --auto-restart
```

To query the live containers,
```
$ docker exec -ti k3d-waggle-k3s-server kubectl get pods --all-namespaces
NAMESPACE     NAME                                      READY   STATUS      RESTARTS   AGE
kube-system   local-path-provisioner-58fb86bdfd-rflsp   1/1     Running     0          4m14s
kube-system   metrics-server-6d684c7b5-fglwn            1/1     Running     0          4m14s
kube-system   coredns-d798c9dd-8xmph                    1/1     Running     0          4m14s
kube-system   helm-install-traefik-6g9n2                0/1     Completed   2          4m18s
kube-system   svclb-traefik-d6fk6                       2/2     Running     0          3m20s
kube-system   traefik-6787cddb4b-qgmzx                  1/1     Running     0          3m21s
# or
$ docker exec -ti k3d-waggle-k3s-server crictl ps
CONTAINER       IMAGE           CREATED              STATE      NAME                     ATTEMPT   POD ID
ab7f548c919f9   1cdb7e2bd5e25   14 seconds ago       Running   traefik                  0          a22bc871398f6
dd005538f97de   9be4f056f04b7   14 seconds ago       Running   lb-port-443              0          7cb5cf72d7666
af4f1870222fe   9be4f056f04b7   15 seconds ago       Running   lb-port-80               0          7cb5cf72d7666
e1faf158bff70   8ae59e1bfb260   About a minute ago   Running   coredns                  0          e20f1b4faaa03
d55a52956a631   f9499facb1e8c   About a minute ago   Running   metrics-server           0          2c0ea9a2f2e40
07434a3dade7d   caea073758499   About a minute ago   Running   local-path-provisioner   0          e515f3ee68e83
```

#### Interfacing To The k3s Server

To command to the server via [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/),

```
# on the host where k3d is installed
$ kubectl --kubeconfig=$(k3d get-kubeconfig --name=waggle-k3s) get po --all-namespaces
NAMESPACE     NAME                                      READY   STATUS      RESTARTS   AGE
kube-system   metrics-server-6d684c7b5-ttzx9            1/1     Running     0          4h41m
kube-system   local-path-provisioner-58fb86bdfd-j8tl4   1/1     Running     0          4h41m
kube-system   helm-install-traefik-xs4pt                0/1     Completed   0          4h41m
kube-system   svclb-traefik-md254                       2/2     Running     0          4h41m
kube-system   coredns-d798c9dd-lxc2v                    1/1     Running     0          4h41m
kube-system   traefik-6787cddb4b-6zlgg                  1/1     Running     0          4h41m
```

Or use Python client for Kubernetes APIs (See [examples](https://github.com/kubernetes-client/python#examples))

#### Simple Deployment in A k3s

```
# export KUBECONFIG="$(k3d get-kubeconfig --name='waggle-k3s')"
$ cat k3s_example.yaml 
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:1.14.2
        ports:
        - containerPort: 80
        name: nginx
$ kubectl apply -f k3s_example.yaml 
deployment.apps/nginx-deployment created
$ kubectl get deployment
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   2/2     2            2           105s
```

