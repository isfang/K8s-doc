登录master:
ssh dong@127.0.0.1 -p 9091




1.添加公钥
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -


2.添加源
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

将在 /etc/apt/sources.list中 添加官方源

3.可执行 apt-get update 更新下本地包索引

4.install docker 可执行 
apt-get install docker-ce=17.03.2~ce-0~ubuntu-xenial   


----------------


安装k8s环境

kubeadm将k8s组建以容器化的方式安装运行
node中的kubelet
kubectl 至少存在于master上 管理集群

1. 添加 kubenetes 源
先拿到公钥
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
再加源
$ cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
> deb http://apt.kubernetes.io/ kubernetes-xenial main
> EOF

# apt-get update

安装工具
# apt-get install -y kubelet kubeadm kubectl
# apt-get install -y kubelet=1.10.2-00 kubeadm=1.10.2-00 kubectl=1.10.2-00

------------------------


下载k8s核心组件
--to do

初始化 master 节点

1.关闭swap
vi /etc/fstab
注释swap



kubeadm config images pull
2.kubeadm init --apiserver-advertise-address=10.0.2.15 --pod-network-cidr=192.168.16.0/20

kubeadm join 10.0.2.15:6443 --token cxxis4.km9xv43ckx1yc8yn \
    --discovery-token-ca-cert-hash sha256:0d578f501a145cd372e9346b8d4161cf92b8bc0eb53186356b8bf35102fa21c6


失败了  重新开始可以用
kubeadm reset  或者 加 --ignore-preflight-errors=all  这个命令

docker 代理
vi /etc/systemd/system/docker.service.d/http-proxy.conf
vi /etc/systemd/system/docker.service.d/https-proxy.conf


3.export KUBECONFIG=/etc/kubernetes/admin.conf
kubeadm config images
kubectl get pods -n kube-system -o -wide


4. 安装成功以后 安装 weave网络节点
执行curl -L "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')" > weave.yaml

vi weave.yaml 修改：IPALLOC_RANGE
在 weave-net 节点下 添加
                - name: IPALLOC_RANGE
                  value: 192.168.16.0/20

执行 kubectl apply -f weave.yaml  运行该容器


