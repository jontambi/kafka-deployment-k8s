# kafka-deployment

Kafka Deployment

Step 1: Deploy Apache Zookeeper
The first step is to deploy Apache Zookeeper on your Kubernetes cluster using Bitnami's Helm chart. Your Apache Kafka deployment will use this Apache Zookeeper deployment for coordination and management.

First, add the Bitnami charts repository to Helm:

helm repo add bitnami https://charts.bitnami.com/bitnami

Next, execute the following command to deploy an Apache Zookeeper cluster with three nodes:

helm install --name zookeeper --namespace kafka bitnami/zookeeper --set replicaCount=1 --set auth.enabled=false --set allowAnonymousLogin=true --set volumePermissions.enabled=true --set persistence.enabled=true --set persistence.size=2Gi --set persistence.dataLogDir.size=2Gi --set persistence.storageClass=zookeeper-storage

Step 2: Deploy Apache Kafka
The next step is to deploy Apache Kafka, again with Bitnami's Helm chart. In this case, you will provide the name of the Apache Zookeeper service as a parameter to the Helm chart.

Execute the command below, replacing the ZOOKEEPER-SERVICE-NAME placeholder with the Apache Zookeeper service name obtained at the end of Step 1:

helm install --name kafka --namespace kafka bitnami/kafka --set zookeeper.enabled=false  --set replicaCount=1 --set externalZookeeper.servers=zookeeper --set volumePermissions.enabled=true --set persistence.enabled=true --set persistence.size=2Gi --set persistence.storageClass=kafka-storage

Documentacion Oficial: https://docs.bitnami.com/tutorials/deploy-scalable-kafka-zookeeper-cluster-kubernetes/