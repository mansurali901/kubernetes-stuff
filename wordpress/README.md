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
