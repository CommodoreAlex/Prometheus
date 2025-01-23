# Prometheus Stack with Grafana and Node Exporter

This repository provides a comprehensive guide and setup for deploying a **Prometheus Stack** that includes **Prometheus**, **Grafana**, and **Node Exporter**. This stack is ideal for monitoring system metrics such as CPU, memory, disk usage, and network stats. With Grafana, you can easily visualize these metrics through customizable dashboards.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Setup Guide](#setup-guide)
  - [1. Deploying Grafana](#1-deploying-grafana)
  - [2. Deploying Prometheus and Configuring it to Scrape Node Exporter](#2-deploying-prometheus-and-configuring-it-to-scrape-node-exporter)
  - [3. Deploying Node Exporter](#3-deploying-node-exporter)
- [Verifying the Setup](#verifying-the-setup)
  - [1. Prometheus UI](#1-prometheus-ui)
  - [2. Grafana UI](#2-grafana-ui)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Overview

This repository will guide you through setting up the Prometheus monitoring stack using Docker Compose. You will be deploying:

- **Prometheus**: A powerful open-source time series database that scrapes and stores metrics.
- **Grafana**: A visualization tool that connects to Prometheus to display beautiful and informative dashboards.
- **Node Exporter**: A lightweight exporter that collects system-level metrics (CPU, memory, disk, etc.) from your server.

This stack will provide you with real-time insights into your system's health and performance.

## Prerequisites

Before you begin, ensure that you have the following:

- **Docker**: Make sure Docker is installed and running on your system.
- **Docker Compose**: Install Docker Compose to manage multi-container applications.

If you need instructions on installing Docker and Docker Compose, please refer to the official documentation:
- [Install Docker](https://docs.docker.com/get-docker/)
- [Install Docker Compose](https://docs.docker.com/compose/install/)

## Setup Guide

### 1. Deploying Grafana

Start by deploying Grafana, which will be used to visualize the metrics collected by Prometheus.

- **Create and Configure Grafana with Docker Compose**:
  Follow the detailed instructions in **Part 1: Deploying Grafana** of this guide.

### 2. Deploying Prometheus and Configuring it to Scrape Node Exporter

In this step, you'll deploy Prometheus and configure it to scrape the Node Exporter metrics.

- **Create and Configure Prometheus with Docker Compose**:
  Follow the detailed instructions in **Part 2: Deploying Prometheus and Connecting it to Grafana** of this guide.

### 3. Deploying Node Exporter

Node Exporter collects system-level metrics. In this step, you'll deploy the Node Exporter container.

- **Create and Configure Node Exporter with Docker Compose**:
  Follow the detailed instructions in **Part 3: Deploying Node Exporter** of this guide.

## Verifying the Setup

Once everything is deployed, you can verify the setup by accessing the web interfaces for Prometheus and Grafana.

### 1. Prometheus UI

Go to [http://localhost:9090](http://localhost:9090) in your web browser to access the Prometheus web interface.

Here you can:
- Check the status of targets being scraped.
- Query and visualize raw metrics.

### 2. Grafana UI

Go to [http://localhost:3000](http://localhost:3000) in your web browser to access the Grafana web interface.

Here you can:
- Configure dashboards to visualize the metrics collected by Prometheus.
- Create custom dashboards or use pre-configured templates for system monitoring.

## Troubleshooting

If you encounter issues with the setup, here are some things to check:
- **Prometheus not scraping metrics**: Ensure the `prometheus.yml` configuration is correct, especially the `static_configs` and the target URL for Node Exporter.
- **Grafana cannot connect to Prometheus**: Double-check that Prometheus is up and running, and that the Grafana data source URL is set correctly (i.e., `http://prometheus:9090`).
