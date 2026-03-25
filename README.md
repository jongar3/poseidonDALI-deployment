# IoT Poseidon: Orchestration & Deployment

Este repositorio contiene la infraestructura necesaria para desplegar el ecosistema **IoT Dali hecho sobre poseidon**. El sistema utiliza una arquitectura de microservicios aislados, dividida en un entorno de control de borde (Edge) y una capa de monitorización y gestión en la nube.

Este proyecto nace como una solución de infraestructura moderna para el ecosistema de hardware-software de **Enika**

## Estructura del Proyecto

* **`/local`**: Servicios destinados a la ejecución en el sitio (Edge).
    * `docker-compose.yml`: Orquestador para los servicios locales.
    `config.json`: Archivo de configuración crítica (IP del Gateway, dominios, etc.) que se inyecta en el daemon.
    *`data/`: Donde se guardara la base de datos SQLite SE DEBE INCLUIR EL MAPEO CSV mediante el archvio registers.csp
* **`/cloud`**: Infraestructura centralizada de monitorización y backend.
    * `docker-compose.yml`: Orquestador para los servicios locales.
    * `grafana`: Visualización avanzada con dashboards pre-configurados vía JSON.
    * `caddy`: Proxy inverso para gestión de certificados SSL y acceso seguro.
---
## Prerrequisitos
    * Docker-DockerDesktop Instalado
    * Docker Compose
    * Acceso al gateway
    * Git """Obviamente"""

## Configuración Inicial

Para garantizar el correcto funcionamiento, sigue estos pasos antes de iniciar los contenedores:

### 1. Exportación registers.csv
Se debe preconfigurar todos los componenetes con el software poseidon tal y como se explica en el manual. Después se debe exportar el arhivo csv. y llamarlo registers.csv (explicado como hacerlo en el manual). Este archivo incluye un mapeo necesario de los registros del gateway.


### 2. Preconfiguración config.json
Se debe editar el archivo `config.json` ubicado en /local. Se debe cambiar la ip del Gateway asi como los dominios de la nube, el puerto rara vez se cambia. Se incluye un ejemplo del json "logicamente si usamos https usaremaos el pueto 443":

```json
{
    "API_DOMAIN": "api.jgc-server.duckdns.org",
    "GRAFANA_DOMAIN": "grafana.jgc-server.duckdns.org",
    "API_PORT": 443,
    "USE_HTTPS": true,
    "GATEWAY_IP": "192.168.1.224" 
}
```
### 3. Preconfiguración Caddyfile
Se debe configurar el Reverse-Proxy para ello hay que cambiar el `Caddyfile` situado en cloud / simplemente nos aseguramos que coincida con el dominio

## Puesta en marcha

Primero clonamos el repositorio:
- ***`git clone https://github.com/jongar3/poseidonDALI-deployment`***

Depende de lo que queramos levantar, vamos a la carpeta cloud o local (normalmente iremos a local) y ejecutamos el comando:
- ***`docker compose up -d --build`***
Y ya estaria. Incluyo algunos comando utiles:

| Acción | Comando |
| :--- | :--- |
| **Detener todos los servicios** | `docker compose down` |
| **Ver logs en tiempo real** | `docker compose logs -f` |
| **Reiniciar Servicios** | `docker compose restart [nombre_servicio]` |
| **Apagar Servicio** | `docker compose down` |

## Licencia

Este proyecto es propiedad exclusiva de **Ekilor**.

- Uso No Comercial: Permitido para formación, desarrollo y pruebas internas de integración con software de Enika.

- Uso Comercial: Requiere autorización explícita del titular para despliegues en clientes finales o servicios de pago.

---

Hecho con ❤️ por Jongar3 en ekilor
