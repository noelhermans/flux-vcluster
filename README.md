# flux-vcluster
Multicluster Flux with vclusters


## Steps 

### 1. Build vm on hetzner
```shell
cd ~/hetzner-cloud-docker
tofu init
tofu apply
ssh@10.10.10.10
```

### 2. install pre-reqs

#### Install go
```shell
wget https://go.dev/dl/go1.19.2.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.19.2.linux-amd64.tar.gz

vi ~/.profile 
export PATH=$PATH:/usr/local/go/bin

vi ~/.bashrc
export GOROOT=/usr/local/go
export PATH=${GOROOT}/bin:${PATH}
export GOPATH=$HOME/go
export PATH=${GOPATH}/bin:${PATH}

source ~/.profile 
go version
```

#### Clone repo
```shell
git clone https://github.com/noelhermans/flux-vcluster.git
cd flux-vcluster


#### Install tools
```shell
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

curl -sS https://webinstall.dev/k9s | bash
```

### 3. Deploy everything
```shell
export GITHUB_TOKEN="<your github PAT"
make install
```

### 4. Checks
```shell
kubectl get gitrepo -A
kubectl get hr -A
kubectl get kustomization -A
```

### 5. Connect to vlcusters
```shell
$ make vctx
Switched to context "kind-host-cluster".
/home/davar/Downloads/kind-vcluster-flux-playground/bin/vcluster connect vcluster-a -n vcluster-a
info   Using vcluster vcluster-a load balancer endpoint: 172.17.0.210
done √ Switched active kube context to vcluster_vcluster-a_vcluster-a_kind-host-cluster
- Use `vcluster disconnect` to return to your previous kube context
- Use `kubectl get namespaces` to access the vcluster
/home/davar/Downloads/kind-vcluster-flux-playground/bin/vcluster connect vcluster-b -n vcluster-b
info   Using vcluster vcluster-b load balancer endpoint: 172.17.0.211
done √ Switched active kube context to vcluster_vcluster-b_vcluster-b_kind-host-cluster
- Use `vcluster disconnect` to return to your previous kube context
- Use `kubectl get namespaces` to access the vcluster


$ ./bin/vcluster connect vcluster-a -n vcluster-a
info   Using vcluster vcluster-a load balancer endpoint: 172.17.0.210
done √ Switched active kube context to vcluster_vcluster-a_vcluster-a_kind-host-cluster
- Use `vcluster disconnect` to return to your previous kube context
- Use `kubectl get namespaces` to access the vcluster

$ kubectl get namespaces
NAME              STATUS   AGE
kube-system       Active   4m48s
default           Active   4m48s
kube-public       Active   4m48s
kube-node-lease   Active   4m48s

$ kubectl get po --all-namespaces
NAMESPACE     NAME                       READY   STATUS    RESTARTS   AGE
kube-system   coredns-68559449b6-cltc8   1/1     Running   0          16m
default       nginx-77b4fdf86c-7lts5     1/1     Running   0          13m

./bin/vcluster disconnect

$ ./bin/vcluster connect vcluster-b -n vcluster-b
info   Using vcluster vcluster-b load balancer endpoint: 172.17.0.211
done √ Switched active kube context to vcluster_vcluster-b_vcluster-b_kind-host-cluster
- Use `vcluster disconnect` to return to your previous kube context
- Use `kubectl get namespaces` to access the vcluster

$ kubectl get namespaces
NAME              STATUS   AGE
default           Active   6m10s
kube-system       Active   6m10s
kube-public       Active   6m10s
kube-node-lease   Active   6m10s

$ kubectl get po --all-namespaces
NAMESPACE     NAME                       READY   STATUS    RESTARTS   AGE
kube-system   coredns-68559449b6-6m28q   1/1     Running   0          17m
default       nginx-77b4fdf86c-9hrzh     1/1     Running   0          14m

./bin/vcluster disconnect
```



