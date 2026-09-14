# 🏗️ Home Cyber Lab Architecture

## Descripción

El Home Cyber Lab es un entorno doméstico y controlado diseñado para practicar ciberseguridad defensiva utilizando dispositivos propios dentro de una red autorizada.

El laboratorio permite generar tráfico real, observar eventos, analizar logs, realizar experimentos de red y desarrollar progresivamente capacidades de monitoreo y detección.

## Arquitectura actual

Internet
   |
Router doméstico
   |
Red local (LAN)
   |
   +-- Lenovo G50-30
   |     - Linux
   |     - Python
   |     - Git
   |     - Splunk
   |     - Análisis de logs
   |     - Servicios de laboratorio
   |
   +-- Motorola Android
         - Termux
         - curl
         - Cliente de red
         - Generación de eventos
         - Automatización

## Roles de los dispositivos

### Lenovo G50-30 — Host de análisis

La Lenovo funciona como sistema principal del laboratorio.

Actualmente se utiliza para:

- ejecutar servicios controlados;
- recibir conexiones desde otros dispositivos;
- generar y analizar logs;
- realizar tareas de administración Linux;
- ejecutar Splunk;
- desarrollar scripts;
- administrar el repositorio Git.

### Motorola Android — Cliente y dispositivo de pruebas

El Motorola funciona como un segundo host real dentro del laboratorio.

Mediante Termux puede utilizarse para:

- iniciar conexiones hacia la Lenovo;
- generar tráfico controlado;
- ejecutar herramientas Linux;
- automatizar tareas;
- simular actividad de otro dispositivo de la red.

### Router doméstico — Infraestructura de red

El router proporciona conectividad entre los dispositivos del laboratorio y permite que Lenovo y Motorola se comuniquen dentro de la LAN.

## Flujo básico

Motorola → Router/LAN → Lenovo → Evento/Log → Análisis

## Experimentos actuales

### LAB-001 — Wake-on-LAN

Investigación del soporte Wake-on-LAN de la Lenovo y preparación para encendido remoto desde el Motorola.

Estado: parcial / pendiente de conexión Ethernet.

### LAB-002 — HTTP Communication & Log Analysis

Comunicación HTTP entre Motorola y Lenovo, análisis del evento generado, identificación de una exposición innecesaria y posterior corrección.

Estado: completado.

## Evolución prevista

El laboratorio podrá ampliarse progresivamente con:

- descubrimiento y baseline de red;
- scripts de monitoreo;
- generación de eventos controlados;
- centralización de logs;
- análisis con Splunk;
- reglas de detección;
- automatización defensiva;
- simulaciones de incidentes.

## Alcance y seguridad

Todos los experimentos se realizan exclusivamente sobre dispositivos y redes propios o expresamente autorizados.

El repositorio no debe contener contraseñas, tokens, claves privadas ni información sensible innecesaria.
