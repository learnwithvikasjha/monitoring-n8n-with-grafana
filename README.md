# Monitoring N8N with Grafana — Quick and simple

This repository contains a minimal, no‑fluff guide to monitor an n8n instance using Prometheus and Grafana.

Prerequisites
- n8n running (container, VM, or Kubernetes)
- Prometheus reachable from your n8n exporter or instrumented n8n
- Grafana to import and view the dashboard

1) Enable metrics in n8n
- Option A — run an n8n Prometheus exporter sidecar that exposes /metrics (e.g. http://n8n-exporter:9460/metrics).
- Option B — instrument n8n directly (prom-client) so n8n serves /metrics itself (e.g. http://n8n:9460/metrics).
- Verify the endpoint:
  - curl http://<host-or-service>:<port>/metrics
  - You should see Prometheus-format metrics (lines like my_metric_name{...} 123).

2) Monitor with Prometheus
Add this minimal snippet to prometheus.yml and point Prometheus to the metrics endpoint:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "n8n"
    metrics_path: /metrics
    static_configs:
      - targets: ["n8n-exporter:9460"]   # or ["n8n:9460"] if instrumented directly
```

Reload or restart Prometheus and confirm the n8n target is UP at Prometheus → Status → Targets.

3) Import the Grafana dashboard
- This repo already includes the Grafana dashboard JSON; import it in Grafana:
  - File: n8n Monitoring Dashboard-1761399249626.json
  - In Grafana: Dashboards → Import → Upload the JSON (or paste) → select your Prometheus datasource → Import.
- A dashboard screenshot is included for preview:
  - File: N8N-GRAFANA.png

Files in this repo
- n8n Monitoring Dashboard-1761399249626.json  ← Grafana dashboard JSON you can import
- N8N-GRAFANA.png                              ← dashboard screenshot preview

That's it — enable metrics in n8n, have Prometheus scrape them, and import the included Grafana dashboard JSON to visualize.
