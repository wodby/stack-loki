# Grafana Loki application stack for Kubernetes on Wodby

Deploy Grafana Loki applications on Kubernetes with Wodby.

This repository defines the Wodby stack manifests and default service
composition for Grafana Loki.

<!-- wodby:generated:start -->

## Stack contract

- [Grafana Loki stack on Wodby](https://wodby.com/stacks/loki)
- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Grafana Loki stack guide](https://wodby.com/docs/2.0/stacks/catalog/loki/)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Service definitions

- [Grafana Loki service](https://github.com/wodby/service-loki)
- [OpenTelemetry Collector service](https://github.com/wodby/service-opentelemetry-collector)
- [Grafana service](https://github.com/wodby/service-grafana)

## What's included

| Component / service | Default configuration |
| --- | --- |
| Grafana Loki<br>`loki` | required; enabled by default; volumes: `data` 20 GB |
| OpenTelemetry Collector<br>`otel-collector` | optional; enabled by default; links: `loki` → `loki` |
| Grafana<br>`grafana` | optional; enabled by default; volumes: `data` 10 GB; links: `loki` → `loki` |

Enabled optional services are selected by default but can be excluded when an
app is created. Disabled optional services are available but not selected by
default. Required services cannot be excluded.

## Validate the stack manifest

```bash
wodby stack validate-manifest stack.yml --org <org-id>
```

<!-- wodby:generated:end -->

## Connected experience

Grafana is enabled by default as the stack's main UI, with the internal Loki
service provisioned as its default data source. Loki remains required and its
HTTP endpoint remains private. Grafana can be excluded when only the Loki API
is needed.

Sign in to Grafana with the generated `admin_username` and `admin_password`
tokens from the Grafana service.

## Send logs

Applications send logs to the OpenTelemetry Collector over OTLP. Its HTTP port
has a public HTTPS domain, so applications in other environments, other
clusters or outside Wodby can use it. Every request needs the Collector's
generated `ingest_token`.

```
OTEL_EXPORTER_OTLP_ENDPOINT=https://<Collector domain>
OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf
OTEL_EXPORTER_OTLP_HEADERS=Authorization=Bearer%20<ingest token>
OTEL_SERVICE_NAME=<application name>
OTEL_LOGS_EXPORTER=otlp
OTEL_TRACES_EXPORTER=none
OTEL_METRICS_EXPORTER=none
```

The Collector accepts logs only and forwards them to Loki's OTLP endpoint.
Search them in Grafana by `service_name`. The Collector is optional: services
in the same environment can still push to Loki directly.
