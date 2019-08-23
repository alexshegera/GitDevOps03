Teamcity:
Server:
docker run -it --name teamcity-server-instance -v /home/alex/ITEA/GitDevOps03/lesson4/teamcity-server/data:/data/teamcity_server/datadir -v /home/alex/ITEA/GitDevOps03/lesson4/teamcity-server/logs:/opt/teamcity/logs -p 8111:8111 -d jetbrains/teamcity-server
Client:
docker run -it -e SERVER_URL="172.17.0.2:8111" -d -v /home/alex/ITEA/GitDevOps03/lesson4/teamcity-agent/conf:/data/teamcity_agent/conf -v /var/run/docker.sock:/var/run/docker.sock --name teamcity-agent-1 jetbrains/teamcity-agent

Nexus Server:
docker run -p 8081:8081 -d --name nexus-server sonatype/nexus3

Ansible:
sudo apt install ansible
sudo nano /etc/ansible/hosts
sudo nano /etc/ansible/ansible.cfg
ansible servers -a uptime
ansible servers -b -m yum -a "name=httpd state=latest"
ansible servers -b -m yum -a "name=httpd state=running"

Installing Docker:
sudo apt update && sudo apt install apt-transport-https ca-certificates curl software-properties-common && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" && sudo apt update && apt-cache policy docker-ce && sudo apt install docker-ce && sudo systemctl status docker && sudo usermod -aG docker alex && docker

Installing Kubernetes:
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --ignore-preflight-errors=NumCPU
sudo mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config


kubeadm join 192.168.100.6:6443 --token 06tl4c.oqn35jzecidg0r0m --discovery-token-ca-cert-hash sha256:c40f5fa0aba6ba311efcdb0e8cb637ae0eb8ce27b7a03d47be6d966142f2204c




