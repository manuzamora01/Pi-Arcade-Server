# ⚙️ Gestión de Procesos (PM2)

Una vez que el código está en la Raspberry Pi, el siguiente desafío es mantenerlo vivo. Si ejecutamos el servidor con un simple `npm run dev`, el juego se apaga en cuanto cerramos la terminal SSH.

Para solucionar esto, utilizamos **PM2** (Process Manager 2), una herramienta de producción para Node.js que permite mantener aplicaciones activas en segundo plano, reiniciarlas automáticamente si fallan y gestionarlas como servicios del sistema.

## 🚀 Instalación

PM2 se instala globalmente para poder controlar cualquier proceso en la máquina:

```bash
sudo npm install -g pm2
```

## 🧠 Estrategia de "Doble Proceso"

Para que el sistema Arcade funcione, necesitamos dos piezas de software ejecutándose simultáneamente y sin interrupciones. Configuramos PM2 para manejar ambas:

### 1. El Juego (Backend)
En lugar de ejecutar el comando de desarrollo manual, delegamos la tarea a PM2. Esto arranca el servidor Node.js en el puerto 5000.

```bash
# Comando utilizado para iniciar el juego
pm2 start npm --name "impostor" -- run dev
```

### 2. El Túnel (Ngrok)
El túnel que expone el puerto 5000 a internet también debe ser gestionado como un servicio. Se configuró para usar el dominio fijo reservado.

```bash
# Comando para iniciar el túnel persistente
pm2 start ngrok --name "tunel-web" -- http --domain=restful-cliquish-tamera.ngrok-free.dev 5000
```
Resultado: Ahora tenemos dos procesos (impostor y tunel-web) con estado "online" en la lista de PM2.

## 🛡️ Persistencia (Inmortalidad)
La característica más crítica de esta configuración es la capacidad de sobrevivir a un corte de luz o reinicio de la Raspberry Pi.

Para lograr esto, se generó un script de arranque que se integra con systemd (el sistema de inicio de Linux):

Generar el script:

```bash
pm2 startup
```
(Este comando genera una línea de código específica que se debe copiar y pegar en la terminal para registrar el servicio).

Congelar la lista actual: Una vez que los procesos están funcionando como queremos, guardamos la "foto" actual de la configuración:

```bash
pm2 save
```
Gracias a esto, si la Raspberry Pi se reinicia, PM2 arrancará automáticamente y lanzará tanto el juego como el túnel sin intervención humana.

## 📊 Monitorización y Logs
A diferencia de una terminal normal, aquí no vemos los errores en pantalla. Para depurar o buscar el código QR de conexión, utilizamos el sistema de logs de PM2:

```bash
# Ver las últimas 100 líneas del log del túnel
pm2 logs tunel-web --lines 100

# Ver el estado de todos los procesos (CPU/Memoria)
pm2 list
```
