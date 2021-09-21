# Elastic-Search Kibana Fluentd
EKF is used to store and publish the apllication logs to get a real time view of the application status and deep dive to resolve the problems if any.


Fluentd: 
Fluentd is a popular open-source data collector that helps to tail container log files, filter and transform the log data, and deliver it to the Elasticsearch cluster, where it will be indexed and stored. 
It can collect the logs automatically from the std output of the app pod or from any other redirected location or as side-car with an appropriate configuration.
Pre-requisites:
Deployment Kind:Daemonset (This ensures that a fluentd pod running inside every node in the cluster, a fluentd pod is created automatically either if a new node is created to replace exisitng problemetic node or a new node is created based on the cluster autoscaling feature in Cloud Native KUbernetes Clusters for the high availability).
Service Account: A service account is needed because fluentd needs to collect logs from the pods and since the logs are pushed to the Elastic-Search internally within the cluster so the default Cluster-IP service is used.
Cluster Role and Role Binding: This is needed to authorize the fluentd to collect the logs from a cluster resource.


Elastic Search:
