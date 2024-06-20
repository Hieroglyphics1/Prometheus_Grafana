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


Prometheus 

![image](https://github.com/Hieroglyphics1/Prometheus_Grafana/assets/107959161/eebf58d2-d279-4d75-af70-9b3ac2d892cd)


Grafana 

![image](https://github.com/Hieroglyphics1/Prometheus_Grafana/assets/107959161/9e81487e-39a4-4e6f-8898-aa5febfc90d4)


