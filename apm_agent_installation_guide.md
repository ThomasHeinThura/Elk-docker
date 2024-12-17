### Download the apm agent
- Download apm jar file from https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/tag/v2.10.0

or with wget cmd
```sh
 wget https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/download/v2.10.0/opentelemetry-javaagent.jar
```
  
### Add agent to the vm 
- Add apm jar to the weblogic server vm 


### Add agent to weblogic server 
- Add the path to the Java agent to your domain startup script:
- full path - wls1411/user_projects/domains/base_domain/bin/startWebLogic.sh
- add line in `startweblogic.sh` file
- add -javaagent:<APM Agent path> to actual file path of apm agent jar file. 
- add -Dotel.service.name=UAT-<service-name> to actual service.name.

#### For UAT
```sh
export JAVA_OPTIONS="$JAVA_OPTIONS -javaagent:<APM Agent path>/opentelemetry-javaagent.jar -Dotel.service.name=UAT-<Service-name> -Dotel.exporter.otlp.endpoint=http://10.100.64.41:8200 -Dotel.metrics.exporter=otlp -Dotel.log    s.exporter=otlp -Dotel.resource.attributes=deployment.environment=UAT"
```

#### For Production.
- add -javaagent:<APM Agent path> to actual file path of apm agent jar file. 
- add -Dotel.service.name=UAT-<service-name> to actual service.name.

```sh
export JAVA_OPTIONS="$JAVA_OPTIONS -javaagent:<APM Agent path>/opentelemetry-javaagent.jar -Dotel.service.name=Prod-<Service-name> -Dotel.exporter.otlp.endpoint=http://10.100.66.41:8200 -Dotel.metrics.exporter=otlp -Dotel.log    s.exporter=otlp -Dotel.resource.attributes=deployment.environment=Production"
```





