# nexus-repo-guild
# nexus-repo-guild
Hi Today We install Nexus Repository And config it For *Docker* and *NPM*
Require:
    1- docker compose 
    2- install npm in you server (or can be localhost)
    3- config /.npmrc

1- You should install Nexus with Dokcer Compose 
2- Create NPM_HOSTED NPM_PROXY DOCKER_HOST
3- ADD LINK NPM_PROXY 
4- Change Firewall Config
5- FINISH 
# FIRE WALL CONFIG
```
sudo firewall-cmd --zone=public --add-port=8081/tcp --permanent
sudo firewall-cmd --zone=public --add-port=8082/tcp --permanent

```
# install nexus and create your repo : learn how to install nexus in youtube
edit this file ~/.npmrc
and add this config
```
NPM CONFIG :
registry=<URL NEXUS:PORT>/repository/npm-hosted/
//<URL NEXUES:port>/repository/npm-hosted/:username=admin
//<URL NEXUES:port>/repository/npm-hosted/:_password=<GENERATE YOUR PASSWORD WITH BASE64>
//<URL NEXUES:port>/repository/npm-hosted/:email=your-email@example.com
//<URL NEXUES:port>/repository/npm-hosted/:always-auth=true

```
Afther that :
```
npm init 
npm login --registry=http://172.16.2.129:8081/repository/npm-hosted/
```
and give a test :
```
[root@localhost nexus]# npm publish --registry=http://172.16.2.129:8081/repository/npm-hosted/
npm notice 
npm notice 📦  nexus@1.0.0
npm notice === Tarball Contents === 
npm notice 227B docker-compose.yml
npm notice 201B package.json      
npm notice === Tarball Details === 
npm notice name:          nexus                                   
npm notice version:       1.0.0                                   
npm notice filename:      nexus-1.0.0.tgz                         
npm notice package size:  378 B                                   
npm notice unpacked size: 428 B                                   
npm notice shasum:        f5889d91a1677214d7660455142adf5d507feae0
npm notice integrity:     sha512-NN8vCsRNm2HdF[...]rByzAW9TdoUfw==
npm notice total files:   2                                       
npm notice 
npm notice Publishing to http://172.16.2.129:8081/repository/npm-hosted/
+ nexus@1.0.0
```
base64 commnad:
```
echo -n 'username:password' | base64
```
#########
NPM_PROXY 
# CONFIG :
```
 npm set registry <URL NEXUS :PORT/repository/npm-proxy/
 ```
 ```
 registry=http://172.16.2.129:8081/repository/npm-proxy/
//<NEXUS URL : PORT >/repository/npm-hosted/:username=admin
//<NEXUS URL : PORT /repository/npm-hosted/:_password=QW1pcjMyZXJA
email=your-email@example.com
```
TEST TO SEE IT WORK : 
```
[root@localhost nexus]# npm publish --registry=http://172.16.2.129:8081/repository/npm-proxy/
npm notice 
npm notice 📦  nexus@1.0.0
npm notice === Tarball Contents === 
npm notice 227B docker-compose.yml
npm notice 201B package.json      
npm notice === Tarball Details === 
npm notice name:          nexus                                   
npm notice version:       1.0.0                                   
npm notice filename:      nexus-1.0.0.tgz                         
npm notice package size:  378 B                                   
npm notice unpacked size: 428 B                                   
npm notice shasum:        f5889d91a1677214d7660455142adf5d507feae0
npm notice integrity:     sha512-NN8vCsRNm2HdF[...]rByzAW9TdoUfw==
npm notice total files:   2                                       
npm notice 
npm notice Publishing to http://172.16.2.129:8081/repository/npm-hosted/
+ nexus@1.0.0
```
INSTALL SOMETHING :
```
[root@localhost nexus]# npm install express

added 65 packages in 10s

13 packages are looking for funding
  run `npm fund` for details
  ```
  # DOCKER REPO :
```
edit /etc/docker/damon.json
{
  "insecure-registries": ["<NEXSUS URL >:8082"]
}

#add this 
```
afther that you run this command :
```
docker login http://<NEXUS URL>:8082

```
Download one image and change tag : 
```
docker tag ubuntu:latest http://<NEXUS URL>:8082/image-name
````
Push to repo :
```
docker push 172.16.2.129:8082/image-name
```
FINISHE 
