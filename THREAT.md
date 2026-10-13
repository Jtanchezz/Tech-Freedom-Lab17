# THREAT · Pactrail
Carnet: 20230005 · 12 de octubre de 2026 · Análisis y observación real documentada.

## Flujo y alcance
Elegí el flujo de marcar requisitos y liberar un pago de escrow. En el MVP descrito, React consulta la API Express y los contratos Solidity viven en Hardhat local; USAT es de prueba. No afirmé que exista un servicio HTTPS público ni que sus transacciones ya salgan por Tor. Comparé tres escenarios hipotéticos de transporte para una futura interfaz remota. Si un wallet o cliente RPC funciona fuera de Tor Browser, su tráfico no queda protegido por abrir la interfaz en Tor.

Identifiqué como activos sensibles la identidad de las partes, direcciones de billetera, monto, requisitos, pruebas de cumplimiento, sesión y autorización para liberar fondos. Busqué confidencialidad e integridad del payload y reducir la vinculación entre IP y actividad comercial. El servidor y las contrapartes autorizadas deben conocer lo necesario para ejecutar el escrow; la privacidad frente al ISP no implica ocultar una orden al backend que la procesa.

## Comparación de tres escenarios
| Dimensión | Solo HTTPS | Tor hacia clearnet HTTPS | I2P dentro de la red |
|---|---|---|---|
| IP del usuario | El sitio ve la IP de origen o de su NAT | El sitio ve la IP del exit; el guard conoce el origen | El destino de aplicación usa identidad I2P; routers vecinos conocen IPs de transporte |
| Destino | La red observa IP del servidor y a menudo dominio, según DNS/ECH | La red local ve Tor; el exit conoce el destino | Se consultan destinos en netDb; no hay exit clearnet en este escenario |
| Payload | TLS protege el trayecto; el backend lo lee | TLS sigue siendo necesario hasta el sitio; los relays no leen la orden | Cifrado del transporte I2P entre routers; proteger también la frontera aplicación-router |
| Exit o relay malicioso | No hay exit Tor; un intermediario puede bloquear, no descifrar TLS válido | Puede observar destino y tiempos, bloquear o degradar; no leer ni modificar silenciosamente TLS válido | Puede observar su tramo y afectar disponibilidad; no aprende automáticamente ambos extremos |
| Login | Vincula sesión, usuario y escrow | Iniciar sesión identifica al usuario ante Pactrail | La cuenta sigue identificando al usuario ante la aplicación |
| Costo para el usuario | Navegador habitual, menor fricción | Tor Browser, más latencia y posibles verificaciones de acceso | Cliente y acceso a un servicio in-net; no es un reemplazo directo del sitio clearnet |
| Qué ve Pactrail | Sesión, orden, monto, requisitos, wallet e IP | Los mismos datos de negocio; IP de exit | Los mismos datos entregados a la aplicación; destino/sesión I2P |
| Persona Obligada, si aplica | Identificación y registros según régimen | Los deberes no dependen de ver una IP residencial | Tampoco desaparecen por usar una red interna |

## Quién ve qué en Tor + HTTPS
Apliqué este modelo al circuito observado en Tor Browser hacia https://polymarket.com/: Serbia (guard), Estados Unidos (middle) y Suecia (exit). Después de solicitar otro circuito registré Serbia, Suecia y Países Bajos, respectivamente. Las capturas del 12 de octubre muestran 21:33 y 21:34 en el reloj del equipo; usé la red de casa. La identidad exacta del guard no puede verificarse a partir de las IPs tapadas, aunque su país permaneció igual.

| Observador | Puede observar | No obtiene directamente |
|---|---|---|
| Red local / ISP | IP de origen, guard, uso de Tor, tiempos y volumen | Destino final y payload HTTPS |
| Guard | IP de origen, siguiente hop, tiempos y volumen | Destino final y payload |
| Middle | Guard y exit vecinos; tráfico cifrado | IP de origen, sitio y payload |
| Exit | Destino/IP/normalmente dominio, middle y tiempos | IP original, ruta URL, contraseña y JSON bajo TLS válido |
| Sitio | IP del exit, sesión, cookies y contenido recibido | IP original por el transporte Tor por sí solo |

