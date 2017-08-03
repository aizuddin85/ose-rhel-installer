# ose-rhel-installer
1. OSE can be deployed in two type of RHEL images:  
a. RHEL Atomic Image  
b. RHEL Standard Image (Minimal OS)

2. And there are two types of OSE deployments:  
a. RPM  
b. Containerized  

In this guide, we will start with the standard image + containerized deployment.  
When planning the deployment, below limit must be taken into considerations:

Maximum nodes per cluster => 1000  
Maximum pods per cluster => 120,000  
Maximum pods per nodes => 250  
Maximum pods per code => 10

Original doc: https://access.redhat.com/documentation/en-us/openshift_container_platform/3.4/html-single/installation_and_configuration/#inital-planning 

In this example , we are going to deploy all-in-one OSE.

## Pre-Req
1. DNS  
a. An A+PTR of the master (ose-master.example.com)  
b. An CNAME for the subdomain (*.cloudapp.example.com) to the ose-master.example.com.   

2. Host Registration.  
```
#> subscription-manager register --username=<user_name> --password=<password>  
#> subscription-manager list --available --matches '*OpenShift*'  
#> subscription-manager attach --pool=<pool_id>  
#> subscription-manager repos --disable="*"  
#> subscription-manager repos \
    --enable="rhel-7-server-rpms" \
    --enable="rhel-7-server-extras-rpms" \
    --enable="rhel-7-server-ose-3.4-rpms  
```

3. Base Packages  
```
yum install wget git net-tools bind-utils iptables-services bridge-utils bash-completion kexec-tools sos psacct
yum update
```  

4. OSE Installer  
```
yum install atomic-openshift-utils
```

5. OSE Excluder (helps ensure your systems stay on the correct versions of atomic-openshift and docker packages when you are not trying to upgrade, according to the OpenShift Container Platform version, but later in the configuration the docker_excluder sets to False or else installation will failed, need further digging."
```
yum install atomic-openshift-excluder atomic-openshift-docker-excluder
```


6. Docker  
```
yum install docker
```

7. Add insecure registry:
```
OPTIONS='--selinux-enabled --insecure-registry 172.30.0.0/16'
```


8. There are many ways of configuring docker storage (this is ephemeral and separated from any persistent storage). Make sure the LVM has free space and run:  
```
#> docker-storage-setup
```  


## Installation
The example of advanced installation is located in this project with the filename of advanced-ose-rhel.yml.  
Make any adjustment needed, the full example can be located https://github.com/openshift/openshift-ansible/blob/master/inventory/byo/hosts.ose.example.  

```
ansible-playbook -i advanced-ose-rhel.yml /usr/share/ansible/openshift-ansible/playbooks/byo/config.yml -vv
```


Once the installation completed you need to:  
1. Generate the htpass
```
htpasswd -c /etc/origin/master/htpasswd userid
```  

2. Login via oc or console (https://ose-master.example.com:8443)



## Uninstallation
```
ansible-playbook -i advance-ose-rhel.yml /usr/share/ansible/openshift-ansible/playbooks/adhocs/uninstall.yml -vv
```





