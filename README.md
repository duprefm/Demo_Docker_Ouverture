# Demo_Docker_Ouverture
## Minikube.
Cluster local Kubernetes utilisé dans le cadre de phases de tests ou de développement.
### Démarrage.
> 🐳 minikube start

`````````````````
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
`````````````````
### Arrêt.
> 🐳 minikube stop

``
✋  Stopping "minikube" in virtualbox ...
🛑  "minikube" stopped.
``
## Créer une inmage Docker from scratch.
Je vais créer une image proposant le service apache.
- Contenu de mon fichier index.html:
`````````
<!DOCTYPE html>
<html>
<head>
    <title>Ouvertures I2s</title>
</head>
<body>
    <h1>Bonjour a tous !</h1>
</body>
</html>
`````````
- Contenu du Dockerfile:
``````
FROM centos:latest
MAINTAINER NewstarCorporation
RUN yum -y install httpd
COPY index.html /var/www/html/
EXPOSE 80
CMD [ "/usr/sbin/httpd", "-DFOREGROUND" ]
``````
### Construction de l'image Docker.
> docker build -t webserver:0.1 .
### Lancement du container Docker.
> 🐳 docker run -dit -p 1234:80 webserver:0.1
`
a906027058814311d858d6cfbca82b1bc5a1d02c25388f00be547608e1c14089
`
### Vérification de la bonne execution du container.
> 🐳 docker ps
``
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                  NAMES
a90602705881        webserver:0.1       "/usr/sbin/httpd -DF…"   About a minute ago   Up About a minute   0.0.0.0:1234->80/tcp   great_benz
``
Le contenu de la page **index.html** est visible a l'adresse http://localhost:1234.
### Connexion a l'interieur du container.
> 🐳 docker exec -it a90602705881 /bin/bash

`````````````
[root@a90602705881 /]# ps -edf
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 20:05 pts/0    00:00:00 /usr/sbin/httpd -DFOREGROUND
apache       8     1  0 20:05 pts/0    00:00:00 /usr/sbin/httpd -DFOREGROUND
apache       9     1  0 20:05 pts/0    00:00:00 /usr/sbin/httpd -DFOREGROUND
apache      10     1  0 20:05 pts/0    00:00:00 /usr/sbin/httpd -DFOREGROUND
apache      11     1  0 20:05 pts/0    00:00:00 /usr/sbin/httpd -DFOREGROUND
apache      12     1  0 20:05 pts/0    00:00:00 /usr/sbin/httpd -DFOREGROUND
apache      13     1  0 20:06 pts/0    00:00:00 /usr/sbin/httpd -DFOREGROUND
apache      14     1  0 20:06 pts/0    00:00:00 /usr/sbin/httpd -DFOREGROUND
apache      15     1  0 20:06 pts/0    00:00:00 /usr/sbin/httpd -DFOREGROUND
root        16     0  0 20:12 pts/1    00:00:00 /bin/bash
root        29    16  0 20:12 pts/1    00:00:00 ps -edf
`````````````
Nous avons créer une image Docker, lancé un container et vérifié qu'il fonctionnait.

## Wordpress
### Lancement
Editer le fichier **kustomization.yaml** et y placer le mot de passe de la base Mysql.
> 🐳 kubectl apply -k ./

```````
secret/mysql-pass-ffgtbt8k66 unchanged
service/wordpress-mysql unchanged
service/wordpress unchanged
deployment.apps/wordpress-mysql unchanged
deployment.apps/wordpress unchanged
persistentvolumeclaim/mysql-pv-claim unchanged
persistentvolumeclaim/wp-pv-claim unchanged
```````
### Récupération de l'url d'accès.
> 🐳 minikube service wordpress --url

`
http://192.168.99.106:30830
`
## Nettoyage
> 🐳 kubectl delete -k ./
