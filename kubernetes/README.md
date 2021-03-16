# Kubernetes

1. On that script, we'll have 3 VMs running the kubernetes service. You must grant that the VMs are seeing each other and comunicating to internet, also.

    1. You may set the VirtualBox connection on VMs to Bridge and set your VMs OS to the same gateway and IP range.

    1. /etc/sysconfig/network-scripts/ifcfg-enp0s3

        <code>
            NM_CONTROLLED="yes"
            DEVICE="enp0s3"
            BOOTPROTO=static
            ONBOOT="yes"
            IPADDR=192.168.111.193
            NETMASK=255.255.255.0
            GATEWAY=192.168.111.254
            DNS1=8.8.8.8
        </code>

1. Update your repositories with ***dnf update*** or any other repo manager you're using;

1. Install the following packages: kubectl, minikube, kubeadm e kubelet (prefer not install using snap):
    - https://kubernetes.io/docs/tasks/tools/

1. dnf install docker socat conntrack ebtables ipset -y

1. systemctl enable docker.service

1. systemctl enable kubelet.service

1. Desativar swap:

    1. ***swapon -a*** to show active swaps

    1. ***systemctl mask dev-zram0.swap*** replacing the dev-zram0 with the name of your swap file

    1. reboot

1. Initialize your kube host: ***kubeadm init --apiserver-advertise-address MASTER_IP***

    1. If you have to init again the kube, run ***kubeadm reset*** in order to fix it

    1. If you're having the cgroups problem, add to the file ***/etc/systemd/system/kubelet.service.d/11-cgroups.conf*** the following:

        <code>
            [Service]
            CPUAccounting=true
            MemoryAccounting=true
        </code>

        1. restart with ***systemctl daemon-reload && systemctl restart kubelet***

    1. If you're having the Docker daemon communication problem, add/create the file ***/etc/systemd/system/docker.service.d/hosts.conf*** and add it:

        <code>
           [Service]
            ExecStart=
            ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2736 
        </code>

        1. restart the daemon and the docker.service with ***systemctl daemon-reload && systemctl restart docker.service***





????/

1. /etc/selinux/config -> SELINUX=enforcing to SELINUX=disabled

1. /var/lib/kubelet/config.yaml -> cgroupDriver: cgroupfs

1. ***

1. ***

1. ***

1. ***

1. ***



1. ***kubectl get nodes***

1. ***kubectl run nginx --image***

1. ***kubectl get deployment***

1. ***kubectl get pods***