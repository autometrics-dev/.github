# Autometrics 📈✨

Autometrics is a set of open source libraries that make it easy to understand the performance of your code in production.

The libraries track the most useful metrics (request and error rate and latency) of any function, generate queries to help you understand the data collected, and insert links to the live charts directly into each function's doc comments.

Autometrics is built on the excellent [Prometheus](https://prometheus.io/) and [OpenTelemetry](https://opentelemetry.io/) open source projects.

---

<!-- vscode-markdown-toc -->
* 1. [Getting Started](#GettingStarted)
* 2. [Implementations](#Implementations)
* 3. [Why Metrics?](#WhyMetrics)
	* 3.1. [Too many logs!](#Toomanylogs)
	* 3.2. [Metrics](#Metrics)
	* 3.3. [Metrics today are hard to use](#Metricstodayarehardtouse)
* 4. [Why Autometrics?](#WhyAutometrics)
	* 4.1. [Simplifying code-level observability](#Simplifyingcode-levelobservability)
	* 4.2. [Standardizing function-level metrics](#Standardizingfunction-levelmetrics)
	* 4.3. [Labeling metrics with useful, low-cardinality function details](#Labelingmetricswithusefullow-cardinalityfunctiondetails)
	* 4.4. ["Tracing Lite"](#TracingLite)
* 5. [Configuring Prometheus](#ConfiguringPrometheus)
	* 5.1. [Local Prometheus (For Testing)](#LocalPrometheusForTesting)
	* 5.2. [Fly.io](#Fly.io)
	* 5.3. [Kubernetes](#Kubernetes)
* 6. [Contributing](#Contributing)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

---

##  1. <a name='GettingStarted'></a>Getting Started

1. Add autometrics to your app using one of the [implementations](#implementations)
2. Add a `/metrics` endpoint that exports the collected metrics (see the docs for your language's autometrics library for exactly how to do this)
3. [Run and configure Prometheus](#configuring-prometheus) to scrape the metrics from your app

##  2. <a name='Implementations'></a>Implementations

- [Rust](https://github.com/autometrics-dev/autometrics-rs)
- (Coming Very Soon!) TypeScript
- (Coming Soon!) Python
- (Coming Soon!) Go
- (Coming Soon!) Ruby

Want to help implement autometrics in another language? Get in touch! We'd love to work with you on it.

##  3. <a name='WhyMetrics'></a>Why Metrics?

###  3.1. <a name='Toomanylogs'></a>Too many logs!

Logs are a great. We all use them. Printing a log line is the first thing we learn how to do in any new programming language. But our industry places too much emphasis on logs today.

The two main problems with logs are understandability and cost. When running a system in production, it can be hard to find the information you need in the mountains of logs you have probably collected. There's tons of noise for the amount of signal. Of course, many vendors promise to sift through your data to find useful insights -- for a price, of course. Processing and storing logs is not cheap!

Can we do better?

###  3.2. <a name='Metrics'></a>Metrics

Metrics are the next tool in the observability stack most teams turn to. Counting things. The good part about counting things is that it's a lot less expensive than producing and processing tons of text. And, they can help give a clear, overall understanding of how your system is performing.

While metrics aren't a panacea, a core hypothesis of the autometrics project is that **metrics are underused relative to their potential usefulness and cost-efficiency**.

###  3.3. <a name='Metricstodayarehardtouse'></a>Metrics today are hard to use

Metrics are a powerful and relatively inexpensive tool for understanding your system in production.

However, they are still hard to use. Developers need to:
- Think about what metrics to track and which metric type to use (counter, histogram... 😕)
- Figure out how to write PromQL or another query language to get some data 😖
- Verify that the data returned actually answers the right question 😫

##  4. <a name='WhyAutometrics'></a>Why Autometrics?

###  4.1. <a name='Simplifyingcode-levelobservability'></a>Simplifying code-level observability

Many modern observability tools promise to make life "easy for developers" by automatically instrumenting your code with an agent or eBPF. Others ingest tons of logs or traces -- and charge high fees for the processing and storage.

Most of these tools treat your system as a black box and use complex and pricey processing to build up a model of your system. This, however, means that you need to map their model onto your mental model of the system in order to navigate the mountains of data.

Autometrics takes the opposite approach. Instead of throwing away valuable context and then using compute power to recreate it, it starts inside your code. It enables you to understand your production system at one of the most fundamental levels: from the function.

###  4.2. <a name='Standardizingfunction-levelmetrics'></a>Standardizing function-level metrics

Functions are one of the most fundamental building blocks of code. Why not use them as the building block for observability?

A core part of autometrics is the simple idea of using standard metric names and a consistent scheme for tagging/labeling metrics.

The three metrics currently used are:
- `function.calls.count`
- `function.calls.duration`
- `function.calls.concurrent`.

###  4.3. <a name='Labelingmetricswithusefullow-cardinalityfunctiondetails'></a>Labeling metrics with useful, low-cardinality function details

The following labels are added automatically to all three of the metrics:
- `function`
- `module`

For the function call counter, the following labels are also added:

- `caller` - (see ["Tracing Lite"](#tracing-lite) below)
- `result` - either `ok` or `error`
- `ok` / `error` - the name of the concrete type of the returned value or error can also be attached as a label

Autometrics aims to only support static values as labels to avoid the footgun of attaching labels with too many possible values. The [Prometheus docs](https://prometheus.io/docs/practices/naming/#labels) explain why this is important in the following warning:

> CAUTION: Remember that every unique combination of key-value label pairs represents a new time series, which can dramatically increase the amount of data stored. Do not use labels to store dimensions with high cardinality (many different label values), such as user IDs, email addresses, or other unbounded sets of values.

###  4.4. <a name='TracingLite'></a>"Tracing Lite"

A slightly unusual idea baked into autometrics is that by tracking more granular metrics, you can debug some issues that we would traditionally need to turn to tracing for.

Autometrics can be added to any function in your codebase, from HTTP handlers down to database methods.

This means that if you are looking into a problem with a specific HTTP handler, you can browse through the metrics of the functions _called by the misbehaving function_.

Simply hover over the function names of the nested function calls in your IDE to look at their metrics. Or, you can directly open the chart of the request or error rate of all functions called by a specific function.

##  5. <a name='ConfiguringPrometheus'></a>Configuring Prometheus

Once you have your code instrumented with autometrics, you'll need to run [Prometheus](https://prometheus.io/) and point it at your app.

Prometheus is pull-based, meaning it will poll your app to scrape the metrics collected.

###  5.1. <a name='LocalPrometheusForTesting'></a>Local Prometheus (For Testing)

1. Download Prometheus
   - Homebrew on MacOS: `brew install prometheus`
   - Docker: `docker pull prom/prometheus`
   - [Pre-compiled binary](https://prometheus.io/download/)
2. Create a `prometheus.yml` file:
```yaml
scrape_configs:
  - job_name: my-app
    metrics_path: /metrics
    static_configs:
      # Replace the port with the port your app listens on
      - targets: ['localhost:3000']
    # For a real deployment, you would want the scrape interval to be
    # longer but for testing, you want the data to show up quickly
    scrape_interval: 200ms
```
3. Run Prometheus with the config file:
```shell
prometheus --config.file="prometheus.yml"
```
Or with Docker:
```shell
docker run \
    -p 9090:9090 \
    -v prometheus.yml:/etc/prometheus/prometheus.yml \
    prom/prometheus
```

###  5.2. <a name='Fly.io'></a>Fly.io

[Fly.io](https://fly.io) provides a hosted Prometheus instance for every project.

To have Fly's Prometheus scrape your autometrics-instrumented app, simply add this to your `fly.toml`:
```toml
# fly.toml

app = "your-app-name"

[metrics]
port = 9091 # default for most prometheus clients
path = "/metrics" # default for most prometheus clients
```

###  5.3. <a name='Kubernetes'></a>Kubernetes

Prometheus supports service discovery, so it can automatically find Kubernetes pods to scrape.

If you have Prometheus installed using the [helm chart](https://github.com/prometheus-community/helm-charts), service discovery should be enabled by default.

Add the following annotations to your app's deployment:

```yaml
# deployment.yml

apiVersion: apps/v1
kind: Deployment
spec:
  template:
    metadata:
      annotations:
        prometheus.io/scrape: "true"
        prometheus.io/path: "/metrics"
        prometheus.io/port: "9091"
```

##  6. <a name='Contributing'></a>Contributing

We would love to have you! If you have ideas, suggestions, questions, or would like to pitch in, please open up an issue or submit a PR to any of the repos!
