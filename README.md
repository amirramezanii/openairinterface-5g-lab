# OAI 5G Lab

This repository contains the configuration and monitoring setup I used while building a small 5G laboratory with OpenAirInterface.

The main goal of the project was to bring up an OAI-based 5G Core, run an OAI gNB and UE with RF simulation, and then add monitoring for the Dockerized network functions.

## What is included

The setup is based on:

- OpenAirInterface 5G Core
- OAI gNB
- OAI UE
- OAI RF Simulator
- Docker
- Prometheus
- cAdvisor
- Grafana

I used the RF Simulator instead of SDR hardware, so the gNB and UE can be tested without a USRP or other radio device.

The monitoring path is:

OAI Docker containers
        |
     cAdvisor
        |
    Prometheus
        |
      Grafana

## 5G Core

The 5G Core runs as Docker containers.

The main network functions used in this setup are:

- AMF
- SMF
- UPF
- NRF
- AUSF
- UDM
- UDR

The Core setup is based on the OAI CN5G Docker Compose environment.

## RAN and UE

For the radio side of the lab I used the OpenAirInterface implementations directly:

- `nr-softmodem` for the gNB
- `nr-uesoftmodem` for the UE

The RF connection between them is simulated by OAI RF Simulator.

No physical SDR is required for this setup.

## Monitoring

I added a small monitoring stack to observe the Docker containers running the 5G Core.

cAdvisor collects container-level metrics such as:

- CPU usage
- Memory usage
- Network RX/TX
- Disk I/O

Prometheus collects and stores these metrics, and Grafana is used to visualize them.

At the moment, CPU and memory monitoring are available for the OAI network functions. More 5G-specific metrics will be added later.

## Configuration files

The repository contains the gNB and UE configuration files used in the lab.

Subscriber authentication values are not published with their real values. Sensitive fields are replaced with placeholders, for example:
```text

imsi = "YOUR_IMSI";
key  = "YOUR_KEY";
opc  = "YOUR_OPC";
```

Real authentication keys, tokens, passwords, and private credentials should not be committed to a public repository.

## Current status

So far, the following parts have been completed:

- OAI 5G Core deployment with Docker
- OAI gNB setup
- OAI UE setup
- RF Simulator testing
- Prometheus setup
- cAdvisor setup
- Grafana setup
- CPU monitoring
- Memory monitoring
The next steps are mainly focused on connecting the RAN side to the Core and adding more 5G-specific monitoring, such as UE registration, PDU sessions, and UPF traffic.

## Notes

This repository contains my own configuration, monitoring setup, and project documentation.

The OpenAirInterface source code itself is not included here. For the original source code and licensing information, refer to the official OpenAirInterface repositories.
