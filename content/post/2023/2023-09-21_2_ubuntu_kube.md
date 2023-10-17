---
title: Virtualbox Ubuntu 22.04에 Kubernetes Cluster 설치하는 법
description: How to set Kubernetes Cluster to Ubuntu 22.04 in Virtualbox
slug: ubuntu_kube_cluster_setup
date: 2023-09-21 00:00:00+0000
image: 
tags:
    - Ubuntu
    - Virtualbox
    - Kubernetes
---

**긴급 글 복구 중**

쿠버네티스 설치하실 정도면 어느 정도는 기본기가 있으신 분이라고 가정하고 그냥 설명하겠습니다.

## Virtualbox에 Ubuntu 설치

다른 블로그의 글을 참조하시길 바랍니다.

## 네트워크 설정

[https://gonghakjoa.tistory.com/134](https://gonghakjoa.tistory.com/134)

네트워크 설정을 어떻게 해야할지 모르겠다면 위 글을 참고하세요.

**저는 호스트 아이피 대역이 192.168.0.0/24라서,**  
**게스트 아이피는 호스트 아이피 대역 내인 192.168.0.10과 20으로 설정했습니다.**

<span style="color:blueviolet"> **버추얼박스 자체 네트워크 설정은 아래와 같이 합니다.** </span>  
<span style="background-color:khaki"> **\[NAT Networks\]** </span>  
**Name : NatNetwork**  
**IP Prefix : 192.168.0.0/24**  
**Enable DHCP**  

<span style="color:blueviolet"> **우분투가 설치되어있는 가상 머신 자체의 설정은 다음과 같습니다.** </span>  
<span style="background-color:khaki"> **\[어댑터 1\]** </span>  
**NAT 네트워크**  
**이름 : NatNetwork**

<span style="color:blueviolet"> **우분투 내부에서의 IP 설정은 다음과 같습니다.** </span>  
**IPv4 Method : Manual(수동)**  
**IP : 192.168.0.10**  
**SM : 255.255.255.0**  
**GW : 192.168.0.1**  
**DNS : 8.8.8.8**


## Docker 설치

[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

터미널을 열고 다음 명령어를 실행합니다.

```bash
sudo su

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg -y
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
swapoff -a & sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```


## 쿠버네티스 설치

[https://kubernetes.io/docs/setup/production-environment/container-runtimes/](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf  
overlay  
br_netfilter  
EOF  
sudo modprobe overlay  
sudo modprobe br_netfilter  
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf  
net.bridge.bridge-nf-call-iptables  = 1  
net.bridge.bridge-nf-call-ip6tables = 1  
net.ipv4.ip_forward                 = 1  
EOF  
sudo sysctl --system
```

[https://pwittrock.github.io/docs/setup/independent/install-kubeadm/](https://pwittrock.github.io/docs/setup/independent/install-kubeadm/)
  
```bash
apt-get update && apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm
```

'''
vi /etc/hosts
'''

아래의 두 줄을 파일에 추가하고 저장
```
192.168.0.10 master
192.168.0.20 worker
```

다했으면 마스터 노드를 종료합니다.

## master 노드 복제

복제할 때 이름은 worker로 해줍니다.

복제 방법은 아래를 참고해주세요.

[https://sidepower.tistory.com/511](https://sidepower.tistory.com/511)
  
  
## worker 노드 IP, 호스트명 설정

<span style="color:blueviolet"> **worker 노드를 실행한 뒤 다음과 같이 설정합니다.** </span>  
**IPv4 Method : Manual**  
**IP : 192.168.0.20**  
**SM : 255.255.255.0**  
**GW : 192.168.0.1**  

사실 그냥 IP 끝자리만 바꿔준 겁니다. master node와 중복되면 안되니까요.

마찬가지로 호스트명도 중복되면 안 되므로 터미널에서 다음 명령어를 실행해줍니다.

```bash
hostnamectl set-hostname worker
```

그리고 재부팅합니다.

## Kubernetes Cluster 설정

<span style="color:blueviolet"> **master node VM을 실행합니다.** </span>  
[https://velog.io/@makengi/K8s-%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0-%EA%B5%AC%EC%84%B1%EC%A4%91-%EC%97%90%EB%9F%AC](https://velog.io/@makengi/K8s-%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0-%EA%B5%AC%EC%84%B1%EC%A4%91-%EC%97%90%EB%9F%AC)

kubeadm init 과정 중 에러가 발생하기 때문에 아래 명령어를 실행해줘야 합니다.

```bash
sudo su

swapoff -a

rm /etc/containerd/config.toml  
systemctl restart containerd

kubeadm init --control-plane-endpoint=master
```

시간이 오래 걸립니다. 멈춘 거 아니에요ㅋㅋ

다 끝나면 **kubeadm join** 어쩌고 하는 내용이 맨 뒤에 나오는데, 이를 복사해줍니다.

cat > token.txt 명령어를 사용해 따로 저장해주면 더욱 좋습니다.

터미널에 다음 명령어를 실행합니다.

```bash
export KUBECONFIG=/etc/kubernetes/admin.conf
export KUBECONFIG=$HOME/.kube/config

exit
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
export KUBECONFIG=/etc/kubernetes/admin.conf
export KUBECONFIG=$HOME/.kube/config
```

## kubeadm join 설정

<span style="color:blueviolet"> **이제 worker node VM으로 건너갑니다.** </span>

```bash
sudo su

rm /etc/containerd/config.toml
systemctl restart containerd
swapoff -a
```

그리고 터미널에 아까 복사했던 kubeadm join 어쩌고 하는 값을 그대로 붙여넣고 실행합니다.


## Pod Network Addon 설정

<span style="color:blueviolet"> **다시 master node VM로 건너가서,** </span>
여기에 pod network addon을 설치해주어야 합니다.
저는 Weave-net을 사용해주었습니다.

[https://www.weave.works/docs/net/latest/kubernetes/kube-addon/](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/)

일반 사용자 모드에서 다음 명령어를 수행합니다.

```bash
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```
  
  이제 제대로 됐는지 확인해봅니다.

```bash
kubectl get nodes
```

ready 상태로 변환되기까지 시간이 좀 걸립니다. 기다려주세요.