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