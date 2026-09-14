# LAB-002 — Motorola → Lenovo HTTP Communication & Log Analysis

## Objetivo

Establecer una comunicación HTTP controlada entre un Motorola Android con Termux y una Lenovo Linux dentro de la red doméstica, observar los eventos generados y analizar una configuración con exposición innecesaria.

## Entorno

- Cliente: Motorola Android con Termux
- Servidor: Lenovo G50-30 con Linux
- Lenovo: `192.168.100.6`
- Motorola observado por el servidor: `192.168.100.21`
- Protocolo: HTTP
- Transporte: TCP
- Puerto: `8000`
- Cliente: `curl`
- Servidor: módulo HTTP de Python 3

## Primera prueba

En la Lenovo se inició:

`python3 -m http.server 8000`

Desde Termux se realizó:

`curl http://192.168.100.6:8000`

La comunicación fue exitosa.

El servidor registró:

`192.168.100.21 - - [14/Sep/2026 01:58:59] "GET / HTTP/1.1" 200 -`

### Análisis del evento

- `192.168.100.21`: IP origen observada por el servidor.
- `14/Sep/2026 01:58:59`: fecha y hora del evento.
- `GET`: método HTTP utilizado.
- `/`: recurso solicitado.
- `HTTP/1.1`: versión del protocolo.
- `200`: petición procesada correctamente.

## Hallazgo

El servidor había sido iniciado desde el directorio HOME del usuario.

Como consecuencia, la respuesta HTTP mostró un listado de archivos y directorios que no era necesario exponer a otros dispositivos de la LAN.

El servidor fue detenido inmediatamente.

No se conservarán ni publicarán los nombres de los archivos personales observados durante esta prueba.

## Corrección

Se creó un directorio dedicado exclusivamente al laboratorio:

`~/home-cyber-lab/http-lab`

Dentro se creó únicamente un archivo controlado:

`index.html`

Contenido:

`Home Cyber Lab - LAB-002 - HTTP Test`

El servidor HTTP se inició nuevamente desde este directorio y se repitió la petición desde el Motorola.

## Validación

El servidor registró:

`192.168.100.21 - - [14/Sep/2026 02:13:22] "GET / HTTP/1.1" 200 -`

La comunicación continuó funcionando correctamente, pero el contenido servido quedó limitado al directorio específico del laboratorio.

## Flujo observado

Motorola → Wi-Fi/router → Lenovo `192.168.100.6:8000` → HTTP GET `/` → HTTP `200`

## Lecciones aprendidas

- Modelo cliente-servidor.
- Direcciones IP de origen y destino.
- Uso de puertos TCP.
- Funcionamiento básico de HTTP.
- Generación e interpretación de logs.
- Diferencia entre conectividad y exposición segura.
- Importancia de limitar qué directorios publica un servicio.
- Reducción de superficie de exposición.
- Validación posterior a una corrección.

## Evidencia

- `lab-002-http-request-log.png`
- `lab-002-http-isolated-server.png`

## Resultado

✅ Completado.

Se consiguió establecer y observar comunicación HTTP real entre dos dispositivos del Home Cyber Lab, identificar una exposición innecesaria, aplicar una corrección y validar posteriormente que el servicio continuaba funcionando.
