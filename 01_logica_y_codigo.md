# 🧠 Lógica y Código del Juego

Este documento explica la arquitectura interna del juego desplegado. El proyecto, originalmente diseñado para ejecutarse en la nube (Replit), ha sido migrado para funcionar en un entorno local de alto rendimiento (Raspberry Pi 5).

## 🛠️ Stack Tecnológico

El juego utiliza tecnologías web modernas para permitir una comunicación bidireccional en tiempo real entre los jugadores:

* **Runtime:** Node.js (JavaScript/TypeScript en el servidor).
* **Protocolo:** WebSockets (vía `socket.io`) para la comunicación instantánea.
* **Servidor Web:** Express/Vite para servir los archivos del juego.
* **Gestor de Paquetes:** NPM (Node Package Manager).

## 📂 Estructura de Archivos

La estructura organizada en la Raspberry Pi (`/home/pi/juegos/impostor`) sigue este esquema:

```text
/juegos
  └── /impostor
       ├── node_modules/     # Librerías instaladas (Express, Socket.io, etc.)
       ├── public/           # Archivos visibles (HTML, CSS, Imágenes)
       ├── src/              # Código fuente del juego
       ├── index.ts          # Punto de entrada del servidor
       ├── package.json      # "Lista de la compra" de dependencias
       └── vite.config.ts    # Configuración del empaquetador
