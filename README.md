# Monitoring-AlertManager
Automated Bash script to install and configure Prometheus AMonitoring Stack: Prometheus, Grafana, Node Exporter, Alertmanager, and PagerDuty

This project provides an automated, script-based setup for a monitoring stack using Prometheus, Grafana, Node Exporter, Alertmanager, and PagerDuty. Each component is installed and configured via dedicated shell scripts, making it easy to deploy on a Unix-like system. The stack enables monitoring, visualization, alerting, and incident management.

Components Explained
1. Node Exporter (node.sh)

Purpose:
Node Exporter is an open-source tool that exposes system metrics (CPU, memory, disk, network, etc.) for Prometheus to scrape.

How the script works:

Download: Fetches the latest Node Exporter binary from GitHub.

Extract: Unpacks the downloaded archive.

Run: Starts Node Exporter as a systemd service, listening on port 9100.

Metrics Endpoint: Metrics are available at http://localhost:9100/metrics.

Usage:

./node.sh


Outcome:

Real-time system metrics for Prometheus.

Fully automated installation—no manual configuration required.

2. Prometheus & Grafana (grafana-prometheus.sh)

Prometheus

Purpose: A monitoring and alerting toolkit that scrapes metrics from Node Exporter and stores them in a time-series database.

Script Functionality: Downloads, extracts, and runs Prometheus (default port 9090). Configures it to scrape metrics from Node Exporter.

Access: http://localhost:9090

Grafana

Purpose: An open-source platform for visualizing metrics.

Script Functionality: Installs Grafana (default port 3000). After starting, you can add Prometheus as a data source and create dashboards.

Default Login: admin / admin

Access: http://localhost:3000

Usage:

./grafana-prometheus.sh


Outcome:

Prometheus server running and scraping Node Exporter metrics.

Grafana ready for dashboards and visualization.

3. Alertmanager (alertmanager.sh)

Purpose:
Alertmanager manages alerts sent by Prometheus. It handles grouping, deduplication, silencing, and routing to receivers like PagerDuty.

Workflow:

Prometheus detects an issue (e.g., high CPU).

Sends the alert to Alertmanager.

Alertmanager processes and sends notifications to the configured receivers.

Configuration:

Configured via alertmanager.yml.

Receivers can include email, Slack, or PagerDuty.

Usage:

./alertmanager.sh


Outcome:

Alertmanager service running on port 9093.

Alerts routed to PagerDuty (or other configured channels).

4. PagerDuty Integration

Purpose:
PagerDuty is an incident management platform. It receives alerts from Alertmanager and notifies the right personnel immediately.

Integration Steps:

Create a service in PagerDuty and obtain the integration key.

Configure Alertmanager (alertmanager.yml) with the PagerDuty key:

receivers:
  - name: 'pagerduty'
    pagerduty_configs:
      - routing_key: <YOUR_PAGERDUTY_INTEGRATION_KEY>


Ensure Prometheus sends alerts to Alertmanager.

Test by triggering a sample alert.

Benefits:

Automated alert delivery.

On-call escalation and incident tracking.

Step-by-Step Setup

Make scripts executable:

chmod +x node.sh grafana-prometheus.sh alertmanager.sh


Start Node Exporter:

./node.sh


Start Prometheus and Grafana:

./grafana-prometheus.sh


Start Alertmanager:

./alertmanager.sh


Access the services:

Node Exporter: http://localhost:9100/metrics

Prometheus: http://localhost:9090

Grafana: http://localhost:3000

Alertmanager: http://localhost:9093

Prerequisites

Unix-like OS (Linux, macOS, or WSL on Windows)

wget, curl, and tar installed

Permissions to run scripts and install binaries

Troubleshooting & Tips

If ports 9100, 9090, 3000, or 9093 are in use, stop conflicting services or change ports in the scripts.

For persistent monitoring, scripts can run as systemd services.

Grafana default login: admin/admin. Change password after first login.

Import community dashboards for quick visualization.

Stress Testing Commands:

sudo apt-get install stress-ng -y
stress-ng --cpu 2 --timeout 300


Custom Grafana Queries:

CloudWatch:

SELECT AVG(CPUUtilization) FROM "AWS/EC2"


Prometheus:

node_cpu_seconds_total{cpu="1"}

License

MIT Licenselertmanager and Node Exporter on Linux.
