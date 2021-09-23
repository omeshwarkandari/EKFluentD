# Elastic-Search Kibana Fluentd for the Log Aggregation

EKF is used to store and publish the apllication logs to get a real time view of the application status and deep dive to resolve the problems if any.


Fluentd: 
Fluentd is a popular open-source data collector that helps to tail container log files, filter and transform the log data, and deliver it to the Elasticsearch cluster, where it will be indexed and stored. 
It can collect the logs automatically from the std output of the app pod or from any other redirected location or as side-car with an appropriate configuration.
Pre-requisites:
Deployment Kind:Daemonset (This ensures that a fluentd pod running inside every node in the cluster, a fluentd pod is created automatically either if a new node is created to replace exisitng problemetic node or a new node is created based on the cluster autoscaling feature in Cloud Native KUbernetes Clusters for the high availability).
Service Account: A service account is needed because fluentd needs to collect logs from the pods and since the logs are pushed to the Elastic-Search internally within the cluster so the default Cluster-IP service is used.
Cluster Role and Role Binding: This is needed to authorize the fluentd to collect the logs from a cluster resource.


Elastic Search:
It is a database to store, retrieve and manage bot the document orieneted as well as semi-strucuted data like logs.
Its deployed as Statefulset typically with 2-3 pods where one pod is master while rest are slaves.


Kibana: It helps to explore the log data stored in the ES Database through Web Interface, build dashboards and queries to help gain real-time insight into the applications
running in the kubernetes cluster.

Depoly the EFK:
Login the Master Server and clone the EFK repo:
$ git clone https://github.com/omeshwarkandari/EKFluentD.git
kubeadm@Master-Node:~$ cd EKF/EKFluentD/
kubeadm@Master-Node:~/EKF/EKFluentD$ ls
README.md  es-service.yaml  es-statefulset.yaml  fluentd-es-configmap.yaml  fluentd-es-ds.yaml  kibana-deployment.yaml  kibana-service.yaml
$ kubectl apply -f .
This will create required configuration inside a new namespace "efklog" as mentioned in the deployment and get all the objects inside ns efklog.
$ kubectl get all -n efklog
NAME                                  READY   STATUS    RESTARTS   AGE
pod/elasticsearch-logging-0           1/1     Running   0          43m
pod/elasticsearch-logging-1           1/1     Running   0          43m
pod/fluentd-es-v2.7.0-htk2q           1/1     Running   0          43m
pod/fluentd-es-v2.7.0-v64mb           1/1     Running   0          43m
pod/kibana-logging-54b4c97577-hzq6g   1/1     Running   0          43m

NAME                            TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/elasticsearch-logging   ClusterIP      10.96.6.60      <none>        9200/TCP         43m
service/kibana-logging          LoadBalancer   10.100.22.246   <pending>     5601:30493/TCP   43m

NAME                               DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
daemonset.apps/fluentd-es-v2.7.0   2         2         2       2            2           <none>          43m

NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/kibana-logging   1/1     1            1           43m

NAME                                        DESIRED   CURRENT   READY   AGE
replicaset.apps/kibana-logging-54b4c97577   1         1         1       43m

NAME                                     READY   AGE
statefulset.apps/elasticsearch-logging   2/2     43m


Explore the Kibana dashboard on the http:<Public-IP>:30493
Once dashboard is visible follow below steps to see the metrics:
1. Click on the icon "Discover"
2. Create an index pattern by entering "*" in the index pattern space and click next
3. Select the time stamp "@Timestamp" and click next
4. Click on the Create Index Pattern
5. Click on the Discover again and the dashboard is ready
6. Customize or add filter....
