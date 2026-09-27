<p align="center">
  <img src="https://raw.githubusercontent.com/logshed/logshed/main/assets/logshed-logo.png" alt="LogShed Logo" width="160">
</p>

<h1 align="center">LogShed</h1>

<p align="center">
  <strong>Lightweight, self-hosted log aggregation for homelabs and personal servers.</strong><br>
  Real-time streaming, fast SQLite FTS5 search, and user-directed AI incident analysis in a single process.
</p>

<p align="center">
  <a href="https://github.com/logshed/logshed"><img src="https://img.shields.io/badge/repo-logshed%2Flogshed-blue?style=flat-square" alt="Main Repo"></a>
  <a href="https://github.com/logshed/logshed/pkgs/container/logshed"><img src="https://img.shields.io/badge/container-ghcr.io-blue?logo=docker&logoColor=white&style=flat-square" alt="GHCR Container"></a>
  <a href="https://github.com/logshed/logshed/releases"><img src="https://img.shields.io/badge/releases-latest-green?style=flat-square" alt="Releases"></a>
</p>

---

### What is LogShed?

Most log aggregation suites (such as Grafana Loki or Elasticsearch) demand gigabytes of memory and multiple services. LogShed provides a lightweight alternative tailored for homelabs, routers, and container hosts.

* **Single Container, Single Process:** Built on Python 3.12 (`asyncio`), FastAPI, and React. No external databases, message brokers, or background caches required.
* **Dual Ingestion:** Ingests standard RFC 3164 and RFC 5424 syslog over UDP and TCP (port 1514), while tailing local or remote Docker containers directly via the Docker Engine API.
* **Fast Full-Text Search:** Decoupled SQLite FTS5 indexing with sub-second catch-up latency.
* **Real-Time Alerting & Spike Detection:** Sliding-window thresholds, pattern matching, log storm rate detection, and 80+ notification channels via Apprise.
* **User-Directed AI Investigation:** On-demand root-cause analysis with Google Gemini, OpenAI, or local models (Ollama / vLLM), with client and server secret redaction before dispatch.

---

### Quick Start

Deploy via Docker Compose in seconds:

```yaml
services:
  logshed:
    image: ghcr.io/logshed/logshed:latest
    container_name: logshed
    restart: unless-stopped
    ports:
      - "8080:8080"        # Web Dashboard and REST API
      - "1514:1514/udp"    # Syslog UDP Ingestion
      - "1514:1514/tcp"    # Syslog TCP Ingestion
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=UTC
      - DOCKER_HOST=unix:///var/run/docker.sock
    volumes:
      - ./data:/data
      - /var/run/docker.sock:/var/run/docker.sock:ro
```

---

### Key Resources

* **Main Repository:** [logshed/logshed](https://github.com/logshed/logshed)
* **Configuration Guide:** [docs/CONFIGURATION.md](https://github.com/logshed/logshed/blob/main/docs/CONFIGURATION.md)
* **Forwarding Logs (Syslog & Docker):** [docs/SENDING_LOGS.md](https://github.com/logshed/logshed/blob/main/docs/SENDING_LOGS.md)
* **Alert Rules Guide:** [docs/ALERT_RULES.md](https://github.com/logshed/logshed/blob/main/docs/ALERT_RULES.md)
* **Changelog & Releases:** [Releases](https://github.com/logshed/logshed/releases) | [CHANGELOG.md](https://github.com/logshed/logshed/blob/main/CHANGELOG.md)
* **Issue Tracker & Feature Requests:** [Issues](https://github.com/logshed/logshed/issues)
