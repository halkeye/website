---
title: sap-hana
---

## Overview



{{< panel style="danger" >}}
Jsonnet source code is available at [github.com/grafana/jsonnet-libs](https://github.com/grafana/jsonnet-libs/tree/master/sap-hana-mixin)
{{< /panel >}}

## Alerts

{{< panel style="warning" >}}
Complete list of pregenerated alerts is available [here](https://github.com/monitoring-mixins/website/blob/master/assets/sap-hana/alerts.yaml).
{{< /panel >}}

### sap-hana-alerts

##### SapHanaHighCpuUtilization

{{< code lang="yaml" >}}
alert: SapHanaHighCpuUtilization
annotations:
  description: The CPU usage is at {{$labels.value}}% on {{$labels.core}} on {{$labels.host}}
    which is above the threshold of 80%.
  summary: CPU utilization is high.
expr: |
  sum without (database_name) (hanadb_cpu_busy_percent) > 80
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### SapHanaHighPhysicalMemoryUsage

{{< code lang="yaml" >}}
alert: SapHanaHighPhysicalMemoryUsage
annotations:
  description: The physical memory usage is at {{$labels.value}}% on {{$labels.host}}
    which is above the threshold of 80%.
  summary: Current physical memory usage of the host is approaching capacity.
expr: |
  100 * sum without (database_name)(hanadb_host_memory_resident_mb) / sum without (database_name) (hanadb_host_memory_physical_total_mb) > 80
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### SapHanaMemAllocLimitBelowRecommendation

{{< code lang="yaml" >}}
alert: SapHanaMemAllocLimitBelowRecommendation
annotations:
  description: The memory allocation limit is set at {{$labels.value}}% on {{$labels.host}}
    which is below the recommended value of 90%.
  summary: Memory allocation limit set below recommended limit.
expr: |
  100 * sum without (database_name) (hanadb_host_memory_alloc_limit_mb) / sum without (database_name) (hanadb_host_memory_physical_total_mb) < 90
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### SapHanaHighMemoryUsage

{{< code lang="yaml" >}}
alert: SapHanaHighMemoryUsage
annotations:
  description: The memory usage is at {{$labels.value}}% on {{$labels.host}} which
    is above the threshold of 80%.
  summary: Current SAP HANA memory usage is approaching capacity.
expr: |
  100 * sum without (database_name) (hanadb_host_memory_used_total_mb) / sum without (database_name) (hanadb_host_memory_alloc_limit_mb) > 80
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### SapHanaHighDiskUtilization

{{< code lang="yaml" >}}
alert: SapHanaHighDiskUtilization
annotations:
  description: The disk usage is at {{$labels.value}}% on {{$labels.host}} which is
    above the threshold of 80%.
  summary: SAP HANA disk is approaching capacity.
expr: |
  100 * sum without (database_name, filesystem_type, path, usage_type) (hanadb_disk_total_used_size_mb) / sum without (database_name, filesystem_type, path, usage_type) (hanadb_disk_total_size_mb) > 80
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### SapHanaHighSqlExecutionTime

{{< code lang="yaml" >}}
alert: SapHanaHighSqlExecutionTime
annotations:
  description: The average SQL execution time is at {{$labels.value}}s on {{$labels.host}}
    which is above the threshold of 1s.
  summary: SAP HANA SQL average execution time is high.
expr: |
  avg without (database_name, port, service, sql_type) (hanadb_sql_service_elap_per_exec_avg_ms) / 1000 > 1
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### SapHanaHighReplicationShippingTime

{{< code lang="yaml" >}}
alert: SapHanaHighReplicationShippingTime
annotations:
  description: The average system replication log shipping delay is at {{$labels.value}}s
    from primary site {{$labels.site_name}} to replica site {{$labels.secondary_site_name}}
    which is above the threshold of 1s.
  summary: SAP HANA system replication log shipping delay is high.
expr: |
  avg without (database_name, port, secondary_port, replication_mode) (hanadb_sr_ship_delay) > 1
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### SapHanaReplicationStatusError

{{< code lang="yaml" >}}
alert: SapHanaReplicationStatusError
annotations:
  description: The replication status of replica {{$labels.secondary_site_name}} is
    ERROR
  summary: SAP HANA system replication status signifies an error.
expr: |
  hanadb_sr_replication == 4
for: 5m
labels:
  severity: critical
{{< /code >}}
 
## Dashboards
Following dashboards are generated from mixins and hosted on github:


- [sap-hana-instance-overview](https://github.com/monitoring-mixins/website/blob/master/assets/sap-hana/dashboards/sap-hana-instance-overview.json)
- [sap-hana-system-overview](https://github.com/monitoring-mixins/website/blob/master/assets/sap-hana/dashboards/sap-hana-system-overview.json)
