## Setup
* Monitoring Server:
  - Prometheus Server
  - Grafana Server
* Client Nodes:
  - Node Exporter

## Monitoring - Prometheus and Grafana
Q. What is Prometheus?
A. Prometheus is a whitebox monitoring and alerting tool that is used to collect and store metrics as time series data. It collects  precise metrics from configured targets at specified intervals, evaluates rule expressions, displays results, and triggers alerts if certain conditions are met.

Q. What prometheus does?
* Metric collection and store in TSDB (Time Series Database)
* Data querying using PromQL
* Alerting based on defined rules
* Visualization through integration with Grafana
* Pulls metrics from client machine by node exporter in frequent amount of time as defined in scrape interval config `scrape_interval: 15s`.

Q. How Prometheus collect data from node exporter?
A. Prometheus works in a pull model, meaning the Prometheus server periodically contacts each client (EC2 Machine) or exporter over HTTP, requests the /metrics endpoint, receives the current metrics data, and then stores that data in its internal time-series database, after which the stored metrics are used for querying, dashboards, and alerting

Q. What is the prometheus configuration file?
A. `/opt/prometheus/prometheus.yml` It can very be based on installation path.

Q. How many days prometheus stores data by default?
A. By default, Prometheus stores data for 15 days.

Q. Which database does Prometheus use to store time-series data?
A. Prometheus uses its own custom time-series database to store metrics data.

Note: Need to attach EC2ReadOnlyAccess policy IAM Role to the monitoring server role to pull metrics from other server.

Q. What is prometheus default port?
A. The default port for Prometheus is 9090.

Q. What is the prometheus service file location and service name?
A. The service file for Prometheus is typically located at `/etc/systemd/system/prometheus.service`, and the service name is `prometheus.service`.

Q. In which file prometheus keep the gather data from node exporter?
A. /opt/prometheus/data (directory where Prometheus stores its time-series data)

Q. What is Node Exporter?
Node Exporter helps to collect system-level metrics from the client nodes, such as CPU, memory, disk usage, and network statistics. Prometheus scrapes these metrics from the Node Exporter and stores them for analysis and visualization in Grafana.

Q. In which file node exporter metrics are available?
A. Node Exporter metrics are available at the `/metrics` endpoint. Node Exporter collects system information from the OS in real time and exposes the metrics at http://<server_ip>:9100/metrics, and when Prometheus accesses this URL, Node Exporter reads data from places like /proc and /sys, formats it as metrics, and returns it on the fly rather than saving it in any local file.

Q. What is node exporter default port?
A. The default port for Node Exporter is 9100. Allow this port from SG for Prometheus server to pull metrics.

Q. What is the node exporter service file location and service name?
A. The service file for Node Exporter is typically located at `/etc/systemd/system/node_exporter.service`, and the service name is `node_exporter.service`.

Q. What is Grafana?
A. Grafana is an open-source analytics and monitoring platform that integrates with various data sources, including Prometheus. It provides powerful visualization capabilities, allowing users to create interactive and customizable dashboards to monitor and analyze metrics data.

Q. What is Grafana default port?
A. The default port for Grafana is 3000.

Q. What is the Grafana service file location and service name?
A. The service file for Grafana is typically located at `/etc/systemd/system/grafana-server.service`, and the service name is `grafana-server.service`.

Q. What is scrape config in Prometheus?
A. The scrape config in Prometheus is defined in the `prometheus.yml` configuration file under the `scrape_configs` section. It specifies the targets (client nodes) from which Prometheus will collect metrics, along with the scrape interval and other settings. Example:
## This is EC2 service discovery example it will fetch the ec2 by tags.
```yamlscrape_configs:
- job_name: "ec2"
  ec2_sd_configs:
    - region: us-east-1
      port: 9100
      filters:
        - name: "tag:monitor"
          values: ["yes"]
```
## This is static config example
```yamlscrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['<client_node_ip>:9100']
```

Q. How do you add Prometheus as a data source in Grafana?
A. To add Prometheus as a data source in Grafana, follow these steps:
1. Log in to Grafana web interface.
2. Go to "Configuration" (gear icon) > "Data Sources."
3. Click "Add data source."
4. Select "Prometheus" from the list of data sources.
5. Configure the Prometheus URL (e.g., `http://<prometheus_server_ip>:9090`).
6. Click "Save & Test" to verify the connection.
7. Once saved, you can start creating dashboards using Prometheus metrics.

Q. What is the complete setup flow of Prometheus and Grafana for monitoring?
A.
1. Install Prometheus on a dedicated monitoring server.
2. Install Node Exporter on each client node (EC2 instances) to collect system metrics. Allow port 9100 in the security group of client nodes.
3. Configure Prometheus to scrape metrics from the Node Exporter instances by adding their IP addresses and ports (default 9100) in the `prometheus.yml` configuration file.
4. Start the Prometheus service to begin collecting metrics.
5. Install Grafana on the monitoring server. And allow port 3000 in the security group.
6. Add Prometheus as a data source in Grafana.
7. Create dashboards in Grafana to visualize the collected metrics.
8. Set up alerts in Prometheus or Grafana based on specific thresholds for proactive monitoring.

Note: Whenever alert triggered it will generate notification to the configured channel like email, slack, etc. Also it will help to create tickets in the incident management tools like Jira, ServiceNow, etc.