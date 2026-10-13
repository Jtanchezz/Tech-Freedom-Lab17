# Observación real de Tor
Carnet 20230005 · Red de casa confirmada por el estudiante.

| Campo | Antes | Después |
|---|---|---|
| Fecha del reloj local | 12 octubre 2026, 21:33 | 12 octubre 2026, 21:34 |
| URL | https://polymarket.com/ | https://polymarket.com/ |
| Guard | Serbia | Serbia |
| Middle | Estados Unidos | Suecia |
| Exit | Suecia | Países Bajos |
| Connected | Sí, circuito y sitio visibles | Sí, circuito y sitio visibles |
| Archivo | evidence/tor-antes.png | evidence/tor-despues.png |

El país del guard se mantuvo; no se afirma identidad exacta del relay porque las IPs están tapadas. Middle y exit cambiaron de país. La comparación con Chrome está en evidence/navegador-normal.png. Las horas provienen del reloj visible, no de una medición de latencia.

El exit ve destino y metadatos temporales; con HTTPS válido no ve la contraseña, ruta URL ni contenido del formulario. El sitio sí recibe el payload de aplicación. Un login identifica al usuario ante el sitio aunque llegue por Tor.

Pendiente de confirmar para trazabilidad de la consigna: que Polymarket fuera el sitio autorizado por el instructor, descarga oficial y configuración por defecto. La red de casa sí fue confirmada. Las capturas son aportadas por el estudiante y no fueron reconstruidas ni reemplazadas.
