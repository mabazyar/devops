1. `up`: Returns a binary vector indicating whether a target is up or down.

2. `node_cpu_seconds_total`: Returns the total amount of CPU time used by a node, in seconds.

3. `node_memory_MemTotal_bytes`: Returns the total amount of memory available on a node, in bytes.

4. `node_filesystem_avail_bytes`: Returns the amount of available disk space on a node's file system, in bytes.

5. `node_network_receive_bytes_total`: Returns the total number of bytes received by a node's network interface.

6. `node_network_transmit_bytes_total`: Returns the total number of bytes transmitted by a node's network interface.
7. `rate(node_cpu_seconds_total[5m])`: Returns the per-second rate of CPU usage over the last 5 minutes.
8. `sum(node_memory_MemFree_bytes)`: Returns the total amount of free memory across all nodes.
9. `max_over_time(node_cpu_seconds_total[1h])`: Returns the maximum CPU usage over the last hour.
10. `histogram_quantile(0.99, sum(rate(node_network_receive_bytes_total[5m])) by (instance))`: Returns the 99th percentile of network traffic received by each instance over the last 5 minutes.
11. `sum_over_time(container_memory_working_set_bytes[1d])`: Returns the total memory usage of all containers over the last day.
12. `sum without (instance)(node_cpu_seconds_total{job="node"})`: Returns the total CPU usage across all nodes, excluding the instance label.
13. `max without (job,instance)(node_cpu_seconds_total)`: Returns the maximum CPU usage across all nodes, excluding the job and instance labels.
14. `topk(5, sum by (job,instance)(container_cpu_usage_seconds_total))`: Returns the top 5 containers by CPU usage, grouped by job and instance.
15. `avg by (job)(rate(node_cpu_seconds_total[5m]))`: Returns the average CPU usage per job over the last 5 minutes.
16. `sum by (status)(kube_pod_status_phase{namespace="default"})`: Returns the number of pods in the default namespace by phase (e.g. Running, Pending, Failed).
17. `sum by (job,instance)(rate(container_cpu_usage_seconds_total{container_name!="POD"}[5m]))`: Returns the total CPU usage across all non-POD containers by job and instance over the last 5 minutes.
18. `max without (instance)(container_memory_working_set_bytes{container_name!="POD"})`: Returns the maximum memory usage across all non-POD containers, excluding the instance label.
19. `sum without (instance)(irate(node_cpu_seconds_total{mode="idle"}[5m]))`: Returns the total CPU idle time across all nodes over the last 5 minutes.
20. `count_over_time(http_requests_total{status_code="500"}[5m])`: Returns the total number of HTTP 500 errors over the last 5 minutes.
