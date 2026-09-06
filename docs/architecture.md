
# Architecture

This lab is built around OpenAirInterface components and is divided into three main parts:

1. 5G Core
2. RAN and UE
3. Monitoring

## 1. 5G Core

The 5G Core is deployed with Docker using the OAI CN5G project.

The main network functions used in the lab are:

- AMF
- SMF
- UPF
- NRF
- AUSF
- UDM
- UDR

Each network function runs in its own Docker container.

For this setup, the Core is started using the OAI Docker Compose configuration.

## 2. RAN and UE

The radio side uses OpenAirInterface directly.

The gNB is started using `nr-softmodem`.

The UE is started using `nr-uesoftmodem`.

Instead of physical SDR hardware, the connection between the gNB and UE is simulated using the OAI RF Simulator.

The basic path is:

OAI UE
   |
   | RF Simulator
   |
OAI gNB
   |
   | N2 / N3
   |
OAI 5G Core

The gNB communicates with the AMF over N2 for control-plane signaling and with the UPF over N3 for user-plane traffic.

## 3. Monitoring

The monitoring setup is separate from the 5G signaling path.

It is mainly used to monitor the Docker containers running the Core.

OAI Core Containers
        |
     cAdvisor
        |
    Prometheus
        |
      Grafana

cAdvisor collects container metrics.

Prometheus periodically scrapes and stores these metrics.

Grafana reads data from Prometheus and displays it in dashboards.

The current dashboard setup focuses on infrastructure-level metrics such as:

- CPU usage
- Memory usage
- Network RX/TX
- Disk I/O

Later, the monitoring setup can be extended with 5G-specific metrics such as UE registration, PDU sessions, PFCP status, and UPF traffic.

## Lab Notes

This setup does not depend on physical radio hardware.

Using the OAI RF Simulator makes it possible to test the OAI UE and gNB in a virtualized lab environment before moving to SDR-based tests.
