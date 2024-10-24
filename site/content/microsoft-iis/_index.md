---
title: microsoft-iis
---

## Overview



{{< panel style="danger" >}}
Jsonnet source code is available at [github.com/grafana/jsonnet-libs](https://github.com/grafana/jsonnet-libs/tree/master/microsoft-iis-mixin)
{{< /panel >}}

## Alerts

{{< panel style="warning" >}}
Complete list of pregenerated alerts is available [here](https://github.com/monitoring-mixins/website/blob/master/assets/microsoft-iis/alerts.yaml).
{{< /panel >}}

### microsoft-iis

##### MicrosoftIISHighNumberOfRejectedAsyncIORequests

{{< code lang="yaml" >}}
alert: MicrosoftIISHighNumberOfRejectedAsyncIORequests
annotations:
  description: |
    The number of rejected async IO requests is {{ printf "%.0f" $value }} over the last 5m on {{ $labels.instance }} - {{ $labels.site }} which is above the threshold of 20.
  summary: There are a high number of rejected async I/O requests for a site.
expr: |
  increase(windows_iis_rejected_async_io_requests_total[5m]) > 20
for: 5m
labels:
  severity: warning
{{< /code >}}
 
##### MicrosoftIISHighNumberOf5xxRequestErrors

{{< code lang="yaml" >}}
alert: MicrosoftIISHighNumberOf5xxRequestErrors
annotations:
  description: |
    The number of 5xx request errors is {{ printf "%.0f" $value }} over the last 5m on {{ $labels.instance }} - {{ $labels.app }} which is above the threshold of 5.
  summary: There are a high number of 5xx request errors for an application.
expr: |
  sum without (pid, status_code)(increase(windows_iis_worker_request_errors_total{status_code=~"5.*"}[5m])) > 5
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### MicrosoftIISLowSuccessRateForWebsocketConnections

{{< code lang="yaml" >}}
alert: MicrosoftIISLowSuccessRateForWebsocketConnections
annotations:
  description: |
    The success rate for websocket connections is {{ printf "%.0f" $value }} over the last 5m on {{ $labels.instance }} - {{ $labels.app }} which is above the threshold of 80.
  summary: There is a low success rate for websocket connections for an application.
expr: |
  sum without (pid)  (increase(windows_iis_worker_websocket_connection_accepted_total[5m]) / clamp_min(increase(windows_iis_worker_websocket_connection_attempts_total[5m]),1)) * 100 > 80
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### MicrosoftIISThreadpoolUtilizationNearingMax

{{< code lang="yaml" >}}
alert: MicrosoftIISThreadpoolUtilizationNearingMax
annotations:
  description: |
    The threadpool utilization is at {{ printf "%.0f" $value }} over the last 5m on {{ $labels.instance }} - {{ $labels.app }} which is above the threshold of 90.
  summary: The thread pool utilization is nearing max capacity.
expr: |
  sum without (pid, state)(windows_iis_worker_threads / windows_iis_worker_max_threads) * 100 > 90
for: 5m
labels:
  severity: critical
{{< /code >}}
 
##### MicrosoftIISHighNumberOfWorkerProcessFailures

{{< code lang="yaml" >}}
alert: MicrosoftIISHighNumberOfWorkerProcessFailures
annotations:
  description: |
    The number of worker process failures is at {{ printf "%.0f" $value }} over the last 5m on {{ $labels.instance }} - {{ $labels.app }} which is above the threshold of 10.
  summary: There are a high number of worker process failures for an application.
expr: |
  increase(windows_iis_total_worker_process_failures[5m]) > 10
for: 5m
labels:
  severity: warning
{{< /code >}}
 
## Dashboards
Following dashboards are generated from mixins and hosted on github:


- [microsoft-iis-applications](https://github.com/monitoring-mixins/website/blob/master/assets/microsoft-iis/dashboards/microsoft-iis-applications.json)
- [microsoft-iis-overview](https://github.com/monitoring-mixins/website/blob/master/assets/microsoft-iis/dashboards/microsoft-iis-overview.json)
