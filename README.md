# Terminologies

## Observability
Observability is a measure of how well internal states of a system can be inferred from knowledge of its external outputs. Observability helps developers and operators (“DevOps”) understand distributed systems: what’s slow, what’s broken, and what needs to be done to improve performance. Observabiility helps you understand the internals of your production system by asking questions from outside.

Observability is the ability to measure the internal states of a system by examining its outputs. A system is considered “observable” if the current state can be estimated by only using information from outputs, namely sensor data. Over the last several years, enterprises have rapidly adopted cloud-native infrastructure services, such as AWS, in the form of microservice, serverless and container technologies. Tracing an event to its origin in these distributed systems requires thousands of processes running on the cloud, on-premise or both. But conventional monitoring techniques and tools struggle to track the many communication pathways and interdependencies in these distributed architectures.

Observability allows teams to monitor modern systems more effectively and helps them to find and connect effects in a complex chain and trace them back to their cause. Further, it gives system administrators, IT operations analysts and developers visibility into their entire architecture.

## Monitoring
Monitoring — the process of gathering telemetry data on the operation of an IT environment to gauge performance and troubleshoot issues. Monitoring involves many different steps — data collection, data processing, data analysis — to name just a few.   
`Definition from Google: Collecting, processing, aggregating, and displaying real-time quantitative data about a system, such as query counts and types, error counts and types, processing times, and server lifetimes.`


## Monitoring vs Observability
Monitoring and observability are distinct concepts that depend on each other. Monitoring is an action you perform to increase the observability of your system. Observability is a property of that system, like functionality or testability.

Specifically, monitoring is the act of observing a system’s performance over time. Monitoring tools collect and analyze system data and translate it into actionable insights. Fundamentally, monitoring technologies, such as application performance monitoring (APM), can tell you if a system is up or down or if there is a problem with application performance. Monitoring data aggregation and correlation can also help you to make larger inferences about the system. Load time, for example, can tell developers something about the user experience of a website or an app.

Observability, on the other hand, is a measure of how well the system’s internal states can be inferred from knowledge of its external outputs. It uses the data and insights that monitoring produces to provide a holistic understanding of your system, including its health and performance. The observability of your system, then, depends partly on how well your monitoring metrics can interpret your system's performance indicators.

Another important difference is that monitoring requires you to know what’s important to monitor in advance. Observability lets you determine what’s important by watching how the system performs over time and asking relevant questions about it.


## White-box monitoring
White box monitoring is where you know the internals of the system. And the system has instrumentation in place to emit telemetry—metrics, logs, traces.

## Black-box monitoring
Black box monitoring is where you don’t have control and don’t know what’s happening inside the system. You only monitor the system from the outside—its behavior.

### Three pillers of Observability
**1. Logs:** A log is a text record of an event that happened at a particular time and includes a timestamp that tells when it occurred and a payload that provides context. Logs come in three formats: plain text, structured and binary. Plain text is the most common, but structured logs — which include additional data and metadata and are easier to query — are becoming increasingly popular. Logs are also typically the first place you look when something goes wrong in a system.  
**2. Metrics:** A metric is a numeric value measured over an interval of time and includes specific attributes such as timestamp, name, KPIs and value. Unlike logs, metrics are structured by default, which makes it easier to query and optimize for storage, giving you the ability to retain them for longer periods.  
**3. Traces:** A trace represents the end-to-end journey of a request through a distributed system. As a request moves through the host system, every operation performed on it — called a “span” — is encoded with important data relating to the microservice performing that operation. By viewing traces, each of which includes one or more spans, you can track its course through a distributed system and identify the cause of a bottleneck or breakdown.

Standard components of a logging stack are:
*  Logs exporter (configure logs per host)
*  Log collector listening for log input
*  Logs storage
*  Logs visualization

### SRE
SRE (site reliability engineering) is a discipline used by software engineering and IT teams to proactively build and maintain more reliable services. SRE is a functional way to apply software development solutions to IT operations problems. 


### SLA, SLO, SLI
#### SLA: Service Level Agreement. It is the agreement that you make with your clients or users.
#### SLO: Service Level Objective. It is the objective that the team must hit to meet the SLA.  It defines the target for SLIs. For example, p99 latency < 1s; 99.9% uptime; <1% errors. etc.
#### SLI: Service Level Indicators. It is the actual/real numbers on your performance. These are measurable data such as latency, uptime, and error rate.


### Error Budget
An error budget is the maximum amount of time that a technical system can fail without contractual consequences. 

For example, if your Service Level Agreement (SLA) specifies that systems will function 99.99% of the time before the business has to compensate customers for the outage, that means your error budget (or the time your systems can go down without consequences) is 52 minutes and 35 seconds per year.

