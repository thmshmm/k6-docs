---
title: 'Inspect test results'
description: 'Grafana Cloud k6 provides views that visualize, analyze, and present trends for the test results. This topic describes the primary sections of the Test Result view.'
weight: 201
---

# Inspect test results

When running a cloud test, Grafana Cloud stores the test metrics generated by the k6 instances running the test. Grafana Cloud k6 provides views that visualize, analyze, and present trends for the test results. 

This topic describes the primary sections of the Test Result view, also known as Test Run view.

## Navigate to the Test Result view

You can navigate to the Test Result view from different flows. From the project view:

1. **Select the test** that want to view its test run results.
2. On the Test view, **select the test run** from the Test Runs table.

![Test view - Test runs table](/media/docs/k6/screenshot-grafana-cloud-dashboard-test-runs.png)

## Performance overview

The Performance Overview section displays high-level data for your test. While the test runs, it displays live metrics.

![Test Results view - Performance Overview](/media/docs/k6/screenshot-grafana-cloud-results-performance-overview.png)

### Indicators of good and bad results

The first sign of a good or bad result is usually in the Performance Overview panel.

Typical signs of a good result:

- Response time has a flat trend for the duration of the test.
- Request rates follow the same ramping pattern as Virtual Users (if VUs increase, so does the request rate).

Typical signs of a performance issue or bottleneck:

- Response times rise during the test.
- Response times rise, then quickly bottom out and stay flat.
- Request rates do not rise with VUs (and response times start to increase).

This is a non-exhaustive list. You should use these patterns as a first indicator of good or bad performance of your test.

Below the Performance overview panel, the Performance Insights section lists any issues that the cloud service algorithms detect. Read more on [Performance Insights]({{< relref "./get-performance-insights" >}}).

## Inspect request data: HTTP, WebSocket, or gRPC

Each HTTP, WebSocket, or gRPC data provides information about the originated k6 request and the response from the system under test (SUT). The response data gives performance information, such as latency and error, about how the system behaves during the test execution.

You might want to inspect the requests by different metrics to analyze the test results. For example:
 - slowest requests.
 - most failed requests.
 - behavior at a specific period.

If you want to visualize or analyze HTTP request results, click the **HTTP** tab: 

![HTTP Requests Table](/media/docs/k6/screenshot-grafana-cloud-http-requests-table.png)

> The WebSocket and gRPC tabs are available if your test run contains requests towards a WebSocket or a gRPC service, respectively. Otherwise, they are inaccessible.

To inspect a request in detail:
1. Select its row.
2. In the expanded row, inspect the:
    - Additional metrics for the request.
    - Options to change the aggregation.
    - To see HTTP metrics from a specific load zone, use the drop down.

![HTTP Request panel](/media/docs/k6/screenshot-grafana-cloud-http-request.png)

## Inspect test data

On the Results tab, you can dig into the specific result from your k6 t4ests:

- **Thresholds**: list the [Thresholds](https://k6.io/docs/using-k6/thresholds/) generated by the test run in the order defined in your script.
- **Checks**: list the [Checks](https://k6.io/docs/using-k6/checks/) generated by the test run, organized into Groups (if used).
- **Logs**: list the logs generated by the test run.

### Thresholds

To visually inspect and analyze [Thresholds](https://k6.io/docs/using-k6/thresholds/), click the **Threshold** tab.

First, check the number of failed thresholds. You should analyze failed thresholds to fix the underlying issues on your system or customize the Threshold settings.

![Thresholds Tab](/media/docs/k6/thresholds-failed.png)

To find failing thresholds, use the search bar. You can filter by name or by pass/fail status.

1. Search for one or multiple names, like `http` or `vus`.
1. From your filters, find which thresholds. For example, `vus: value>100` and `http_reqs:count>10000`.

   Note the <span style="color:RGB(209, 122, 14)">red</span> or <span style="color:RGB(26, 127, 75)">green</span> markings on the left side of each row.

To **see visualizations for the metric**, select the threshold and inspect the graph. 

![Thresholds passed](/media/docs/k6/thresholds-tab.png)

### Checks

To visually inspect and analyze [Checks](https://k6.io/docs/using-k6/checks/), click the **Checks** tab.

First, analyze the number of checks that passed against the total number of checks. This provides a quick overview of the check status.

![Checks Tab](/media/docs/k6/checks-tab.png)

To find failed checks, search for a check or sort columns.
  - Note the <span style="color:RGB(209, 122, 14)">red</span> or <span style="color:RGB(26, 127, 75)">green</span> on the left side of each row.
  - In the preceding image, the Check `HTTP status is 200` succeeded only 84.57% of the time: 137 successful runs and 25 failures.

To inspect a check in detail:
1. Select the Check row.
1. In the expanded check, note how many failures occur at different points in the test.

### Logs

Logging messages can help you debug load tests. The k6 API supports the following console-logging methods:

- `console.log()`
- `console.info()`
- `console.debug()`: output the logs only when running k6 with the `-v/--verbose` flag.
- `console.warn()`
- `console.error()`


> **Tip: Debug locally first**
>
> First, run your test locally `k6 run my-test.js` for testing and debugging. When your script is ready, execute the cloud test with the `k6 cloud` command.

With the **Logs** Tab, you can view, filter, and query log messages in the Cloud Results page.

To filter messages by severity:
1. Use the **Log level** dropdown.
1. Order each JavaScript log statement by the severity level however you want:

  - **Info**: `console.log` and `console.info`.
  - **Debug**: `console.debug`.
  - **Warning**: `console.warning`.
  - **Error**: `console.error`.

The load-zone filter displays only when your test runs in more than one load zone.