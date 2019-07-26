# Demo_Docker_Ouverture
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
> (⎈ |docker-for-desktop:default)🐳 Apache docker build -t webserver:0.1 .
### Lancement du container Docker.
> (⎈ |docker-for-desktop:default)🐳 Apache docker run -dit -p 1234:80 webserver:0.1

`
a906027058814311d858d6cfbca82b1bc5a1d02c25388f00be547608e1c14089
`
### Vérification de la bonne execution du container.
> (⎈ |docker-for-desktop:default)🐳 Apache docker ps

``
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                  NAMES
a90602705881        webserver:0.1       "/usr/sbin/httpd -DF…"   About a minute ago   Up About a minute   0.0.0.0:1234->80/tcp   great_benz
``
Le contenu de la page **index.html** est visible a l'adresse http://localhost:1234.
### Connexion a l'interieur du container.
> (⎈ |docker-for-desktop:default)🐳 Apache docker exec -it a90602705881 /bin/bash

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
> (⎈ |docker-for-desktop:default)🐳 wordpress kubectl apply -k ./

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
> (⎈ |docker-for-desktop:default)🐳 wordpress kubectl get svc

````
NAME              TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes        ClusterIP      10.96.0.1       <none>        443/TCP        3d
wordpress         LoadBalancer   10.111.137.66   localhost     80:30336/TCP   1h
wordpress-mysql   ClusterIP      None            <none>        3306/TCP       1h
````
Le site wordpress est accessible a l'adresse http://localhost:30336

## Nettoyage
> (⎈ |docker-for-desktop:default)🐳 wordpress kubectl delete -k ./
