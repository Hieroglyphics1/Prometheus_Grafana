Proyecto de Monitoreo con Prometheus y Grafana

Este proyecto utiliza Docker Compose para orquestar y desplegar un entorno de monitoreo básico que incluye Prometheus, Grafana, NGINX y NGINX Exporter.

Configuración del Entorno

Servicios

Prometheus

Imagen: prom/prometheus
Puertos Externos: 9090
Archivo de Configuración: prometheus.yml
Acceso: http://localhost:9090

Grafana

Imagen: grafana/grafana
Puertos Externos: 3000
Acceso: http://localhost:3000
Usuario/Contraseña por Defecto: admin/admin

NGINX

Imagen: nginx:latest
Puertos Externos: 8080
Archivos de Configuración: nginx.conf, index.html
Acceso: http://localhost:8080

NGINX Exporter

Imagen: nginx/nginx-prometheus-exporter
Puertos Externos: 9113
Acceso: http://localhost:9113/metrics
Dependencias: Dependiente de NGINX para obtener estadísticas.
Archivos de Configuración



prometheus.yml

# Configuración global
global:
  scrape_interval: 15s
  evaluation_interval: 15s

# Configuración de alertas
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            # Aquí se podría agregar configuración específica del Alertmanager si es necesario.

# Reglas de evaluación
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# Configuración de scraping
scrape_configs:
  - job_name: 'nginx-exporter'
    static_configs:
      - targets: ['nginx-exporter:9113']


nginx.conf

events {
    worker_connections 1024;
}

http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            index index.html;
        }

        location /nginx_status {
            stub_status on;
            allow all;  # Permitir acceso desde cualquier dirección IP
        }
    }
}



index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple HTML Page</title>
</head>
<body>
    <h1>Hello, World!</h1>
    <p>This is a simple HTML page served by NGINX.</p>
</body>
</html>





DOCKER COMPOSE

Código


version: '3'

services:
  prometheus:
    image: prom/prometheus
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    networks:
      - monitoring

  grafana:
    image: grafana/grafana
    container_name: grafana
    ports:
      - "3000:3000"
    networks:
      - monitoring

  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - "8080:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./index.html:/usr/share/nginx/html/index.html:ro
    networks:
      - monitoring

  nginx-exporter:
    image: nginx/nginx-prometheus-exporter
    container_name: nginx-exporter
    ports:
      - "9113:9113"
    networks:
      - monitoring
    depends_on:
      - nginx
    command:
      - '--nginx.scrape-uri=http://nginx:8080/nginx_status'

networks:
  monitoring:
    driver: bridge
