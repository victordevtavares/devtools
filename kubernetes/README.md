# Kubernetes

1. On that script, we'll have 3 VMs running the kubernetes service. You must grant that the VMs are seeing each other and comunicating to internet, also.

    1. You may set the VirtualBox connection on VMs to Bridge and set your VMs OS to the same gateway and IP range.

    1. /etc/sysconfig/network-scripts/ifcfg-enp0s3

        <code>
            NM_CONTROLLED="yes"
            DEVICE="enp0s3"
            BOOTPROTO=static
            ONBOOT="yes"
            IPADDR=192.168.1.254
            NETMASK=255.255.255.0
            GATEWAY=192.168.1.255
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

OBS: This install in fedora is not finished! Lets finish it later!

????/

1. /etc/selinux/config -> SELINUX=enforcing to SELINUX=disabled

1. /var/lib/kubelet/config.yaml -> cgroupDriver: cgroupfs




1. On the next script, we'll deploy a cluster on Google Cloud Platform using GCP features:


Task 2: Confirm that needed APIs are enabled
Make a note of the name of your GCP project. This value is shown in the top bar of the Google Cloud Platform Console. It will be of the form qwiklabs-gcp- followed by hexadecimal numbers.

In the GCP Console, on the Navigation menu (Navigation menu), click APIs & Services.

Scroll down in the list of enabled APIs, and confirm that both of these APIs are enabled:

Kubernetes Engine API
Container Registry API

If either API is missing, click Enable APIs and Services at the top. Search for the above APIs by name and enable each for your current project. (You noted the name of your GCP project above.)

Task 3: Start a Kubernetes Engine cluster
In GCP console, on the top right toolbar, clicke, you mentioned you reset it, but here is a link 

Click Continue. cloudshell_continue.png

For convenience, place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE. At the Cloud Shell prompt, type this partial command:

***export MY_ZONE=***
followed by the zone that Qwiklabs assigned to you. Your complete command will look similar to this:

***export MY_ZONE=us-central1-a***
Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:

***gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2***

It takes several minutes to create a cluster as Kubernetes Engine provisions virtual machines for you.

After the cluster is created, check your installed version of Kubernetes using the kubectl version command:

***kubectl version***
The gcloud container clusters create command automatically authenticated kubectl for you.

View your running nodes in the GCP Console. On the Navigation menu (Navigation menu), click Compute Engine > VM Instances.

Your Kubernetes cluster is now ready for use.

Click Check my progress to verify the objective.
Start a Kubernetes Engine cluster

Task 4: Run and deploy a container
From your Cloud Shell prompt, launch a single instance of the nginx container. (Nginx is a popular web server.)

***kubectl create deploy nginx --image=nginx:1.17.10***

In Kubernetes, all containers run in pods. This use of the kubectl create command caused Kubernetes to create a deployment consisting of a single pod containing the nginx container. A Kubernetes deployment keeps a given number of pods up and running even in the event of failures among the nodes on which they run. In this command, you launched the default number of pods, which is 1.

Note: If you see any deprecation warning about future version you can simply ignore it for now and can proceed further.

View the pod running the nginx container:

***kubectl get pods***

Expose the nginx container to the Internet:

***kubectl expose deployment nginx --port 80 --type LoadBalancer***

Kubernetes created a service and an external load balancer with a public IP address attached to it. The IP address remains the same for the life of the service. Any network traffic to that public IP address is routed to pods behind the service: in this case, the nginx pod.

View the new service:

***kubectl get services***

You can use the displayed external IP address to test and contact the nginx container remotely.

It may take a few seconds before the External-IP field is populated for your service. This is normal. Just re-run the kubectl get services command every few seconds until the field is populated.

Open a new web browser tab and paste your cluster's external IP address into the address bar. The default home page of the Nginx browser is displayed.

Scale up the number of pods running on your service:

***kubectl scale deployment nginx --replicas 3***

Scaling up a deployment is useful when you want to increase available resources for an application that is becoming more popular.

Confirm that Kubernetes has updated the number of pods:

***kubectl get pods***

Confirm that your external IP address has not changed:

***kubectl get services***

Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.


















1. ***kubectl get nodes***

1. ***kubectl run nginx --image***

1. ***kubectl get deployment***

1. ***kubectl get pods***