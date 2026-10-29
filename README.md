# Informe de auditoría: Seguridad en redes Wi-Fi públicas

## Introducción
El objetivo de este informe es identificar la superficie de exposición de los datos del usuario frente a posibles ataques y plantear mecanismos de mitigación efectivos.

## 1. Sitio analizado
* **URL solicitada:** `http://funfinefreshstars.neverssl.com/online/` (Sitio base: `http://neverssl.com`)
* **Host visitado:** `funfinefreshstars.neverssl.com` (puerto estándar HTTP 80)
* **Dirección remota:** `34.223.124.45:80`
* **Protocolo:** HTTP/1.1 (sin cifrado TLS/SSL).
* **Método de petición:** `GET`
* **Código de estado:** `200 OK`

A diferencia de HTTPS (puerto 443), que implementa el protocolo TLS, el sitio analizado opera mediante HTTP plano a través del puerto 80. Lo que conlleva a que no exista cifrado en la capa de transporte ni validación de identidad mediante certificados SSL/TLS.

## 2. Evidencia observada
A partir de la inspección en las Herramientas de Desarrollador (F12 y en la pestaña *Network*), se registraron los siguientes elementos transmitidos en texto plano:

1. **URL completa en la petición:**
   * URL: `http://funfinefreshstars.neverssl.com/online/`
   * Método: `GET`
   * Código de estado: `200 OK`
   * Al viajar sin cifrar, un intermediario en la red local puede interceptar el destino exacto y la ruta consultada mediante un sniffer.

2. **Cabeceras de petición del cliente (Request Headers enviados):**
   * `Host: funfinefreshstars.neverssl.com`: Identifica claramente el servidor de destino solicitado.
   * `User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X ...)`: Expone con precisión el sistema operativo, navegador y motor de renderizado del cliente.
   * `Accept-Language: en-US,en;q=0.9,es;q=0.8`: Informa los idiomas configurados en el sistema del usuario.
   * `Accept-Encoding: gzip, deflate`: Especifica los formatos de compresión admitidos.

<img width="1431" height="685" alt="Screenshot 2026-10-29 at 11 31 59 AM" src="https://github.com/user-attachments/assets/15d25dc5-335d-4986-b4d8-6821fdf1d87e" />

## 3. Riesgos encontrados al navegar en Wi-Fi Pública
En una red Wi-Fi compartida o abierta los principales riesgos son:

* **Espionaje pasivo:** Un atacante mediante herramientas de análisis de paquetes (como Wireshark) puede capturar credenciales de acceso, historiales de navegación y datos confidenciales.
* **Secuestro de sesión:** Robo de cookies de autenticación que permiten suplantar la identidad de la víctima en la plataforma.
* **Inyección de contenido y manipulación:** Al no contar con validación de integridad, el atacante puede inyectar código malicioso, anuncios no deseados o redirigir el tráfico hacia portales de phishing sin que el navegador lo advierta.

## 4. Cómo Ayuda una VPN: Mitigación Técnica
El uso de una Red Privada Virtual (VPN) altera sustancialmente el escenario de exposición:

* **Túnel seguro y encapsulamiento:** La VPN crea un túnel virtual punto a punto entre el dispositivo cliente y el servidor VPN. Mediante el encapsulamiento, cada paquete de datos original se envuelve dentro de un nuevo paquete de red seguro.
* **Cifrado:** Todo el tráfico generado se somete a un cifrado simétrico. Cualquier actor intermedio en la red Wi-Fi pública solo verá bytes cifrados hacia una única dirección IP (la del servidor VPN).
* **Protección del tráfico y privacidad:** La VPN enmascara la IP de origen y oculta los dominios visitados frente al operador del Wi-Fi o atacantes, asegurando la integridad y confidencialidad del tráfico desde el host hasta la salida del túnel.

## 5. Mis 3 Reglas de oro para el uso de redes Wi-Fi públicas
1. **Priorizar HTTPS y forzar HTTPS-First:** No ingresar jamás credenciales ni datos sensibles en sitios HTTP. Utilizar extensiones o modos del navegador que fuercen HTTPS estricto.
2. **Usar siempre una VPN confiable:** Activar una VPN segura antes de iniciar cualquier intercambio de paquetes en una red abierta o desconocida para garantizar el cifrado y encapsulamiento de toda la actividad.
3. **Desactivar el emparejamiento automático y compartir archivos:** Inhabilitar la conexión automática a redes Wi-Fi abiertas, apagar el Bluetooth cuando no se use y desactivar las opciones de compartir carpetas o descubrimiento de red local en el sistema operativo.
