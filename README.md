# ose-rhel-installer
1. OSE can be deployed in two type of RHEL images:  
a. RHEL Atomic Image  
b. RHEL Standard Image

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