If your SLA promises 99.95% uptime, your error budget is four hours, 22 minutes, and 48 seconds. And with an SLA promise of 99.9% uptime, your error budget is eight hours, 46 minutes, and 12 seconds.


# Prometheus
A monitoring tool which allows monitoring of complex infrastructure. It is a time series database for metrics monitoring. It Constantly monitors all the services and alerts when there is crash. It also alerts about the problems before they actually occur, allowing system administrtors to prevent the issue.   

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
    Collecting, processing, aggregating, and displaying real-time quantitative data about a system, such as query counts and types, error counts and types, processing times, and server lifetimes.

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

What makes Prometheus unique among other monitoring tools is a multi-dimensional data model in which metrics are identified with a name and an unordered set of key-value pairs called labels. The native querying language, PromQL, can be used to not only aggregate across these labels and then later visualize the data but also to define alerts.

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

# Distributed Tracing
Distributed tracing allows you to see the “flow" of one request as it moves through various services in the distributed system. Distributed tracing helps pinpoint where failures occur and what causes poor performance.   

In distributed tracing, a single trace contains a series of tagged time intervals called spans. A span can be thought of as a single unit of work. Spans have a start and end time, and optionally may include other metadata like logs or tags that can help classify “what happened.” Spans have relationships between one another, including parent-child relationships, which are used to show the specific path a particular transaction takes through the numerous services or components that make up the application.   

* **Trace** represents an end-to-end request; made up of single or multiple spans
* **Span** represents work done by a single-service with time intervals and associated metadata; the building blocks of a trace. It represents an individual unit of work done in a distributed system. Span Id is applicable to the flow of one microservice. If the developer gets this Id, he/she can find the flow of execution in a particular microservice. Each component of the distributed system contributes a span - a named, timed operation representing a piece of the workflow.   
Spans can (and generally do) contain “References” to other spans, which allows multiple Spans to be assembled into one complete Trace - a visualization of the life of a request as it moves through a distributed system.   
  * A start timestamp and finish timestamp
  * A set of key:value span Tags
  * A set of key:value span Logs
  * A SpanContext   
  
* **Tags metadata** to help contextualize a span

The point of traces is to provide a request-centric view. So, while microservices enable teams and services to work independently, distributed tracing provides a central resource that enables all teams to understand issues from the user’s perspective.
logs in traces.

