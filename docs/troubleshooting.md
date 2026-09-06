# Troubleshooting Notes

This document summarizes the main issues encountered during the development of this OAI 5G laboratory setup and the solutions applied.

---

## 1. Docker image pull problems

### Problem

Some Docker images could not be downloaded because of registry access issues.

Examples:

- `gcr.io/cadvisor/cadvisor`
- `ghcr.io/google/cadvisor`

Errors were related to registry access and TLS connection problems.

### Solution

The cAdvisor image source was changed to:

```text
ghcr.io/google/cadvisor:latest
```

After successfully pulling the image, cAdvisor was added to the monitoring stack.

---

## 2. Grafana container restart issue

### Problem

Grafana entered a restart loop because port `3000` was already occupied.

Error:

```text
failed to open listener on address 0.0.0.0:3000:
bind: address already in use
```

### Solution

The process using port 3000 was identified and the conflicting Grafana instance was removed/restarted.

Grafana was then successfully started.

---

## 3. Prometheus and cAdvisor integration

### Problem

The monitoring stack initially did not collect container-level metrics.

### Solution

cAdvisor was added to the monitoring environment and configured as a Prometheus target.

The monitoring stack was verified using:

- Prometheus targets page
- cAdvisor metrics endpoint

Verified components:

- Prometheus: UP
- cAdvisor: UP

---

## 4. Handling subscriber authentication data

### Problem

The UE configuration contains subscriber authentication parameters:

```text
imsi
key
opc
```

### Solution

Real authentication values were not published.

Only placeholder values are stored in the public repository:

```text
imsi = "YOUR_IMSI";
key  = "YOUR_KEY";
opc  = "YOUR_OPC";
```

---

## 5. Virtualized 5G laboratory environment

### Problem

Running multiple 5G components inside a VirtualBox environment can introduce networking and resource-related issues.

### Solution

The following checks were used during troubleshooting:

- Docker container status
- Network connectivity
- Prometheus target availability
- Service health status

---

## 6. Monitoring limitations

### Current status

The current monitoring stack focuses mainly on infrastructure-level metrics:

- CPU usage
- Memory usage
- Network RX/TX
- Disk I/O

Future improvements can include:

- UE registration metrics
- PDU session monitoring
- UPF traffic statistics
- 5G-specific KPIs
