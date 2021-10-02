# curator

# 1) Deploy Curator as a Cronjob in your cluster

Create Cronjob resource from [curator.yaml](https://github.com/ManasPecenek/curator/blob/main/curator/curator.yaml)

# 2) Configure configmap for Cronjob


* To clean up logstash-year.month.day indices which are older than 9 days, deploy  [curator-config-01.yaml](https://github.com/ManasPecenek/curator/blob/main/curator/curator-config-01.yaml)

* To clean up logstash-year-month-day indices which are older than 24 hours, deploy   [curator-config-02.yaml](https://github.com/ManasPecenek/curator/blob/main/curator/curator-config-02.yaml)

* To clean up logstash indices which are older than 9 days, deploy [curator-config-03.yaml](https://github.com/ManasPecenek/curator/blob/main/curator/curator-config-03.yaml)

* To clean up logstash, fluentbit, arm indices which are older than 3 weeks simultaneously, deploy [curator-config-04.yaml](https://github.com/ManasPecenek/curator/blob/main/curator/curator-config-04.yaml)


Curator Pod and Config Map for kubernetes

If your elasticsearch is using secure connection, you need to configure curator-config-HTTPS.yaml file as your config file. Otherwise, it is enough to use curator-config.yaml.

For further information, you can visit https://docs.search-guard.com/latest/elasticsearch-curator-search-guard
