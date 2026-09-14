# LAB-001 — Wake-on-LAN Motorola → Lenovo Linux

## Objetivo

Intentar encender de forma remota una notebook Lenovo con Linux desde un Motorola con Termux mediante Wake-on-LAN.

## Dispositivos involucrados

- Motorola Android con Termux
- Lenovo G50-30 con Linux
- Router de la red doméstica

## Estado actual

🟡 Parcialmente completado / bloqueado por falta de conexión Ethernet.

## Hallazgos

La interfaz Wi-Fi de la Lenovo es:

- Broadcom BCM43142
- Driver: `wl`

La salida de `iw phy` no mostró soporte WoWLAN, por lo que no se pudo confirmar capacidad de encendido remoto mediante Wi-Fi.

La interfaz Ethernet es:

- Realtek RTL8111/8168/8411
- Interfaz Linux: `enp3s0`

Se verificó soporte Wake-on-LAN mediante:

`Supports Wake-on: pumbg`

Inicialmente:

`Wake-on: d`

Luego se habilitó Magic Packet mediante:

`sudo ethtool -s enp3s0 wol g`

Y se validó:

`Wake-on: g`

## Bloqueo actual

No hay cable Ethernet disponible.

La interfaz mostró:

`Link detected: no`

Por lo tanto, todavía no fue posible realizar la prueba final de encendido remoto.

## Pasos pendientes

1. Conectar la Lenovo al router mediante cable Ethernet.
2. Confirmar enlace activo.
3. Obtener la dirección MAC de `enp3s0`.
4. Preparar el Motorola para enviar un Magic Packet.
5. Apagar completamente la Lenovo.
6. Enviar el Magic Packet desde Termux.
7. Confirmar si la Lenovo enciende correctamente.

## Evidencia a conservar

- Captura mostrando `Supports Wake-on: pumbg`
- Captura mostrando el cambio de `Wake-on: d` a `Wake-on: g`
- Evidencia futura del envío del Magic Packet
- Evidencia futura del encendido exitoso

## Resultado esperado

Encender la Lenovo Linux desde el Motorola mediante un comando de Termux, por ejemplo:

`notebook-on`