Si el exit fuera malicioso, con este sitio HTTPS podría ver el destino, tiempos y volumen, y no podría ver el contenido del formulario, la contraseña ni el JSON de liberación, suponiendo TLS válido, navegador íntegro y ausencia de filtraciones de aplicación. El sitio sí lee lo que le envío. Una IP compartida del exit nunca reemplazaría la autenticación o la autorización de una transacción.

## Adversarios y límites
Consideré una red local curiosa, un ISP que registra destinos, un relay malicioso, el propio sitio y un gobierno con visibilidad parcial. Ninguno necesita descifrar el contenido para obtener información útil de horarios y frecuencia. Si un adversario alcanza visibilidad suficiente de entrada y salida, la correlación temporal puede vincularlas; no prometí defensa frente a observación global. Una cadena pública futura también podría relacionar montos, wallets y eventos aunque el acceso web llegara por Tor.

Tor no protege tres aspectos centrales de mi flujo: primero, no oculta mi identidad a Pactrail cuando inicio sesión o presento documentos; segundo, no corrige un contrato vulnerable ni impide que una cuenta autorizada mienta sobre un requisito; tercero, no cubre el tráfico de wallet, RPC u otras aplicaciones que no atraviesen Tor Browser. Un endpoint comprometido o un documento abierto fuera del navegador agrega riesgos. Esas limitaciones me llevan a priorizar TLS, autorización por rol y minimización de datos antes de prometer anonimato.

## I2P conceptual y decisión
Estudié I2P como comparación in-net, sin instalarlo ni alojar servicios. El emisor usa un túnel de salida que conecta con uno de entrada del receptor; la respuesta usa otros túneles. netDb ayuda a localizar destinos y garlic encryption protege mensajes agrupados. No hay un exit hacia el sitio clearnet en el caso comparado. Pactrail necesitaría una integración distinta para ser accesible ahí, por lo que esta columna no describe una función del MVP.

Concluí que Pactrail necesita confidencialidad, integridad y autorización de pagos; el anonimato de red puede ser una opción de acceso futuro, pero no elimina identidad contractual ni trazabilidad necesaria. Si su actividad efectiva lo convierte en PSAV o Persona Obligada bajo Decreto 15-2026, identificar al cliente y conservar/reportar lo exigible sigue siendo responsabilidad del servicio. No asigné ese estatus solo por usar Solidity o USAT de prueba ni inventé plazos reglamentarios.

## Observación real y límites del curso
Guardé `tor-antes.png`, `tor-despues.png` y `navegador-normal.png`. Observé el mismo dominio en Tor Browser y Chrome sin sesión iniciada visible. La captura de Chrome demuestra que cargó la página; no mide la IP que recibió el servidor. La diferencia de tema claro/oscuro no demuestra una propiedad de anonimato. No abrí cuentas ni realicé operaciones como parte de esta observación.

La red de casa fue confirmada por mí en el chat. No quedó confirmación expresa de que Polymarket fuera el sitio del pizarrón ni de la procedencia de la instalación y configuración default; las capturas no bastan para verificar esos puntos. Los mantuve como límites de trazabilidad, sin atribuir una aprobación al instructor. Los requisitos siguen siendo Tor Browser oficial, configuración predeterminada y sitios HTTPS públicos y legales permitidos por el curso; no se documentó uso de bridges, relays, onion services ni cambios de selección de nodos.

## Fuentes
- [Tor: HTTPS y visibilidad](https://support.torproject.org/about-tor/security/https-encryption-and-tor/).
- [Tor: guard estable](https://support.torproject.org/tor-browser/security/guard-relay/).
- [I2P: arquitectura](https://i2p.net/en/docs/overview/intro/).
- [Congreso: Decreto 15-2026](https://www.congreso.gob.gt/noticias_congreso/17000/2025/).
- [S20: límites y requisitos](https://docs.google.com/document/d/1CMLsWsaZI_V_n2QbPPnQ3RIW5HvDOoGcgFzVIxbUKDA/edit).
