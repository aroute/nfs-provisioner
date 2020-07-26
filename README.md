# NFS Server with Provisioner
NFS server and provisioner for dynamic storage provisioning.

## Deploy NFS
Uses `/var/nfsshare` as a primary NFS shared location. Uses `/var/nfsshare/registry` for the registry. Replace your network address and adjust accordingly.
```
sudo systemctl enable nfs-server rpcbind
sudo systemctl start nfs-server rpcbind
sudo mkdir -p /var/nfsshare/registry
sudo chmod -R 777 /var/nfsshare
sudo chown -R nobody:nobody /var/nfsshare
echo '/var/nfsshare <x.x.x.0>/24(rw,sync,no_root_squash,no_all_squash,no_wdelay)' | sudo tee /etc/exports
sudo setsebool -P nfs_export_all_rw 1
echo '/var/nfsshare <x.x.x.0>/24(rw,sync,no_root_squash,no_all_squash,no_wdelay)' | sudo tee /etc/exports
sudo systemctl restart nfs-server
sudo firewall-cmd --permanent --zone=public --add-service mountd
sudo firewall-cmd --permanent --zone=public --add-service rpc-bind
sudo firewall-cmd --permanent --zone=public --add-service nfs
sudo firewall-cmd --reload
```

## Create a new project
```
oc new-project nfs-provisioner
```
## Service Account and Security
```
oc create -f https://raw.githubusercontent.com/aroute/nfs-provisioner/master/nfs-provisioner.yaml
```
## Add policy
```
oc adm policy add-scc-to-user hostmount-anyuid system:serviceaccount:nfs-provisioner:nfs-client-provisioner
```
## Deploy Storageclass
```
oc create -f https://raw.githubusercontent.com/aroute/nfs-provisioner/master/storageclass.yaml
```
## Deploy Provisioner
Insert the IP address of your NFS server before applying.
```
oc create -f https://raw.githubusercontent.com/aroute/nfs-provisioner/master/deploy-provisioner.yaml
```
## Create a Claim (test)
```
oc create -f https://raw.githubusercontent.com/aroute/nfs-provisioner/master/test-claim.yaml
```
## Create a Pod (test)
```
oc create -f https://raw.githubusercontent.com/aroute/nfs-provisioner/master/test-pod.yaml
```
## Verify
```
oc get pvc
ls -l /var/nfsshare/nfs-provisioner-test-claim-pvc-xxxxxx
SUCCESS
```