![Distributed Tracing](https://github.com/pankdhnd/Observability/blob/main/images/logs_in_trace.png)

[For more information, follow this link](https://lightstep.com/distributed-tracing/)

It’s critical that spans and traces are tagged in a way that identifies these resources: every span should have tags that indicate the infrastructure it’s running on (datacenter, network, availability zone, host or instance, container) and any other resources it depends on (databases, shared disks). For spans representing remote procedure calls, tags describing the infrastructure of your service’s peers (for example, the remote host) are also critical.

Distrubuted tracing require instrumentation of the application/code. Instrumentation:   
* Assigns each external request a unique external request id
* Passes the external request id to all services that are involved in handling the request
* Includes the external request id in all log messages
* Records information (e.g. start time, end time) about the requests and operations performed when handling a external request in a centralized service



# Tools
### Elastisearch
A text search engine that stores and makes available string data for searching. It is commonly used for log analytics, full-text search, security intelligence, business analytics, and operational intelligence use cases.

### Logstash
Logstash is a light-weight, open-source, server-side data processing pipeline that allows you to collect data from a variety of sources, transform it on the fly, and send it to your desired destination.

### Kibana
Kibana is a data visualization and exploration tool used for log and time-series analytics, application monitoring, and operational intelligence use cases. Kibana is an free and open frontend application that sits on top of the Elastic Stack, providing search and data visualization capabilities for data indexed in Elasticsearch. It is also known as the Charting tool for Elastic stack

### Jeager
Jaeger is open source software for tracing transactions between distributed services. Distributed tracing allows you to see the “flow" of one request as it moves through various services in the system.

### OpenTelemetry
An observability framework for cloud-native software. OpenTelemetry is a collection of tools, APIs, and SDKs. You can use it to instrument, generate, collect, and export telemetry data (metrics, logs, and traces) for analysis in order to understand your software's performance and behavior.

### Fluentd
Fluentd is a cross platform open-source data collection software project originally developed at Treasure Data. Fluentd can aggregate data from multiple sources, unify the differently formatted data into JSON objects and route it to different output destinations. Design wise — performance, scalability, and reliability are some of Fluentd’s outstanding features. A vanilla Fluentd deployment will run on ~40MB of memory and is capable of processing above 10,000 events per second. Adding new inputs or outputs is relatively simple and has little effect on performance. Fluentd uses disk or memory for buffering and queuing to handle transmission failures or data overload and supports multiple configuration options to ensure a more resilient data pipeline.

### Fluentbit
Fluent Bit is an open source Log Processor and Forwarder which allows you to collect any data like metrics and logs from different sources, enrich them with filters and send them to multiple destinations. It's the preferred choice for containerized environments like Kubernetes. Fluent Bit was created with a specific use case in mind — highly distributed environments where limited capacity and reduced overhead (memory and CPU) are a huge consideration. To serve this purpose, Fluent Bit was designed for high performance and comes with a super light footprint, running on ~450KB only. An abstracted I/O handler allows asynchronous and event-driven read/write operations. For resiliency and reliability, various configuration option are available for defining retries and the buffer limit.

### Zipkin
Zipkin is a distributed tracing system. It helps gather timing data needed to troubleshoot latency problems in service architectures. 

Zipkin is used with Sleuth (Spring Cloud Sleuth), which provides instrumentation for spring boot applications. Sleuth is a library available as a part of Spring Cloud project. Sleuth provides unique ids for the request flow. Further, the developer uses these ids to find out the flow of execution with the help of tools like Zipkin, ELK etc. Generally it has two ids : Trace id and Span Id. Trace Id is applicable for a complete flow. If the developer gets this Id, he/she can find the flow of execution in all microservices involved. However, Span Id is applicable to the flow of one microservice. If the developer gets this Id, he/she can find the flow of execution in a particular microservice. Moreover, there is a concept of parent id which is applicable to a particular microservice like span id. During the flow of execution, span id of the previous microservice becomes the parent id of the next microservice as shown below.

![Sleuth](https://github.com/pankdhnd/Observability/blob/main/images/sleuth.png)

We use Zipkin in two parts : Zipkin Client and Zipkin Server. Zipkin Client contains Sampler which collects data from microservices with the help of Sleuth and provides it to the Zipkin Server. In order to utilize the benefits of both tools, we should always add Zipkin Client’s dependency along with Sleuth in every microservice. However, there must be only one centralized Zipkin Server, which collects all data from Zipkin Client and display it as a UI. Hence, after making a request developer should look into Zipkin Server to find trace id, span id and even flow of execution. From here itself, Open Log files to check Log lines that are related to the current trace Id.

Zipkin internally it has 4 modules
* **Collector** – Once any component sends the trace data arrives to Zipkin collector daemon, it is validated, stored, and indexed for lookups by the Zipkin collector.
* **Storage** – This module store and index the lookup data in backend. Cassandra, ElasticSearch and MySQL are supported.
* **Search** – This module provides a simple JSON API for finding and retrieving traces stored in backend. The primary consumer of this API is the Web UI.
* **Web UI** – A very nice UI interface for viewing traces.


# The Four Golden Insights/Golden Signals
## Latency (Request Service Time)
Latency is the time it takes a system to respond to a request. Both successful and failed requests have latency and it’s vital to differentiate between the latency of successful and failed requests. For example, an HTTP 500 error, triggered because of a connection loss to the database might be served fast. Although, since HTTP 500 is an error indicating failed request, factoring it into the overall latency will lead to misleading calculations. Alternatively, a slow error can be even worse as it factors in even more latency. Therefore, instead of filtering out errors altogether, keep track of the error latency. Define a target for a good latency rate and monitor the latency of successful requests against failed ones to track the system’s health. 

## Traffic (User Demand)
Traffic is the measure of how much your service is in demand among users. How this is determined varies depending on the type of business you have. For a web service, traffic measurement is generally HTTP requests per second, while. In a storage system, traffic might be transactions per second or retrievals per second.  For an audio streaming system, this measurement might focus on network I/O rate or concurrent sessions. For a key-value storage system, this measurement might be transactions and retrievals per second.

By monitoring user interaction and traffic in the service, SRE teams can usually figure out the user experience with the service and how it’s affected by shifts in the service's demand. 

## Errors (Rate of Failed Requests)
Error is the rate of requests that fail in any of the following ways: 

* **Explicitly:** for example, HTTP 500 internal server error.
* **Implicitly:** for example, HTTP 200 success response coupled with inaccurate content.
* **By policy** for example, as your response time is set to one second, any request that takes over one second is considered an error.

SRE teams can monitor all errors across the system and at individual service levels to define which errors are critical and which are less severe. By identifying that, they determine the health of their system from the user’s perspective and can take rapid action to fix frequent errors.

## Saturation
Saturation refers to the overall capacity of the service or how “full” the service is at a given time. It signifies how much memory or CPU resources your system is utilizing. Many systems start underperforming before they reach 100% utilization. Therefore, setting a utilization target is critical as it will help ensure the service performance and availability to the users.

An increase in latency is often a leading indicator of saturation. Measuring your 99th percentile response time over a small time period can provide an early indicator of saturation. For example, a 99th percentile latency of 60 ms indicates that there's a 60 ms delay for every one in 100 requests. 