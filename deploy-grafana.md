
### **Guide 1: Getting Grafana Up and Running with Docker**

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

That’s it for setting up **Grafana**! Next, we can move on to **Prometheus** in the second guide. Let me know if you need any adjustments before we move on!
