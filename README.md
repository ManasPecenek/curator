------------------------
# Curator for Kubernetes
------------------------

## This repository consists of Curator cronjob file and config files which delete certain indeces except ^\.kibana.*$ (in order not to cause Kibana to crash)

# 1) Deploy Curator as a Cronjob in your cluster

Create Cronjob resource from [curator.yaml](https://github.com/ManasPecenek/curator/blob/main/curator/curator.yaml)

# 2) Configure configmap for Cronjob


* To clean up logstash-year.month.day indices which are older than 9 days, deploy  [curator-config-01.yaml](https://github.com/ManasPecenek/curator/blob/main/curator/curator-config-01.yaml)

* To clean up logstash-year-month-day indices which are older than 24 hours, deploy   [curator-config-02.yaml](https://github.com/ManasPecenek/curator/blob/main/curator/curator-config-02.yaml)

* To clean up logstash indices which are older than 9 days, deploy [curator-config-03.yaml](https://github.com/ManasPecenek/curator/blob/main/curator/curator-config-03.yaml)

* To clean up all indices which are older than 15 days, deploy [curator-config-05.yaml](https://github.com/ManasPecenek/curator/blob/main/curator/curator-config-05.yaml)

* To clean up logstash, fluentbit, arm indices which are older than 3 weeks simultaneously, deploy [curator-config-04.yaml](https://github.com/ManasPecenek/curator/blob/main/curator/curator-config-04.yaml)


# 3) Curator Configmap for HTTPS

If your elasticsearch is using secure connection, you need to configure [curator-config-HTTPS.yaml](https://github.com/ManasPecenek/curator/blob/main/curator/curator-config-HTTPS.yaml) file for HTTP Basic Authentication.

# 4) How To Test

* Create a basic cluster

````
git clone --depth=1 https://github.com/ManasPecenek/clinco.git && cd clinco && chmod +x initial-script.sh add-worker.sh && alias startCluster="bash initial-script.sh" addNode="bash add-worker.sh"

startCluster <worker-node-count>
````

* Now we can deploy curator

````
git clone --depth=1 https://github.com/ManasPecenek/curator.git && cd curator/curator

kubectl create ns monitoring-ns

kubectl run es -n monitoring-ns --image elasticsearch:7.14.1 --env "discovery.type=single-node" --port 9200

kubectl expose pod es -n monitoring-ns --name es-svc --port 9200

kubectl apply -f curator-config-01.yaml && kubectl apply -f curator.yaml # Update the hosts part in curator-config-01.yaml as es-svc

kubectl create job -n monitoring-ns test-job --from=cronjob/curator-v2

kubectl get pods -n monitoring-ns -w # Check that job is completed

kubectl logs test-job-xxxx # Check the logs whether the job has successfully deleted the indices
````

* Then delete the cluster

`docker rm -f master worker-1`


# Sources

For further information, you can visit:

https://docs.search-guard.com/latest/elasticsearch-curator-search-guard

https://www.elastic.co/guide/en/elasticsearch/client/curator/current/filters.html

https://www.elastic.co/guide/en/elasticsearch/client/curator/current/filtertype_age.html

https://www.elastic.co/guide/en/elasticsearch/client/curator/current/configfile.html
