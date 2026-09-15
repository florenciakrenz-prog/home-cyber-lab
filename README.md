# 🛡️ Home Cyber Lab

Laboratorio doméstico de ciberseguridad defensiva construido para desarrollar experiencia práctica con **Linux, redes, Android/Termux, análisis de eventos, automatización y monitoreo**.

El proyecto utiliza dispositivos reales de mi entorno doméstico para crear experimentos controlados, observar su comportamiento, generar evidencia y documentar tanto resultados exitosos como limitaciones técnicas.

> **Metodología:** observar → formular hipótesis → validar → correlacionar evidencia → documentar.

---

## 🧪 Experimentos

| ID | Experimento | Estado |
|---|---|---|
| [LAB-001](experiments/wake-on-lan/) | Wake-on-LAN — Motorola → Lenovo Linux | 🟡 Parcial / bloqueado |
| [LAB-002](experiments/http-communication/) | HTTP Communication & Log Analysis | ✅ Completado |
| LAB-003 | Network Baseline & Device Discovery | 🔵 En progreso |

### LAB-001 — Wake-on-LAN

Investigación de la posibilidad de encender remotamente la Lenovo mediante un **Magic Packet** enviado desde el Motorola.

Durante el análisis se identificó que el adaptador Wi-Fi no ofrecía soporte para WoWLAN, mientras que la interfaz Ethernet sí soportaba Wake-on-LAN.

La configuración pudo modificarse de:

```text
Wake-on: d
```

a:

```text
Wake-on: g
```

El experimento permanece parcial porque todavía falta una conexión Ethernet física para realizar la validación final.

**Lección:** identificar y demostrar una limitación técnica también es un resultado válido.

→ [Ver LAB-001](experiments/wake-on-lan/)

---

### LAB-002 — HTTP Communication & Log Analysis

Creación de una comunicación HTTP real entre dos dispositivos del laboratorio:

```text
Motorola / Termux
        ↓
      HTTP
        ↓
Lenovo / Python HTTP Server
        ↓
      Log
```

Se levantó un servidor HTTP temporal con Python y se generó una petición desde Termux utilizando `curl`.

El servidor registró correctamente:

```text
GET / HTTP/1.1 → 200
```

Durante la prueba se detectó que el servidor había sido iniciado desde un directorio demasiado amplio, exponiendo archivos innecesariamente.

El servicio fue aislado en un directorio específico y la comunicación fue ejecutada nuevamente para validar la corrección.

```text
Crear → observar → detectar → corregir → volver a validar
```

→ [Ver LAB-002](experiments/http-communication/)

---

## 📚 Documentación técnica

### Linux & Network Command Reference

Referencia construida a partir de **comandos utilizados realmente durante los experimentos y tareas de troubleshooting del laboratorio**.

Incluye:

- interfaces y rutas de red;
- conectividad y descubrimiento;
- Nmap;
- Wake-on-LAN y `ethtool`;
- HTTP y `curl`;
- recursos del sistema;
- `journalctl` y análisis de logs;
- procesamiento de texto;
- Git y SSH;
- Splunk;
- manejo de evidencia.

→ **[Abrir Linux & Network Command Reference](docs/linux-network-command-reference.md)**

### Arquitectura del laboratorio

Descripción de los dispositivos, roles y flujo general del Home Cyber Lab.

→ [Ver arquitectura](docs/lab-architecture.md)

---

## 🏗️ Entorno

### Lenovo G50-30 — Linux

Funciona como host principal del laboratorio y progresivamente como:

- servidor;
- sistema monitoreado;
- plataforma de análisis;
- entorno de scripting;
- instancia local de Splunk.

### Motorola — Android / Termux

Funciona como:

- cliente de red;
- origen de eventos;
- terminal Linux móvil;
- dispositivo para pruebas controladas.

### Red doméstica

Permite estudiar comunicaciones reales entre dispositivos dentro de un entorno propio y autorizado.

---

## 🧰 Tecnologías y herramientas

`Linux` · `Bash` · `Python` · `Termux` · `Git` · `GitHub` · `SSH` · `Nmap` · `curl` · `ethtool` · `systemd/journalctl` · `Splunk`

---

## 🎯 Objetivos

- Comprender comunicaciones entre hosts.
- Generar y analizar eventos reales.
- Practicar análisis de logs.
- Construir baselines de actividad normal.
- Detectar y validar desviaciones.
- Automatizar tareas defensivas.
- Integrar progresivamente eventos con Splunk.
- Documentar investigaciones de forma reproducible.
- Desarrollar habilidades aplicables a un entorno SOC.

---

## 📁 Estructura

```text
home-cyber-lab/
│
├── docs/
│   ├── lab-architecture.md
│   └── linux-network-command-reference.md
│
├── experiments/
│   ├── wake-on-lan/
│   └── http-communication/
│
├── scripts/
│
├── screenshots/
│
└── .github/
```

### `docs/`

Arquitectura, referencias y documentación general.

### `experiments/`

Investigaciones individuales con objetivo, metodología, evidencia, resultado y lecciones aprendidas.

### `scripts/`

Scripts y automatizaciones desarrollados durante el crecimiento del laboratorio.

### `screenshots/`

Evidencia técnica seleccionada. Las capturas son revisadas antes de publicarse para evitar exponer información sensible.

---

## 🔐 Seguridad y privacidad

Todos los experimentos se realizan exclusivamente sobre **dispositivos, sistemas y redes propios o expresamente autorizados**.

El repositorio no está diseñado para almacenar:

- contraseñas;
- tokens;
- claves privadas;
- secretos;
- información personal innecesaria.

La evidencia técnica se sanitiza antes de publicarse.

---

## 🔎 Enfoque de investigación

No busco únicamente que un comando “funcione”.

Cada experimento intenta responder:

```text
¿Qué estoy observando?
        ↓
¿Qué hipótesis puedo formular?
        ↓
¿Cómo puedo comprobarla?
        ↓
¿Qué otras evidencias la respaldan?
        ↓
¿Qué puedo concluir realmente?
        ↓
¿Cómo lo documento?
```

El objetivo es desarrollar progresivamente una forma de trabajo cercana al **análisis defensivo y SOC**, donde distinguir entre evidencia, hipótesis y conclusión es tan importante como utilizar las herramientas.

---

## 🚧 Estado

**En desarrollo activo.**

Próximo experimento:

**LAB-003 — Network Baseline & Device Discovery**
