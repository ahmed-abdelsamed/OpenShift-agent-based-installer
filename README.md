# OpenShift-agent-based-installer


After create  install-config and agent-config files

$ ./openshift-install agent create image --dir ocp

INFO[0000] Start configuring static network for 3 hosts  pkg=manifests
INFO[0000] Adding NMConnection file <enp2s0.nmconnection>  pkg=manifests
INFO[0001] Adding NMConnection file <enp2s0.nmconnection>  pkg=manifests
INFO[0001] Adding NMConnection file <enp2s0.nmconnection>  pkg=manifests
INFO[0001] Extracting base ISO from release payload     
INFO Consuming Install Config from target directory 
INFO Consuming Agent Config from target directory 

attach  iso on three VMs (masters)

Running and Watching the Deployment
Now lets go ahead and start the virtual machines all at the same time from the hypervisor host:

--------------------------------------------------------------------------------------------
 export KUBECONFIG=/home/bschmaus/installer/kni-22/auth/kubeconfig

$ ./openshift-install agent wait-for install-complete --dir ocp

INFO Waiting for cluster install to initialize. Sleeping for 30 seconds 
INFO Waiting for cluster install to initialize. Sleeping for 30 seconds 
INFO Waiting for cluster install to initialize. Sleeping for 30 seconds 
INFO Checking for validation failures ---------------------------------------------- 
ERROR Validation failure found for asus3-vm1        category=network label=Belongs to majority connected group message=No connectivity to the majority of hosts in the cluster
ERROR Validation failure found for asus3-vm1        category=network label=DNS wildcard not configured message=Parse error for domain name resolutions result
ERROR Validation failure found for asus3-vm1        category=network label=NTP synchronization message=Host couldn't synchronize with any NTP server
ERROR Validation failure found for asus3-vm2        category=network label=Belongs to majority connected group message=No connectivity to the majority of hosts in the cluster
ERROR Validation failure found for asus3-vm2        category=network label=DNS wildcard not configured message=Parse error for domain name resolutions result
ERROR Validation failure found for asus3-vm2        category=network label=NTP synchronization message=Host couldn't synchronize with any NTP server
ERROR Validation failure found for asus3-vm3        category=network label=Belongs to majority connected group message=No connectivity to the majority of hosts in the cluster
ERROR Validation failure found for asus3-vm3        category=network label=DNS wildcard not configured message=Parse error for domain name resolutions result
ERROR Validation failure found for asus3-vm3        category=network label=NTP synchronization message=Host couldn't synchronize with any NTP server
ERROR Validation failure found for cluster          category=hosts-data label=all-hosts-are-ready-to-install message=The cluster has hosts that are not ready to install.
INFO Checking for validation failures ---------------------------------------------- 
ERROR Validation failure found for asus3-vm1        category=network label=Belongs to majority connected group message=No connectivity to the majority of hosts in the cluster
ERROR Validation failure found for asus3-vm2        category=network label=Belongs to majority connected group message=No connectivity to the majority of hosts in the cluster
ERROR Validation failure found for asus3-vm3        category=network label=Belongs to majority connected group message=No connectivity to the majority of hosts in the cluster
ERROR Validation failure found for cluster          category=hosts-data label=all-hosts-are-ready-to-install message=The cluster has hosts that are not ready to install.
INFO Checking for validation failures ---------------------------------------------- 
ERROR Validation failure found for cluster          category=hosts-data label=all-hosts-are-ready-to-install message=The cluster has hosts that are not ready to install.
INFO Pre-installation validations are OK          
INFO Cluster is ready for install                 
INFO Host asus3-vm1: updated status from insufficient to known (Host is ready to be installed) 
INFO Preparing cluster for installation           
INFO Host asus3-vm3: updated status from known to preparing-for-installation (Host finished successfully to prepare for installation) 
INFO Host asus3-vm1: New image status quay.io/openshift-release-dev/ocp-v4.0-art-dev@sha256:b6f2a69fdc1a0844565320fc51316aa79ad6d4661326b30fa606123476c3d9f7. result: success. time: 0.83 seconds; size: 378.98 Megabytes; download rate: 479.70 MBps 
INFO Host asus3-vm1: updated status from preparing-for-installation to preparing-successful (Host finished successfully to prepare for installation) 
INFO Cluster installation in progress             
INFO Host asus3-vm3: updated status from preparing-successful to installing (Installation is in progress) 
INFO Host: asus3-vm2, reached installation stage Writing image to disk 
INFO Host: asus3-vm1, reached installation stage Writing image to disk 
INFO Host: asus3-vm2, reached installation stage Writing image to disk: 24% 
INFO Host: asus3-vm2, reached installation stage Writing image to disk: 30% 
INFO Host: asus3-vm3, reached installation stage Writing image to disk: 45% 
INFO Host: asus3-vm1, reached installation stage Writing image to disk: 43% 
INFO Host: asus3-vm1, reached installation stage Writing image to disk: 57% 
INFO Host: asus3-vm1, reached installation stage Writing image to disk: 62% 
INFO Host: asus3-vm1, reached installation stage Writing image to disk: 72% 
INFO Host: asus3-vm1, reached installation stage Writing image to disk: 77% 
INFO Host: asus3-vm1, reached installation stage Writing image to disk: 85% 
INFO Host: asus3-vm2, reached installation stage Writing image to disk: 100% 
INFO Host: asus3-vm2, reached installation stage Rebooting 
INFO Host: asus3-vm1, reached installation stage Waiting for control plane: Waiting for masters to join bootstrap control plane 
INFO Cluster Kube API Initialized                 
INFO Host: asus3-vm2, reached installation stage Configuring 
INFO Host: asus3-vm2, reached installation stage Joined 
INFO Host: asus3-vm1, reached installation stage Waiting for bootkube 
INFO Host: asus3-vm3, reached installation stage Done 
INFO Host: asus3-vm1, reached installation stage Waiting for controller: waiting for controller pod ready event 
INFO Bootstrap configMap status is complete       
INFO cluster bootstrap is complete                
INFO Cluster is installed                         
INFO Install complete!                            
INFO To access the cluster as the system:admin user when using 'oc', run 
INFO     export KUBECONFIG=/home/bschmaus/installer/kni-22/auth/kubeconfig 
INFO Access the OpenShift web-console here: https://console-openshift-console.apps.kni22.schmaustech.com 
Once the cluster installation has completed we can run a few commands to validate that indeed the cluster is up and operational:

