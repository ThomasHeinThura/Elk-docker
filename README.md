# Elk-docker
Reference:
install docker
https://elasticsearch.evermight.com/docker-elk-2-agent-fleet-server-apm/
https://www.elastic.co/blog/getting-started-with-the-elastic-stack-and-docker-compose-part-2

For SQL server monitoring using ELK
https://medium.com/@gopikrishnapandurangan/what-is-elastic-cloud-090e218d540e


## Pre-install
1. Make sure mount nfs or storage and create volume. 
2. Root uer

## 1. Install docker module 
- https://docs.docker.com/engine/install/rhel/

### deploy post implementation 
- https://docs.docker.com/engine/install/linux-postinstall/

### install portainer
```sh
docker run -d -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.21.4
```

## 2. download elk setup 
```zsh
git clone https://github.com/ThomasHeinThura/Elk-docker.git
```

## install the setup 
```sh 
docker compose up -d
```

### fleet-server setup 
Need to add an encryption key. need to fix the fleet server and remove the APM server. Fleet server enrollment error.

To get the certificate fingerprint, run the following commands:

```zsh
docker cp es-cluster-es01-1:/usr/share/elasticsearch/config/certs/ca/ca.crt /tmp/.
```
and Run the below to get the certificate fingerprint.

```zsh
openssl x509 -fingerprint -sha256 -noout -in /tmp/ca.crt | awk -F"=" {' print $2 '} | sed s/://g
```
Use the below command to output the certificate;
```zsh
cat /tmp/ca.crt
```

Once you have the certificate text, we will add it to a yml format and input all this information into the Fleet Settings screen from earlier.

```yml
ssl:
    certificate_authorities:
    - |
```
and add the contents. 

We need to add fleet-server in the fleet-server. setup as `advance` and add hostname url as `https://fleet-server:8220`.  

we need to add hostname and ip in every db agent and vm.   

```
# \etc\hosts

<ELK-ip> fleet-server

```

### APM server setup 
Uninstall apm integration and re-setup the apm server with http://0.0.0.0:8200


### Test 
1. Test with vm (install new agent on vm)
2. Test with db (add integration to db)
3. Test with elastic defender (add integraitn to vm)
4. Test with apm



```sh
java -javaagent:opentelemetry-javaagent.jar \
    -Dotel.service.name=test-doctor-serivce \
    -Dotel.exporter.otlp.endpoint=http://localhost:8200 \
    -Dotel.metrics.exporter=otlp \
    -Dotel.logs.exporter=otlp \
    -Dotel.resource.attributes=deployment.environment=production \
    -jar doctorinfo-jdk11.jar
```

Test with 
```sh
curl -X GET "http://<Java-app-ip>:9090/grandOak/doctors/Physician" -f
```

### Post integration
1. add life cycle policy to trace. (apm policy and database policy)
2. add dashboard user. 
3. add dashboard