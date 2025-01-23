### Part 3: Deploying Node Exporter with Docker and Connecting to Prometheus and Grafana

Node Exporter is a lightweight exporter for hardware and OS metrics exposed by *NIX kernels, such as CPU usage, memory, disk, network stats, and more. We’ll deploy Node Exporter in a Docker container and configure Prometheus to scrape metrics from it. Finally, we’ll connect it to Grafana for visualization.

#### Step 1: Deploy Node Exporter with Docker

**Create a Docker Compose file for Node Exporter**:  
In the `prometheus-setup` folder (or a new folder if you'd like), create a `docker-compose.yml` file specifically for Node Exporter.

Here's a basic example of the `docker-compose.yml` for Node Exporter:
```yaml
version: "3"
services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    networks:
      - monitoring
    restart: always

  grafana:
    image: grafana/grafana:8.3.3
    container_name: grafana
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    networks:
      - monitoring
    restart: always

  node_exporter:
    image: prom/node-exporter:latest
    container_name: node_exporter
    ports:
      - "9100:9100"
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    networks:
      - monitoring
    restart: always

networks:
  monitoring:
    driver: bridge

```

**Explanation**:
- `prom/node-exporter:latest`: This is the official Docker image for Node Exporter.
- Port `9100` is mapped to allow Prometheus to scrape metrics from Node Exporter.
- The volumes provide the necessary permissions for Node Exporter to access system metrics.

**Start the Node Exporter container**: Navigate to the directory where the `docker-compose.yml` file is located and run the following command to start the container:
```bash
docker-compose up -d
```
 
This will download the Node Exporter image (if it hasn’t been downloaded already), create the container, and start it.

#### Step 2: Configure Prometheus to Scrape Metrics from Node Exporter

**Update Prometheus Configuration**:  
We need to tell Prometheus to scrape the Node Exporter metrics. Open the `prometheus.yml` configuration file for Prometheus (in the Prometheus setup directory) and add the following scrape configuration under `scrape_configs` where the existing configuration is:
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node_exporter:9100']

```

**Explanation**:

- `job_name: 'node-exporter'`: This defines the job name for the scrape configuration.
- `targets: ['node_exporter:9100']`: This tells Prometheus to scrape the Node Exporter metrics from the container at port 9100.

**Restart Prometheus**: After updating the Prometheus configuration, you need to restart Prometheus to apply the changes. Run the following command:
```bash
docker-compose restart prometheus
```

#### Step 3: Verify the Prometheus and Node Exporter Connection

**Access Prometheus Web UI**: Open your browser and go to [http://localhost:9090](http://localhost:9090).

**Check Targets**: In the Prometheus UI, go to `Status` → `Targets` to verify that Prometheus is scraping the Node Exporter metrics. You should see a target named `node-exporter:9100` with the `last scrape` column showing recent activity.

#### Step 4: Create a Grafana Dashboard for Node Exporter Metrics

1. **Access Grafana**: Open your browser and go to http://localhost:3000 to access the Grafana Web UI.
    - Default username: `admin`
    - Default password: `admin`
2. **Add Prometheus as a Data Source**:
    - Go to `Configuration` → `Data Sources`.
    - Click `Add data source` and select `Prometheus`.
    - Set the URL to `http://prometheus:9090` (this is assuming your Prometheus container is named `prometheus`).
    - Click `Save & Test` to verify the connection.
3. **Create a Dashboard**:
    - Go to `Create` → `Dashboard`.
    - Click on `Add Panel` and in the query field, select `node_cpu_seconds_total` or any other metric you'd like to visualize.
    - Customize the panel with your preferred visualization options (e.g., graph, gauge, etc.).
    - Save the dashboard.

#### Step 5: Verify the Grafana Dashboard

1. **View Metrics**: After saving your dashboard, you should be able to see the Node Exporter metrics (e.g., CPU usage, memory, disk stats) visualized on your Grafana dashboard.

----

That is the complete deployment of a basic Grafana, Prometheus, and Node Exporter stack to collect and visualize system metrics (basic monitoring).

In the future there will be documentation for tracking **advanced metrics**, custom dashboards, and alerting for a more comprehensive monitoring solution.
