
### **Guide 1: Getting Grafana Up and Running with Docker**

Grafana is an open-source platform used for monitoring and observability. It allows you to visualize time-series data from various sources in customizable dashboards. Typically, Grafana is used with monitoring tools like Prometheus, InfluxDB, and others to provide real-time visualization of your infrastructure metrics, application logs, and other monitoring data.

In this guide, we’ll walk through how to deploy Grafana using Docker, which is a quick and easy way to get Grafana up and running in isolated containers.

#### **Step 1: Create a project directory**

First, create a directory where you'll store your Docker configuration.
```bash
mkdir grafana-setup cd grafana-setup
```

#### **Step 2: Create a `docker-compose.yml` file**

Inside the `grafana-setup` directory, create a `docker-compose.yml` file for Grafana:
```bash
vi docker-compose.yml
```

Add the following configuration to the file:
```yaml
version: '3'

services:
  grafana:
    image: grafana/grafana:8.3.3
    container_name: grafana
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin  # Default admin password
    ports:
      - "3000:3000"  # Expose Grafana UI on port 3000 default
    networks:
      - monitoring

networks:
  monitoring:
    driver: bridge
```

#### **Step 3: Start Grafana with Docker Compose**

Once you’ve saved the file, bring up the container:
```bash
docker-compose up -d
```

This command will:

- Download the **Grafana** Docker image (if not already downloaded).
- Start Grafana in the background.
- Expose the Grafana web UI on `http://localhost:3000`.

#### **Step 4: Access Grafana**

Open your web browser and navigate to `http://localhost:3000`. You’ll be greeted with the Grafana login page.

- **Username**: `admin`
- **Password**: `admin` (or the password you set in the `docker-compose.yml` file).

You’ll be prompted to change the password on the first login.

#### **Step 5: Confirm Grafana is Running**

Once logged in, you should see the **Grafana dashboard**. Grafana is now up and running, and you’re ready to add data sources and create dashboards.

---

Now that Grafana is up and running, the next step is to set up Prometheus, which will act as the data source for Grafana. Prometheus collects and stores metrics from your systems, and Grafana will visualize those metrics in real-time.

See the 'deploy-prometheus file', to see the deployment and configuration of Prometheus and how to connect it to Grafana.
