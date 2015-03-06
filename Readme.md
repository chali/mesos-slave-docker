#Mesos Slave with Java 7 and Hadoop CLI

Mesos slave image based on this [Mesos Slave Image](https://registry.hub.docker.com/u/redjack/mesos-slave/). I've added Java 7 and Hadoop CLI. It allows to download binaries for mesos task from HDFS. I've used this slave together with [Mesos Master Image](https://registry.hub.docker.com/u/redjack/mesos-master/) and [Zookeeper Image](https://registry.hub.docker.com/u/jplock/zookeeper/)

###Execution
Get docker image

    docker pull chalimartines/mesos-slave

Run image with specified port mapping and support for docker containers as mesos task (it requeires running zookeeper image with name zookeeper)

    docker run -d --name slave-mesos -e MESOS_LOG_DIR=/var/log -e MESOS_HOSTNAME=localhost -e MESOS_MASTER=zk://zookeeper:2181/mesos -e MESOS_EXECUTOR_REGISTRATION_TIMEOUT=5mins -e MESOS_ISOLATOR=cgroups/cpu,cgroups/mem -e MESOS_CONTAINERIZERS=docker,mesos -v /var/run/docker.sock:/var/run/docker.sock -v /sys:/sys -v /proc:/proc -p 5051:5051 --link zookeeper:zookeeper chalimartines/mesos-slave
  
If you are Mac OS user with boot2docker and you would like to get from your local system to a cdh container add these port forwardings

    VBoxManage modifyvm "boot2docker-vm" --natpf1 "tcp-port5050,tcp,,5050,,5050"
