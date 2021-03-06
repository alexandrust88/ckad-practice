18% - Configuration 
* Understanding Configmaps
* Understand SecurityContexts
* Define application resource requirements
* Create and consume secrets
* Understand service accounts
### Bookmark below

#### create a new service account named ckad in namespace ckadns
```bash
k create ns ckadns
k create sa ckad -n ckadns
```
#### create a nginx pod with serviceaccount as ckad and in namespace ckadns
```bash
k run mypod --image=nginx --restart=Never --serviceaccount=ckad -n=ckadns 
```
#### try changing the service account of the above pod to default
```bash
k set serviceaccount po/mypod default -n ckadns  
## **** NOTE: you wont be able to change serviceaccounts on pod as they are immutable but if you have a deployment it would work
```

#### Changing serviceaccount on deployment will work with k set serviceaccount 
```bash
k run mydeploy --image=nginx --replicas=2 --serviceaccount=ckad -n ckadns
k get po -n ckadns
k get po <po_name> -n ckadns -o yaml  # notice the servcieaccount on pod spec level would be ckad
k set serviceaccount deploy/mydeploy default -n ckadns # changing service account on deployment level to ckad 
k get po <new_Po_name> -n ckadns # notice the service account on pod spec level would be default
```