$ oc get nodes

NAME        STATUS   ROLES           AGE   VERSION
asus3-vm1   Ready    master,worker   16m   v1.24.0+9546431
asus3-vm2   Ready    master,worker   30m   v1.24.0+9546431
asus3-vm3   Ready    master,worker   30m   v1.24.0+9546431

$ oc get co

NAME                                       VERSION       AVAILABLE   PROGRESSING   DEGRADED   SINCE   MESSAGE
authentication                             4.11.0        True        False         False      4m38s   
baremetal                                  4.11.0        True        False         False      28m     
cloud-controller-manager                   4.11.0        True        False         False      30m     
cloud-credential                           4.11.0        True        False         False      33m     
cluster-autoscaler                         4.11.0        True        False         False      28m     
config-operator                            4.11.0        True        False         False      29m     
console                                    4.11.0        True        False         False      11m     
csi-snapshot-controller                    4.11.0        True        False         False      29m     
dns                                        4.11.0        True        False         False      29m     
etcd                                       4.11.0        True        False         False      27m     
image-registry                             4.11.0        True        False         False      14m     
ingress                                    4.11.0        True        False         False      20m     
insights                                   4.11.0        True        False         False      52s     
kube-apiserver                             4.11.0        True        False         False      25m     
kube-controller-manager                    4.11.0        True        False         False      26m     
kube-scheduler                             4.11.0        True        False         False      25m     
kube-storage-version-migrator              4.11.0        True        False         False      29m     
machine-api                                4.11.0        True        False         False      25m     
machine-approver                           4.11.0        True        False         False      28m     
machine-config                             4.11.0        True        False         False      28m     
marketplace                                4.11.0        True        False         False      29m     
monitoring                                 4.11.0        True        False         False      13m     
network                                    4.11.0        True        False         False      30m     
node-tuning                                4.11.0        True        False         False      28m     
openshift-apiserver                        4.11.0        True        False         False      14m     
openshift-controller-manager               4.11.0        True        False         False      25m     
openshift-samples                          4.11.0        True        False         False      20m     
operator-lifecycle-manager                 4.11.0        True        False         False      29m     
operator-lifecycle-manager-catalog         4.11.0        True        False         False      29m     
operator-lifecycle-manager-packageserver   4.11.0        True        False         False      23m     
service-ca                                 4.11.0        True        False         False      29m     
storage                                    4.11.0        True        False         False      29m 

--------------------------------------------------
$ oc patch secret -n kube-system kubeadmin --type json -p '[{"op": "replace", "path": "/data/kubeadmin", "value": "'"$(openssl rand -base64 18 | tee auth/kubeadmin-password | htpasswd -nBi -C 10 "" | cut -d: -f2 | base64 -w 0 -)"'"}]'

secret/kubeadmin patched

$ ls -l auth/kubeadmin-password 
-rw-r-----. 1 bschmaus bschmaus 25 Aug 18 10:50 auth/kubeadmin-password

$ cat auth/kubeadmin-password
VdCOu3+DVfE3J2jurERXFxf1

----------------------------------------------------------
https://cloud.redhat.com/blog/meet-the-new-agent-based-openshift-installer-1

