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
21. `rate(http_requests_total{status_code="200"}[5m])`: Returns the per-second rate of HTTP 200 responses over the last 5 minutes.
22. `increase(container_cpu_usage_seconds_total{container_name!="POD"}[5m])`: Returns the total CPU usage increase across all non-POD containers over the last 5 minutes.
23. `sum by (job)(increase(container_cpu_usage_seconds_total{container_name!="POD"}[5m]))`: Returns the total CPU usage increase per job across all non-POD containers over the last 5 minutes.
24. `sum without (instance,device)(node_disk_io_time_seconds_total{device!="sr0",device!="vda"})`: Returns the total disk I/O time across all nodes and devices, excluding the instance and device labels.
25. `avg_over_time(http_request_duration_seconds{status_code="200"}[1h])`: Returns the average request duration for HTTP 200 responses over the last hour.
26. `max_over_time(http_request_duration_seconds{status_code="500"}[1h])`: Returns the maximum request duration for HTTP 500 responses over the last hour.
27. `sum by (instance,job)(node_filesystem_size_bytes{fstype="ext4"})`: Returns the total disk size of all ext4 file systems by instance and job.
28. `histogram_quantile(0.99, sum(rate(http_request_duration_seconds_bucket{le="0.1"}[5m])) by (job,instance))`: Returns the 99th percentile of request durations less than or equal to 0.1 seconds by job and instance.
29. `max_over_time(process_cpu_seconds_total{process_name="myapp"}[1h])`: Returns the maximum CPU usage of the myapp process over the last hour.
30. `sum by (pod)(kube_pod_container_info{container_name!="POD"})`: Returns the total number of containers per pod, excluding POD containers.
31. `sum by (namespace)(kube_pod_status_phase{phase="Pending"})`: Returns the total number of pending pods by namespace.
32. `count_over_time(kube_pod_container_status_waiting_reason{reason="ImagePullBackOff"}[5m])`: Returns the total number of containers waiting due to an ImagePullBackOff error over the last 5 minutes.
33. `sum by (job)(rate(container_network_transmit_bytes_total{container_name!="POD"}[5m]))`: Returns the total network transmit rate across all non-POD containers by job.
34. `sum by (job)(irate(node_cpu_seconds_total{mode="user"}[5m]))`: Returns the total user CPU time across all nodes by job.
35. `avg by (job)(irate(node_cpu_seconds_total{mode="system"}[5m]))`: Returns the average system CPU time per job over the last 5 minutes.
36. `sum without (pod,namespace)(kube_pod_container_resource_requests_cpu_cores{container!="POD"})`: Returns the total CPU request across all non-POD containers, excluding the pod and namespace labels.
37. `max by (job,instance)(node_memory_MemAvailable_bytes{node_role="worker"})`: Returns the maximum available memory per node for nodes with the worker role, grouped by job and instance.
38. `sum without (pod,namespace)(kube_pod_container_resource_limits_memory_bytes{container!="POD"})`: Returns the total memory limit across all non-POD containers, excluding the pod and namespace labels.
39. `max without (pod,namespace)(kube_pod_container_resource_limits_cpu_cores{container!="POD"})`: Returns the maximum CPU limit across all non-POD containers, excluding the pod and namespace labels.
40. `sum without (pod,namespace)(kube_pod_container_resource_requests_memory_bytes{container!="POD"})`: Returns the total memory request across all non-POD containers, excluding the pod and namespace labels.
41. `histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket{le="0.1"}[5m])) by (job))`: Returns the 95th percentile of request durations less than or equal to 0.1 seconds by job.
42. `sum by (namespace)(kube_pod_container_status_restarts_total{reason="CrashLoopBackOff"})`: Returns the total number of restarts due to a CrashLoopBackOff error by namespace.
43. `sum by (namespace,controller)(kube_pod_container_status_restarts_total{reason="Error"})`: Returns the total number of restarts due to an error by namespace and controller.
44. `sum(rate(container_cpu_usage_seconds_total{container_name!="POD",mode!="idle"}[1m])) by (container_name)`: Returns the per-second rate of non-idle CPU usage across all non-POD containers by container_name.
45. `rate(container_cpu_usage_seconds_total{container_name!="POD",mode!="idle"}[5m]) > 0.8`: Returns a boolean indicating whether any non-POD containers have had a CPU usage rate above 80% over the last 5 minutes.
46. `sum(rate(http_request_duration_seconds_sum{status_code="200"}[5m])) / sum(rate(http_request_duration_seconds_count{status_code="200"}[5m]))`: Returns the average request duration for HTTP 200 responses over the last 5 minutes.
47. `sum without (pod)(kube_pod_status_ready{condition="true"})`: Returns the total number of ready pods, excluding the pod label.
48. `sum without (pod)(kube_pod_status_ready{condition="false"})`: Returns the total number of non-ready pods, excluding the pod label.
49. `sum by (namespace)(kube_pod_container_status_waiting{reason="ContainerCreating"})`: Returns the total number of containers waiting due to being in a ContainerCreating state by namespace.
50. `sum by (job)(rate(container_cpu_usage_seconds_total{container_name!="POD"}[5m])) / sum by (job)(kube_pod_container_resource_limits_cpu_cores{container!="POD"})`: Returns the total CPU usage as a percentage of the total CPU limit for each job.
51. `sum by (namespace)(kube_pod_status_phase{phase="Running"}) / sum by (namespace)(kube_pod_status_phase) * 100`: Returns the percentage of running pods per namespace.

