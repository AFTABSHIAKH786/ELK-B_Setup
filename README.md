# Elastic Stack (ELK-B) Docker Setup for Local Development

This project provides a complete local development environment for the Elastic Stack (Elasticsearch, Logstash, Kibana, Beats) using Docker Compose. It enables developers to quickly spin up an observability and log aggregation stack with minimal configuration.

## 📦 Stack Components

| Component         | Description                                                                                    |
| ----------------- | ---------------------------------------------------------------------------------------------- |
| **Elasticsearch** | Distributed search and analytics engine at the core of the Elastic Stack.                      |
| **Kibana**        | Visualization UI for Elasticsearch data. Also used to manage, query, and explore data.         |
| **Logstash**      | Server-side data processing pipeline; ingests, transforms, and forwards logs to Elasticsearch. |
| **Filebeat**      | Lightweight log shipper that forwards log files from your host to Logstash or Elasticsearch.   |
| **Metricbeat**    | Collects system and service metrics and sends them to Elasticsearch.                           |

---

## ✅ Prerequisites

* [Docker Desktop](https://www.docker.com/products/docker-desktop) or Docker Engine with Docker Compose
* At least 4 GB RAM (8 GB+ recommended)

---

## 📁 File Structure

```bash
elastic-stack-docker/
├── .env
├── docker-compose.yml
├── filebeat.yml
├── logstash.conf
├── metricbeat.yml
├── filebeat_ingest_data/
├── logstash_ingest_data/
```

---

## ⚙️ Getting Started

### Step 1: Clone the repository

```bash
git clone https://github.com/your-username/elastic-stack-docker.git
cd elastic-stack-docker
```

### Step 2: Set Environment Variables

Edit the `.env` file to set passwords, memory limits, and version.

```env
ELASTIC_PASSWORD=changeme
KIBANA_PASSWORD=changeme
STACK_VERSION=8.7.1
ES_PORT=9200
KIBANA_PORT=5601
```

### Step 3: Increase Virtual Memory (Linux)

```bash
sudo sysctl -w vm.max_map_count=262144
```

### Step 4: Start the Stack

```bash
docker-compose up
```

> The `setup` container will generate certificates and then exit. Other services will continue running.

---

## 🔍 Accessing the Stack

* **Elasticsearch**: [https://localhost:9200](https://localhost:9200)
* **Kibana**: [http://localhost:5601](http://localhost:5601)

Login:

```
Username: elastic
Password: changeme
```

---

## 📊 Monitoring and Ingestion

### Metricbeat

* Monitors Elasticsearch, Kibana, Logstash, Docker, and host metrics
* Data is visible in Kibana > Stack Monitoring

### Filebeat

* Reads logs from `filebeat_ingest_data/`
* Uses Docker autodiscover to read container logs

### Logstash

* Reads and processes logs from `logstash_ingest_data/`
* Writes to `logstash-*` index in Elasticsearch

---

## 📌 Useful Docker Commands

```bash
docker-compose up              # Start all services
docker-compose down           # Stop and remove containers
docker ps                     # Check running containers
docker-compose logs -f        # Tail logs
```

---

## 📚 Resources

* [Elastic Docker Documentation](https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-docker.html)
* [Beats Modules](https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-modules.html)
* [Logstash Config Docs](https://www.elastic.co/guide/en/logstash/current/index.html)

---

## 🛡️ Security Notes

* Security is enabled by default in 8.x
* TLS certificates are auto-generated using the setup container
* Consider using secrets or keystores for production environments

---

## 🧪 Test the Setup

```bash
docker cp elasticstack_docker-es01-1:/usr/share/elasticsearch/config/certs/ca/ca.crt /tmp/
curl --cacert /tmp/ca.crt -u elastic:changeme https://localhost:9200
```

---

## 🎉 Success!

You now have a full local Elastic Stack with:

* Secure Elasticsearch node
* Kibana dashboard
* Filebeat and Logstash log ingestion
* Metricbeat-based monitoring

