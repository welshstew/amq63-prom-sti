# amq63-prom-sti

Simple extension to the amq63 image to add in a prometheus agent.

Sti just bundles in the jar and prometheus yml config, in `configuration/promconfig.yml`

`ACTIVEMQ_OPTS` in the openshift template dictates the javaagent and prometheus config location:

`ACTIVEMQ_OPTS=/opt/amq/lib/jmx_prometheus_javaagent-0.10.jar=9779:/opt/amq/conf/promconfig.yml`

## To Build

```
oc new-project mydemo
oc import-image jboss-amq-63 --from=registry.access.redhat.com/jboss-amq-6/amq63-openshift --confirm -n openshift
oc new-build registry.access.redhat.com/jboss-amq-6/amq63-openshift:latest~https://github.com/welshstew/amq63-prom-sti.git
oc new-app --template=amq63-basic-prom -p MQ_QUEUES=hello1 -p IMAGE_STREAM_NAMESPACE=mydemo
```


## Queue Depth

This is the prometheus configuration just to pull out the QueueSize

```
rules:
  - pattern: 'org.apache.activemq<type=(\w+), brokerName=(\w+), destinationType=(\w+), destinationName=(\w+)><>QueueSize'
    name: queue_size
    help: Queue Size
    type: GAUGE
    labels:
      queue: $4
```


## References

Heavily inspired by Bruno's article 
https://brunonetid.github.io/2017/11/27/camel-prometheus-openshift.html