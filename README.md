# Observability
Observability is a measure of how well internal states of a system can be inferred from knowledge of its external outputs. Observability helps developers and operators (“DevOps”) understand distributed systems: what’s slow, what’s broken, and what needs to be done to improve performance.

### Three pillers of Observability
1. Logs  
2. Metrics  
3. Traces  

### SLA, SLO, SLI
#### SLA: Service Level Agreement. It is the agreement that you make with your clients or users.
#### SLO: Service Level Objective. It is the objective that the team must hit to meet the SLA.  It defines the target for SLIs. For example, p99 latency < 1s; 99.9% uptime; <1% errors. etc.
#### SLI: Service Level Indicators. It is the actual/real numbers on your performance. These are measurable data such as latency, uptime, and error rate.

# Prometheus
A monitoring tool which allows monitoring of complex infrastructure. Ir is a time series database for metrics monitoring. It Constantly monitors all the services and alerts when there is crash. It also alerts about the problems before they actually occur, allowing system administrtors to prevent the issue.   

In a complex infrastructure where multiple systems are running, and are interdependnet. So if one service goes down, others are impacted as well. In such cases, finding the issue quickly is difficult and time consuming.

## Prometheus Architecture
### 1. Retriever/Data Retriever Worker:  
Responsible from pulling the monitring metrics data from applications, services, servers etc. (termed as target here) and pushing them to database. Prometheus monnitors metrics like CPU usage, memory usage, disk usage, exceptions count, number of requests, request duration etc.   
### 2. Storage:   
It has a time series database, which stores all the metrics data like current CPU usage, number of exceptions etc.
### 3. HTTP Server (PromQL):   
Accepts queries for that stored data and shows data in dashboards either thru prometheus or other 3rd party data visualition tools like Grafana.
### 4. Pushgateway:
The component to which short lived servicres push the data
### 5. Alert Manager:
Pushes alerts via different channels like email, slack channel etc


### Metric
Prometheus uses human readable text based format for this metrics. Below is the example of text based format which Prometheus uses:

```yaml
# HELP http_requests_total The total number of HTTP requests.
# TYPE http_requests_total counter
http_requests_total{method="post",code="200"} 1027 1395066363000
http_requests_total{method="post",code="400"} 3 1395066363000
```

It has a part called help, which decsribes what the metrics is, and other part is type, which is one of the below 4 types:

#### Counter:   
Shows incremental counted things, like number of requests, or how many times x happened. The value of counter can only increase or be reset to zero on restart.  

#### Gauge:   
Metric that can go up and down is called as Gauge. For example CPU usage (what is the current value of CPU usage now), Memory usage, Disk usage, number of coucurrent requests etc. A gauge is a metric that represents a single numerical value that can arbitrarily go up and down. 

### Histogram:   
To measure how long and how big. For example the requests, how long did it take and how big it was. A histogram samples observations (usually things like request durations or response sizes) and counts them in configurable buckets. It also provides a sum of all observed values. Histogram should be used when we know the range of values.

### Summary:
Similar to a histogram, a summary samples observations (usually things like request durations and response sizes). While it also provides a total count of observations and a sum of all observed values, it calculates configurable quantiles over a sliding time window. Summary should be used when we do not know the range of values up front.   


In Prometheus, you can give each metric labels, which group metrics into segments of interest. For instance, a metric tracking HTTP request counts can be split according to status code, allowing us to know the number of HTTP 5XX errors.


### How Prometheus Works
Prometheus pulls metric data from targets. For that purpose, the targets must expose /metrics endpoint which Prometheus can refer. Targets must expose the data on /metrics endpiont in a format understandable to prometheus. Many services by default expose the metric data on /metrics end point. But there are many services who do not have native prometheus endpoints. In such cases we have to use exporters. Exporter is a script or service that fetches the metric data from the target and converts it into the Prometheus understands. It also exposes the converted data on its own /metrics endpoint. Prometheus already has a list of exporter for a lot many number of services. Exporters are also availabe as docker images, so that you can deploy them as side container so that it will fetch the data from main container and expose it on the /metrics endoint for Prometheus to scrape.   

In case you wish to expose your own application for lets say view the data for number of requests, exceptions, CPU and memory usage, then you will have to use Prometheus client libraries to expose this data on /metrics endpoint.   

Prometheus uses pull mechanism to fetch the data from endpoint. This is better than push mechanism where applications and servers keep pushing data to centralized systesm just like AWS CloudWatch. In push mechanism case, as services contineously keep pushing the data to the monitoring system, it generates a large number of traffic in the network, which is a disadvantage. Also demaons have to be installed on these servers to push the data, whereas Prometheus just uses a /metrics endpoint. The advantage of using the endpoint is that multiple Prometheus instances can pull the data from a single system. By using pull mechanish, Prometheus can easily detect whether the service is up and running.

Some services are short lived, for example Kubernetes jobs. So Prometheus pull maybe missed and data might not be scraped on regular interval or might not be scraped at all. For such cases, Prometheus provides a push mechanism called `Pushgateway`.

#### What to scrape and when
It is defined in `Prometheus.yaml` file. You have to specify the targets and scrape interval in the yaml file. Prometheus then uses a service discovery mechanism to find the target endpoints.   


### Rules
We can define rules in Prometheus to aggregate metric values or to create alerts when condition is met (create an alert when CPU reaches 60%).

Below is a sample Prometheus.yaml
```yaml
global:
  scrape_interval:     15s #Global interval to scrape the endpoints
  evaluation_interval: 15s #Global interval to evaluate the rules

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'
# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s #Local interval to scrape the endpoint

    static_configs:
      - targets: ['localhost:9090']
```


### PromQL
A query language used by Prometheus to query the data. One sample PromQL statement is given below:
To select all HTTP status codes except 4xx ones, you could run:
```typescript
http_requests_total{status!~"4.."}
```


# ELK/EFK
### Elastisearch
A text search engine that stores and makes available string data for searching. It is commonly used for log analytics, full-text search, security intelligence, business analytics, and operational intelligence use cases.

### Logstash
Logstash is a light-weight, open-source, server-side data processing pipeline that allows you to collect data from a variety of sources, transform it on the fly, and send it to your desired destination.

### Kibana
Kibana is a data visualization and exploration tool used for log and time-series analytics, application monitoring, and operational intelligence use cases. Kibana is an free and open frontend application that sits on top of the Elastic Stack, providing search and data visualization capabilities for data indexed in Elasticsearch. It is also known as the Charting tool for Elastic stack
