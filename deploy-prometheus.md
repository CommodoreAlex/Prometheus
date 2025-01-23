### 2) **Deploying and Connecting Prometheus to Grafana with Docker**

#### **Introduction: What is Prometheus?**

**Prometheus** is an open-source monitoring and alerting toolkit designed for reliability and scalability. It collects and stores metrics as time-series data, with powerful querying capabilities. It is often used in combination with Grafana to provide detailed, real-time insights into your infrastructure by visualizing Prometheus’ collected data.

In this file, we’ll set up **Prometheus** using Docker and connect it to **Grafana** for visualization.

---

#### **Step 1: Create Prometheus Configuration**

Before deploying Prometheus, we need to create a configuration file. This will define how Prometheus scrapes metrics from targets.

Create a directory for Prometheus configuration:
```bash
mkdir prometheus-setup
cd prometheus-setup
```

Create the `prometheus.yml` configuration file:
```bash
vi prometheus.yml
```

Add the following configuration to the file:
```yaml
global:
  scrape_interval: 15s  # Define the interval to scrape metrics

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']  # Scrape Prometheus metrics
```

This basic config file sets up Prometheus to scrape its own metrics on `localhost:9090`.

---

#### **Step 2: Create the `docker-compose.yml` File for Prometheus**

Now, let’s set up Prometheus using Docker Compose.

In the same `prometheus-setup` directory, create a `docker-compose.yml` file:
```bash
vi docker-compose.yml
```

Add the following contents to the file:
```yaml
version: '3'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml  # Mount the config file
    ports:
      - "9090:9090"  # Expose Prometheus web UI
    networks:
      - monitoring
    restart: unless-stopped

networks:
  monitoring:
    driver: bridge
```

This configuration:

- Uses the **official Prometheus Docker image**.
- Mounts the `prometheus.yml` configuration file to the container.
- Exposes Prometheus' web UI on `http://localhost:9090`.

---
#### **Step 3: Start Prometheus with Docker Compose**

Start Prometheus with the following command:
```bash
docker-compose up -d
```

This will:

- Download the **Prometheus image** if it’s not already downloaded.
- Start the **Prometheus container** in the background (`-d`).
- Expose the **Prometheus web UI** on port `9090`.

---

#### **Step 4: Access Prometheus**

Once the container is running, open your web browser and navigate to:
```bash
http://localhost:9090
```

You’ll see the **Prometheus web UI**, where you can start querying and exploring metrics.

---

#### **Step 5: Connect Prometheus to Grafana**

Now that both **Prometheus** and **Grafana** are up and running, let’s connect **Prometheus** as a data source in **Grafana**.

1. Open the **Grafana dashboard** by navigating to `http://localhost:3000` in your browser.
2. Log in with your credentials (default: **admin/admin**, unless you changed it).
3. In the left sidebar, click on the **gear icon** (⚙️) to open **Configuration**, and then select **Data Sources**.
4. Click on **Add data source**.
5. Choose **Prometheus** from the list of available data sources.
6. In the **HTTP URL** field, enter: `http://prometheus:9090`
    (Since both Grafana and Prometheus are in the same Docker network, you can use the service name `prometheus` as the hostname.)
7. Click **Save & Test** to confirm that Grafana can connect to Prometheus.

---

#### **Step 6: Visualize Prometheus Metrics in Grafana**

Now that Prometheus is connected to Grafana:

1. Click on the **+** sign in the left sidebar and select **Dashboard**.
2. Click **Add new panel** to create a new graph.
3. In the **Query** section, select **Prometheus** as the data source.
4. Enter a Prometheus query (e.g., `up` to check the status of Prometheus) and click **Apply**.

You should now see a live graph displaying metrics from Prometheus.

---

Prometheus is successfully set up and connected to Grafana, now you can start collecting and visualizing metrics.

In the next file in this repository we will set up 'Node Exporter' to collect system-level metrics, which will be scraped by Prometheus and visualized in Grafana. 
