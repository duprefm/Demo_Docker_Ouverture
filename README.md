# Demo_Docker_Ouverture
## Minikube.
Cluster local Kubernetes utilisé dans le cadre de phases de tests ou de développement.
### Démarrage.
``````````````````
🐳 minikube start
😄  minikube v1.0.1 on darwin (amd64)
🤹  Downloading Kubernetes v1.14.1 images in the background ...
💡  Tip: Use 'minikube start -p <name>' to create a new cluster, or 'minikube delete' to delete this one.
🔄  Restarting existing virtualbox VM for "minikube" ...
⌛  Waiting for SSH access ...
📶  "minikube" IP address is 192.168.99.106
🐳  Configuring Docker as the container runtime ...
🐳  Version of container runtime is 18.06.3-ce
⌛  Waiting for image downloads to complete ...
✨  Preparing Kubernetes environment ...
🚜  Pulling images required by Kubernetes v1.14.1 ...
🔄  Relaunching Kubernetes v1.14.1 using kubeadm ... 
⌛  Waiting for pods: apiserver proxy etcd scheduler controller dns
📯  Updating kube-proxy configuration ...
🤔  Verifying component health ......
💗  kubectl is now configured to use "minikube"
🏄  Done! Thank you for using minikube!
``````````````````  
### Arrêt.
```
🐳 minikube stop
✋  Stopping "minikube" in virtualbox ...
🛑  "minikube" stopped.
```
## Weave scope
### Création du namespace k8s
``
🐳 kubectl create ns weave
namespace/weave created
``
### Lancement de l'application
```````````
🐳 kubectl apply -f examples/k8s 
clusterrolebinding.rbac.authorization.k8s.io/weave-scope created
clusterrole.rbac.authorization.k8s.io/weave-scope created
deployment.apps/weave-scope-app created
daemonset.apps/weave-scope-agent created
Warning: kubectl apply should be used on resource created by either kubectl create --save-config or kubectl apply
namespace/weave configured
deployment.apps/weave-scope-cluster-agent created
podsecuritypolicy.extensions/weave-scope created
serviceaccount/weave-scope created
service/weave-scope-app created
```````````
### Récupération de l'url d'accès.
``
🐳 minikube service weave-scope-app --url -n weave
http://192.168.99.106:30827
``
## Nettoyage
`
🐳 kubectl delete -f examples/k8s 
`
## Wordpress
### Lancement
Editer le fichier **kustomization.yaml** et y placer le mot de passe de la base Mysql.
````````
🐳 kubectl apply -k ./
secret/mysql-pass-ffgtbt8k66 unchanged
service/wordpress-mysql unchanged
service/wordpress unchanged
deployment.apps/wordpress-mysql unchanged
deployment.apps/wordpress unchanged
persistentvolumeclaim/mysql-pv-claim unchanged
persistentvolumeclaim/wp-pv-claim unchanged
````````
### Récupération de l'url d'accès.
``
🐳 minikube service wordpress --url
http://192.168.99.106:30830
``
## Nettoyage
`
🐳 kubectl delete -k ./
`
