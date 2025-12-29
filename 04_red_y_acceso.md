# 🌐 Red y Acceso Público (Ngrok)

El mayor desafío de un servidor doméstico es hacerlo accesible a usuarios externos (amigos conectados por 4G/5G) sin comprometer la seguridad de la red local y sin depender de la dirección IP pública dinámica del hogar.

## 🚧 El Problema: CGNAT y Puertos

Tradicionalmente, para exponer un servidor se requiere:
1.  Abrir puertos en el router (Port Forwarding).
2.  Configurar un DNS Dinámico (DDNS) para rastrear la IP pública.
3.  Lidiar con el CGNAT de los proveedores de internet que impide la conexión directa.

Este enfoque es inseguro y complejo de mantener.

## 🚇 La Solución: Tunneling Seguro

Para este proyecto, se implementó una arquitectura de **Túnel Inverso** utilizando **Ngrok**.

### ¿Cómo funciona?
En lugar de abrir una puerta en el router para que "entren" conexiones, la Raspberry Pi abre una conexión de salida segura hacia la nube de Ngrok.
1.  La Raspberry inicia el agente de Ngrok.
2.  Se establece un túnel encriptado (TLS).
3.  Ngrok asigna una URL pública (`https://...`).
4.  Cuando un usuario entra a esa URL, Ngrok reenvía la petición a través del túnel hasta el **puerto 5000** de la Raspberry Pi (donde vive el juego).

## 🔗 Dominio Fijo (Static Domain)

Inicialmente, se utilizaron soluciones efímeras (`localhost.run`) que cambiaban la URL con cada reinicio. Esto era problemático para la experiencia de usuario.

Se migró a una cuenta gratuita de Ngrok configurada con un **Dominio Estático (Static Domain)**. Esto garantiza que el enlace sea permanente, incluso si el servidor se reinicia o se va la luz.

* **Dominio asignado:** `restful-cliquish-tamera.ngrok-free.dev`
* **Comando de vinculación:**
    ```bash
    ngrok http --domain=restful-cliquish-tamera.ngrok-free.dev 5000
    ```
    *(Este comando es ejecutado automáticamente por PM2, ver documento anterior).*

## 🔒 Seguridad y SSL

Una ventaja crítica de esta configuración es que **Ngrok gestiona automáticamente los certificados SSL/HTTPS**.
* El tráfico viaja encriptado desde el móvil del jugador hasta la nube de Ngrok.
* No es necesario gestionar certificados locales con Let's Encrypt ni configurar Nginx manualmente.

### Aviso de Interstitial
Al usar el plan gratuito, Ngrok añade una capa de seguridad extra: una página de advertencia ("Visit Site") en la primera visita. Esto protege contra phishing, alertando al usuario de que está accediendo a un servidor personal.

---
_Siguiente paso: Cómo administramos todo esto desde el móvil sin estar en casa en [05_administracion_remota.md](./05_administracion_remota.md)._
