# 📉 gpu-insights-otel: OpenTelemetry for AI Inference Economics

An OpenTelemetry-native observability framework for mapping hardware-level GPU telemetry to LLM performance, designed to calculate the true Total Cost of Ownership (TCO) per Token in multi-cloud AI clusters.

---

## 🎯 The Problem
As AI inference scales, the cost bottleneck shifts from software licensing to raw compute capacity and power consumption. Traditional observability tools monitor whether a GPU is "up" or "down," but they fail to correlate hardware efficiency (thermal throttling, PCIe bandwidth, wattage) with LLM output (Tokens Per Second). 

Without this correlation, capacity planning across major cloud providers like Microsoft Azure and Google Cloud relies on guesswork, leading to massive over-provisioning or degraded user experiences.

---

## 🛠️ The Solution: Hardware-to-Token Observability
**gpu-insights-otel** bridges the gap between the silicon and the application layer. It utilizes the **OpenTelemetry (OTel) Collector** to scrape metrics from **NVIDIA DCGM (Data Center GPU Manager)** and correlates them with LLM inference logs inside **Splunk**.

This framework introduces a new target metric for AI Infrastructure economics: The **Inference Efficiency Score (IES)**.

$$IES = \frac{\text{Tokens Per Second (TPS)}}{\text{Kilowatt-Hours (kWh)} \times \text{GPU Utilization \%}}$$

### Core Capabilities
* **Compute Arbitrage:** Identifies whether it is more cost-effective to route inference to an underutilized A100 cluster or a highly utilized H100 cluster based on real-time power draw.
* **Thermal-Latency Correlation:** Detects when GPU thermal throttling directly impacts token generation latency, triggering automated alerts before SLA breaches occur.
* **OTel-Native Exporting:** Completely vendor-agnostic data collection, utilizing the Splunk OTel Exporter for downstream TCO analysis.

---

## 📊 The Architecture
1. **Telemetry Scraping:** NVIDIA DCGM Exporter exposes hardware metrics.
2. **OTel Collector:** Ingests DCGM metrics and LLM API gateway logs, enriching the data with Kubernetes/Node metadata.
3. **Strategic Visualization:** The Splunk HEC receiver ingests the OTel payloads to calculate the IES and provide real-time TCO analysis for capacity planners.



---

## 🚀 Getting Started

1. **Clone the Repository:**
   ```bash
   git clone [https://github.com/abokov/gpu-insights-otel.git](https://github.com/abokov/gpu-insights-otel.git)
   ```


2. **Configure the OTel Collector:**
Update the endpoint and token in deploy/otel-collector-config.yaml to point to your Splunk HEC instance.

Deploy via Docker Compose:

```bash
docker-compose up -d
```


---

## 📬 Contact & License

**Author:** Alexey Bokov  
**Contact:** [alex@bokov.net](mailto:alex@bokov.net)  
**License:** This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
