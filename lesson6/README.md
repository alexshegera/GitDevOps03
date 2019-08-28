Kubernetes:

** Add Xenial Kubernetes Repository on both the nodes:
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

** Install Kubeadm:
sudo apt install kubeadm

** Disable swap memory (if running) on both the nodes
sudo swapoff -a

** Give Unique hostnames to each node
sudo hostnamectl set-hostname master-node
hostnamectl set-hostname slave-node

** Initialize Kubernetes on the master node
sudo kubeadm init --pod-network-cidr=10.244.0.0/16  --ignore-preflight-errors=NumCPU

** To start using your cluster, you need to run the following as a regular user:
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

sudo kubeadm join 192.168.100.6:6443 --token 06tl4c.oqn35jzecidg0r0m --discovery-token-ca-cert-hash sha256:c40f5fa0aba6ba311efcdb0e8cb637ae0eb8ce27b7a03d47be6d966142f2204c 

** add security group to Master ec2:
Port range: 0 - 6555 or just 6443

sudo kubectl get nodes

** Work with nodes:
kubectl taint nodes --all node-role.kubernetes.io/master-
kubectl label node {NODE_NAME} node-role.kubernetes.io/node1=node1 --kubeconfig=/etc/kubernetes/kubelet.conf

** Namespaces:
kubectl create namespace devops03
kubectl apply -f ns.yaml
kubectl apply -f pod.yaml
kubectl run lab-pod --image=nginx --port=80 --labels="app=frontend"
kubectl apply -f deployment.yaml


** ns.yaml
apiVersion: v1
kind: Namespace
metadata:
   name: ITEA


** pod.yaml
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod
    labels:
       app: web
spec:
     containers:
        - name: nginx-container
          image: nginx

** deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80


