# Sistema Integral de Monitoreo de Servidores con Docker Compose

Este proyecto despliega un stack completo para monitoreo y gestión de servidores y contenedores basado en Docker Compose. Incluye herramientas clave como **Node Exporter**, **cAdvisor**, **Prometheus**, **Grafana** y **Portainer**, que juntas permiten obtener métricas detalladas del sistema, contenedores y gestionar la infraestructura de manera visual y centralizada.

---

## Componentes

### 1. Node Exporter
- Imagen: `prom/node-exporter:latest`
- Función: Exporta métricas del host (CPU, memoria, disco, red, procesos, etc.) para Prometheus.
- Configuración especial:
  - Monta el filesystem del host en modo lectura para acceder a métricas reales.
  - Comparte el namespace de procesos con el host para información precisa.
- Puerto expuesto: `9100`

### 2. cAdvisor
- Imagen: `gcr.io/cadvisor/cadvisor:latest`
- Función: Recolecta métricas de contenedores Docker en tiempo real (CPU, memoria, I/O, red).
- Monta directorios clave del host para obtener métricas precisas.
- Puerto expuesto: `8080`

### 3. Prometheus
- Imagen: `prom/prometheus:latest`
- Función: Motor de recolección y almacenamiento de métricas.
- Configuración:
  - Usa el archivo `prometheus.yml` para definir targets (Node Exporter, cAdvisor).
  - Opcionalmente puede usar `web.yml` para configuración de autenticación.
- Puerto expuesto: `9090`

### 4. Grafana
- Imagen: `grafana/grafana:latest`
- Función: Visualización avanzada de métricas con dashboards preconfigurados.
- Volumen persistente para datos y configuraciones.
- Puerto expuesto: `3000`
- Dashboards recomendados:
  - ID 1860 para Node Exporter
  - ID 19908 para cAdvisor

### 5. Portainer
- Imagen: `portainer/portainer-ce:latest`
- Función: Gestión visual de contenedores Docker y clusters.
- Monta el socket Docker para controlar el host.
- Volumen persistente para datos.
- Puerto expuesto: `9000`

---

## Cómo usar este sistema

1. **Clona este repositorio** en tu servidor o máquina con Docker instalado.

2. **Configura los archivos** `prometheus.yml` y `web.yml` según tus necesidades (el archivo `prometheus.yml` debe incluir los endpoints de Node Exporter y cAdvisor).

3. **Levanta el stack** con Docker Compose:

   ```bash
   docker-compose up -d
   ```

4. **Accede a las interfaces web:**

   - **Portainer:** `http://:9000` - para gestionar contenedores y servicios.
   - **Prometheus:** `http://:9090` - para consultas y métricas crudas.
   - **Grafana:** `http://:3000` - para dashboards visuales y alertas.
   - **cAdvisor:** `http://:8080` - para métricas detalladas de contenedores.
   - **Node Exporter:** expone métricas en `http://:9100/metrics` (usado por Prometheus).

5. **Importa dashboards recomendados en Grafana:**

   - Node Exporter Full Dashboard (ID: 1860)
   - cAdvisor Docker Insights (ID: 19908)

---

## Beneficios del sistema

- **Monitoreo integral:** Obtienes métricas tanto del host físico como de los contenedores.
- **Visualización intuitiva:** Grafana permite crear alertas y analizar tendencias fácilmente.
- **Gestión simplificada:** Portainer facilita la administración de contenedores sin usar línea de comandos.
- **Escalable y flexible:** Puedes agregar más servicios o nodos modificando la configuración de Prometheus.

---

## Notas importantes

- Los volúmenes `portainer-data` y `grafana-data` aseguran persistencia de datos y configuraciones.
- El socket Docker (`/var/run/docker.sock`) se monta en Portainer para permitir la gestión del host.
- Asegúrate de que los puertos usados no estén en conflicto con otros servicios en tu servidor.
- Para seguridad en producción, considera agregar autenticación a Prometheus y Grafana.

---

## Referencias

- [Node Exporter GitHub](https://github.com/prometheus/node_exporter)
- [cAdvisor GitHub](https://github.com/google/cadvisor)
- [Prometheus Documentation](https://prometheus.io/docs/)
- [Grafana Dashboards](https://grafana.com/grafana/dashboards)
- [Portainer Documentation](https://docs.portainer.io/)

---

¡Con este stack tendrás un sistema robusto y completo para monitorear y gestionar tus servidores y contenedores Docker fácilmente!
```

---

