# nfs-provisioner
NFS provisioner for dynamic storage provisioning.

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
```
oc create -f https://raw.githubusercontent.com/aroute/nfs-provisioner/master/deploy-provisioner.yaml
```
## Create a Claim (test)
```

```
## Create a Pod (test)
```

```
