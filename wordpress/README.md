# Wordpress Deployment on kubernetes Cluster 
This deployment cosist three steps 
- Setup Mysql 
- Add Mysql User and Database for Wordpress
- Crete Docker Image 
- Wordpress deployment 
### Step # 1 Setup Mysql deployment and Service 
```bash
[root@ip-10-0-0-219 wordpress-deployment]# kubectl apply -f mysql/deployment.yaml
```
```bash
[root@ip-10-0-0-219 wordpress-deployment]# kubectl apply -f mysql/service.yaml
```
### Step # 2 Setup Mysql User 
For this purpose we need to access mysql container/POD to add user to achive this task
we are going to use multitools this container is equipped with all the related tools being
used in the development i normally use this tool to do debugging in kubernetes

### Install Multi tools in the cluster and access it 
```bash
[root@ip-10-0-0-219 wordpress-deployment]# kubectl run nwtool --image praqma/network-multitool
```
Connect to multitool pod and create mysql user and database
```bash
[root@ip-10-0-0-219 wordpress]# kubectl get pods
NAME                         READY   STATUS    RESTARTS   AGE
mysql-d8b849957-8l82f        1/1     Running   0          5h56m
nwtool-7479c79848-8xfv4      1/1     Running   0          5h53m
wordpress-6d7fbb9657-tsfvx   1/1     Running   0          13m
[root@ip-10-0-0-219 wordpress]# kubectl exec -it nwtool-7479c79848-8xfv4 -- bash
bash-5.0# mysql -h mysql -u root -p
Enter password:
ERROR 1045 (28000): Access denied for user 'root'@'100.126.0.2' (using password: YES)
bash-5.0# mysql -h mysql -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 51
Server version: 5.6.47 MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]> drop database wpstore;
Query OK, 12 rows affected (0.058 sec)

MySQL [(none)]> create database wpstore;
Query OK, 1 row affected (0.000 sec)

MySQL [(none)]> grant all privileges on wpstore.* to wpadmin@'%' identified by 'password';
Query OK, 0 rows affected (0.000 sec)

MySQL [(none)]> flush privileges;
Query OK, 0 rows affected (0.000 sec)
```
### Step # 3 Download wordpress
```bash
[root@ip-10-0-0-219 wordpress]# cd web && wget http://wordpress.org/latest.tar.gz
[root@ip-10-0-0-219 wordpress]# tar -xvf latest.tar.gz
```
### Step # 4 Build the docker Image
```bash
[root@ip-10-0-0-219 wordpress]# docker build -t wordpressupworks:1.0.0 .
[root@ip-10-0-0-219 wordpress]# docker tag wordpressupworks:1.0.0 <youdockerregistry>/wordpressupworks:1.0.0 
[root@ip-10-0-0-219 wordpress]# docker push <youdockerregistry>/wordpressupworks:1.0.0
``` 
### Step # 5 Install Pod of wordpress web
Note : "Don't forget to update image in deployment.yaml name should be the build in above steps "
```bash
[root@ip-10-0-0-219 wordpress]# kubectl apply -f deployment.yaml
```
### Step # 6 Install Service 
```bash 
[root@ip-10-0-0-219 wordpress]# kubectl apply -f service.yaml 
```
