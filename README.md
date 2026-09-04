# Grafana Loki application stack for Kubernetes on Wodby

Deploy Grafana Loki applications on Kubernetes with Wodby.

This repository defines the Wodby stack manifests and default service
composition for Grafana Loki.

<!-- wodby:generated:start -->

## Stack contract

- [Grafana Loki stack on Wodby](https://wodby.com/stacks/loki)
- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Service definitions

- [Grafana Loki service](https://github.com/wodby/service-loki)
- [Grafana service](https://github.com/wodby/service-grafana)

## What's included

| Component / service | Default configuration |
| --- | --- |
| Grafana Loki<br>`loki` | required; enabled by default; volumes: `data` 20 GB |
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