52. `sum by (namespace)(kube_pod_status_phase{phase="Failed"}) / sum by (namespace)(kube_pod_status_phase) * 100`: Returns the percentage of failed pods per namespace.
53. `sum(rate(container_cpu_usage_seconds_total{container_name!="POD",mode!="idle"}[5m])) by (job) / sum(kube_pod_container_resource_limits_cpu_cores{container!="POD"}) by (job)`: Returns the total CPU usage as a percentage of the total CPU limit for each job.
54. `sum by (job)(kube_pod_container_resource_limits_memory_bytes{container!="POD"}) / sum by (job)(kube_pod_container_resource_requests_memory_bytes{container!="POD"})`: Returns the memory limit as a percentage of the memory request for each job.
55. `topk(10, sum by (pod)(container_cpu_usage_seconds_total{container_name!="POD",mode!="idle"}[5m])) by (job)`: Returns the top 10 pods by non-idle CPU usage over the last 5 minutes, grouped by job.
56. `max without (pod)(kube_pod_status_ready{condition="true"})`: Returns the maximum number of ready pods across all namespaces, excluding the pod label.
57. `max without (pod)(kube_pod_status_ready{condition="false"})`: Returns the maximum number of non-ready pods across all namespaces, excluding the pod label.
58. `max by (job)(irate(node_network_transmit_bytes_total{device="eth0"}[5m]))`: Returns the maximum network transmit rate for the eth0 device per job over the last 5 minutes.
59. `sum without (namespace,job)(kube_pod_container_status_restarts_total{reason="CrashLoopBackOff"})`: Returns the total number of restarts due to a CrashLoopBackOff error across all namespaces and jobs.
60. `max without (pod)(kube_pod_container_resource_limits_memory_bytes{container!="POD"})`: Returns the maximum memory limit across all non-POD containers, excluding the pod label.
61. `sum without (pod)(kube_pod_container_resource_limits_memory_bytes{container!="POD"})`: Returns the total memory limit across all non-POD containers, excluding the pod label.
62. `max by (job)(rate(container_network_transmit_bytes_total{container_name!="POD"}[5m]))`: Returns the maximum network transmit rate across all non-POD containers per job over the last 5 minutes.
63. `sum by (namespace)(kube_pod_status_phase{phase="Succeeded"}) / sum by (namespace)(kube_pod_status_phase) * 100`: Returns the percentage of succeeded pods per namespace.
64. `sum by (namespace,controller)(kube_pod_container_status_restarts_total{reason="BackoffLimitExceeded"})`: Returns the total number of restarts due to reaching the backoffLimit by namespace and controller.
65. `sum by (job)(kube_pod_container_resource_requests_cpu_cores{container!="POD"}) / sum by (job)(kube_pod_container_resource_limits_cpu_cores{container!="POD"})`: Returns the CPU request as a percentage of the CPU limit for each job.
66. `sum by (namespace)(kube_pod_container_status_waiting{reason="CrashLoopBackOff"})`: Returns the total number of containers waiting due to a CrashLoopBackOff error by namespace.
67. `sum by (job)(irate(container_cpu_usage_seconds_total{container_name!="POD",mode!="idle"}[5m])) / sum by (job)(kube_pod_container_resource_limits_cpu_cores{container!="POD"})`: Returns the CPU usage as a percentage of the CPU limit for each job.
68. `sum by (pod)(kube_pod_container_status_restarts_total{reason="CrashLoopBackOff"}) / sum by (pod)(kube_pod_container_status_restarts_total) * 100`: Returns the percentage of restarts due to a CrashLoopBackOff error per pod.
69. `max by (job)(sum by (pod)(container_cpu_usage_seconds_total{container_name!="POD",mode!="idle"}[5m]))`: Returns the maximum total non-idle CPU usage across all pods per job over the last 5 minutes.
70. `max by (job)(sum by (namespace,pod)(kube_pod_container_status_restarts_total{reason="CrashLoopBackOff"}))`: Returns the maximum total number of restarts due to a CrashLoopBackOff error per namespace and pod.
71. `histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket{le="0.1"}[5m])) by (job,instance))`: Returns the 95th percentile of request durations less than or equal to 0.1 seconds by job and instance.
72. `avg by (namespace)(kube_pod_status_ready{condition="true"})`: Returns the average number of ready pods per namespace, excluding the pod label.
73. `avg by (namespace)(kube_pod_status_ready{condition="false"})`: Returns the average number of non-ready pods per namespace, excluding the pod label.
74. `sum(rate(container_cpu_usage_seconds_total{container_name!="POD",mode!="idle"}[5m])) by (namespace) / sum(kube_pod_container_resource_limits_cpu_cores{container!="POD"}) by (namespace)`: Returns the total CPU usage as a percentage of the total CPU limit for each namespace.
75. `max by (namespace,controller)(kube_pod_container_status_restarts_total{reason="Error"})`: Returns the maximum total number of restarts due to an error per namespace and controller.
76. `max by (job)(rate(container_network_receive_bytes_total{container_name!="POD"}[5m]))`: Returns the maximum network receive rate across all non-POD containers per job over the last 5 minutes.
77. `sum(rate(container_cpu_usage_seconds_total{container_name!="POD",mode!="idle"}[5m])) by (namespace) / sum(kube_pod_container_resource_limits_cpu_cores{container!="POD"}) by (namespace)`: Returns the CPU usage as a percentage of the CPU limit for each namespace.
78. `max by (namespace)(kube_pod_container_resource_limits_memory_bytes{container!="POD"})`: Returns the maximum memory limit across all non-POD containers per namespace.
79. `sum(rate(container_cpu_usage_seconds_total{container_name!="POD",mode!="idle"}[5m])) by (controller) / sum(kube_pod_container_resource_limits_cpu_cores{container!="POD"}) by (controller)`: Returns the CPU usage as a percentage of the CPU limit for each controller.
80. `max by (job)(irate(node_network_receive_bytes_total{device="eth0"}[5m]))`: Returns the maximum network receive rate for the eth0 device per job over the last 5 minutes.
81. `sum by (namespace,controller)(kube_pod_container_status_waiting{reason="CrashLoopBackOff"})`: Returns the total number of containers waiting due to a CrashLoopBackOff error by namespace and controller.
82. `sum by (namespace,controller)(kube_pod_container_resource_requests_cpu_cores{container!="POD"}) / sum by (namespace,controller)(kube_pod_container_resource_limits_cpu_cores{container!="POD"})`: Returns the CPU request as a percentage of the CPU limit for each namespace and controller.
83. `sum without (instance,device)(node_disk_io_time_seconds_total{device!="sr0",device!="vda"}) by (namespace)`: Returns the total disk I/O time across all nodes and devices, excluding the instance and device labels, by namespace.
84. `max by (job,instance)(node_memory_MemAvailable_bytes{node_role="worker"}) / 1024 / 1024 / 1024`: Returns the maximum available memory for worker nodes, converted from bytes to gigabytes, grouped by job and instance.
85. `sum by (pod)(kube_pod_container_status_waiting{reason="ContainerCreating"}) / sum by (pod)(kube_pod_container_status_waiting) * 100`: Returns the percentage of containers waiting due to being in a ContainerCreating state per pod.
86. `sum without (pod,namespace)(kube_pod_container_resource_requests_cpu_cores{container!="POD"})`: Returns the total CPU request across all non-POD containers, excluding the pod and namespace labels.
87. `sum by (namespace,controller)(kube_pod_container_status_waiting{reason="Error"})`: Returns the total number of containers waiting due to an error by namespace and controller.
88. `sum by (namespace)(kube_pod_container_status_waiting{reason="ContainerCreating"})`: Returns the total number of containers waiting due to being in a ContainerCreating state by namespace.
89. `max without (namespace,pod)(kube_pod_container_status_restarts_total{reason="CrashLoopBackOff"})`: Returns the maximum number of restarts due to a CrashLoopBackOff error across all pods and namespaces, excluding the namespace and pod labels.
90. `max by (namespace,controller)(kube_pod_container_status_restarts_total{reason="CrashLoopBackOff"})`: Returns the maximum total number of restarts due to a CrashLoopBackOff error per namespace and controller.
91. `sum(rate(container_memory_working_set_bytes{container_name!="POD"}[5m])) by (job) / sum(kube_pod_container_resource_limits_memory_bytes{container!="POD"}) by (job)`: Returns the memory usage as a percentage of the memory limit for each job.
92. `max by (job,instance)(node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes{node_role="worker"}) / 1024 / 1024 / 1024`: Returns the maximum used memory for worker nodes, converted from bytes to gigabytes, grouped by job and instance.
93. `sum by (namespace,controller)(kube_pod_container_resource_requests_memory_bytes{container!="POD"}) / sum by (namespace,controller)(kube_pod_container_resource_limits_memory_bytes{container!="POD"})`: Returns the memory request as a percentage of the memory limit for each namespace and controller.
94. `sum(rate(container_network_transmit_bytes_total{container_name!="POD"}[5m])) by (job)`: Returns the per-second network transmit rate across all non-POD containers, grouped by job.
95. `sum(rate(container_memory_working_set_bytes{container_name!="POD"}[5m])) by (controller) / sum(kube_pod_container_resource_limits_memory_bytes{container!="POD"}) by (controller)`: Returns the memory usage as a percentage of the memory limit for each controller.
96. `max by (job,instance)(node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes{node_role="worker"}) / node_memory_MemTotal_bytes{node_role="worker"} * 100`: Returns the maximum memory utilization for worker nodes as a percentage, grouped by job and instance.
97. `sum(rate(container_network_receive_bytes_total{container_name!="POD"}[5m])) by (controller)`: Returns the per-second network receive rate across all non-POD containers, grouped by controller.
98. `sum by (namespace,controller)(kube_pod_container_status_terminated{reason="OOMKilled"})`: Returns the total number of containers terminated due to an OOMKilled error by namespace and controller.
99. `sum by (namespace,controller)(kube_pod_container_resource_limits_cpu_cores{container!="POD"})`: Returns the total CPU limit across all non-POD containers, grouped by namespace and controller.
100. `max by (namespace,controller)(kube_pod_container_status_terminated{reason="Error"})`: Returns the maximum total number of containers terminated due to an error per namespace and controller.
