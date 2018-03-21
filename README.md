# amq63-prom-sti

Simple extension to the amq63 image to add in a prometheus agent



```
oc import-image jboss-amq-63 --from=registry.access.redhat.com/jboss-amq-6/amq63-openshift --confirm -n openshift


java -javaagent:agentA.jar -javaagent:agentB.jar MyJavaProgram
```


ACTIVEMQ_OPTS=-javaagent:agent/jmx_prometheus_javaagent-0.10.jar=9779:agent/config.yml


ACTIVEMQ_OPTS=/opt/amq/lib/jmx_prometheus_javaagent-0.10.jar=9779:/opt/amq/conf/promconfig.yml

```
[swinches@localhost GitHub]$ oc logs -f pods/sample-metric-app-1-4h8lf
Starting the Java application using /opt/run-java/run-java.sh ...
exec java -javaagent:agent/jmx_prometheus_javaagent-0.10.jar=9779:agent/config.yml -javaagent:/opt/jolokia/jolokia.jar=config=/opt/jolokia/etc/jolokia.properties -XX:ParallelGCThreads=1 -XX:ConcGCThreads=1 -Djava.util.concurrent.ForkJoinPool.common.parallelism=1 -cp . -jar /deployments/sample-metric-app-1.0.jar
I> No access restrictor found, access to any MBean is allowed
Jolokia: Agent started with URL https://172.17.0.2:8778/jolokia/
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.4.1.RELEASE)
```


```
https://192.168.42.197:8443/api/v1/namespaces/mydemo/pods/https:sample-metric-app-1-4h8lf:8778/proxy/jolokia/read/org.apache.camel:context=camel,type=routes,name=%22api-vm%22/LastProcessingTime

rules:
  - pattern: 'org.apache.camel<context=camel, type=routes, name=\"(.*)\"><>LastProcessingTime'
    name: camel_last_processing_time
    help: Last Processing Time [milliseconds]
    type: GAUGE
    labels:
      route: $1

rules:
  - pattern: 'org.apache.activemq<type=Broker, brokerName=\"(.*)\", destinationType="(.*)\", destinationName=\"(.*)\"><>QueueSize'
    name: queue_size
    help: Queue Size
    type: GAUGE
    labels:
      name: $1


org.apache.activemq:type=Broker,brokerName=broker-amq-1-bqvvp,destinationType=Topic,destinationName=ActiveMQ.Advisory.MasterBroker

https://192.168.42.197:8443/api/v1/namespaces/mydemo/pods/https:broker-amq-1-bqvvp:8778/proxy/jolokia/read/org.apache.activemq:type=Broker,brokerName=broker-amq-1-bqvvp,destinationType=Queue,destinationName=hello/QueueSize
```