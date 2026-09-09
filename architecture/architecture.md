# Architecture Overview

## System Components

### Wazuh Manager (Ubuntu VM)
- IP Address: 192.168.1.9 (configurable)
- Role: Endpoint detection and response (EDR)
- Key Functions:
  - Log collection from endpoints
  - Rule-based threat detection
  - File integrity monitoring
  - Vulnerability scanning
  - Alert generation

### Filebeat (Wazuh VM)
- Role: Log shipper
- Function: Reads alerts.json and forwards to Graylog
- Output: Graylog Beats input (port 5044)

### Graylog (Docker on Windows)
- IP Address: 192.168.1.6 (configurable)
- Role: Centralized log management and analysis
- Key Functions:
  - Log ingestion and storage
  - Log normalization and enrichment
  - Threat intelligence integration
  - Alerting and notifications
  - Dashboard visualization

### OpenSearch (Docker)
- Role: Log storage and indexing
- Internal Port: 9200
- Data: All indexed log messages

### MongoDB (Docker)
- Role: Metadata storage
- Internal Port: 27017
- Data: User configurations, streams, pipelines

## Data Flow

1. Wazuh agents collect logs from endpoints
2. Wazuh Manager processes logs and generates alerts
3. Alerts are written to /var/ossec/logs/alerts/alerts.json
4. Filebeat monitors alerts.json for changes
5. Filebeat forwards new alerts to Graylog on port 5044
6. Graylog receives alerts and applies pipeline rules
7. Pipeline rules enrich alerts with threat intelligence
8. Enriched alerts are stored in OpenSearch
9. Users query and visualize alerts via Graylog UI

## Network Ports

| Service | Port | Purpose |
|---------|------|---------|
| Graylog Web UI | 9000 | Web interface access |
| Graylog Beats Input | 5044 | Filebeat log reception |
| Graylog Syslog Input | 5514 | Syslog log reception |
| OpenSearch | 9200 | Internal indexing |
| MongoDB | 27017 | Internal metadata |