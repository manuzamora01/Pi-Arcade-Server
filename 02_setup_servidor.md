# 🖥️ Setup del Servidor (Raspberry Pi)

Este documento detalla el proceso de preparación del hardware y el entorno de software necesario para alojar la aplicación, partiendo de una instalación limpia de Raspberry Pi OS.

## 🍓 Hardware Utilizado

* **Modelo:** Raspberry Pi 5 (8GB RAM).
* **Sistema Operativo:** Raspberry Pi OS (64-bit) basado en Debian Bookworm.
* **Almacenamiento:** Tarjeta MicroSD de alta velocidad.

## 📦 Instalación del Entorno (Node.js)

La versión de Node.js que viene por defecto en los repositorios de Raspberry Pi (`apt`) suele estar desactualizada (v12 o v14). Para ejecutar proyectos modernos con TypeScript, fue necesario actualizar manualmente a una versión estable reciente (v20+).

### Pasos realizados en la Terminal:

1.  **Limpieza de caché antigua:**
    Para evitar conflictos con versiones previas.
    ```bash
    sudo npm cache clean -f
    ```

2.  **Instalación del gestor de versiones 'n':**
    Esta herramienta permite cambiar de versión de Node.js con un solo comando.
    ```bash
    sudo npm install -g n
    ```

3.  **Actualización a la versión estable:**
    ```bash
    sudo n stable
    ```
    *Resultado:* Se instaló la última versión LTS de Node.js, solucionando errores de compatibilidad como `Invalid comparator: npm:tsx` que impedían arrancar el proyecto.

## 📂 Transferencia de Archivos (Despliegue)

Para mover el código desde el PC de desarrollo a la Raspberry Pi, se optó por el protocolo **SFTP** (SSH File Transfer Protocol) por su seguridad y simplicidad, ya que usa el mismo puerto (22) que la consola.

* **Cliente FTP:** Se utilizó **WinSCP** (Windows) por su integración limpia con el sistema de archivos de Linux.
* **Ruta de Destino:**
    Se creó una estructura ordenada en el directorio del usuario principal:
    `~/juegos/impostor`

### ⚠️ Nota sobre Archivos Ocultos
Durante la transferencia, fue crucial habilitar la opción **"Mostrar archivos ocultos"** en WinSCP (o usar `ls -a` en terminal) para asegurar que los archivos de configuración críticos que empiezan por punto se copiaran correctamente:
* `.env` (Variables de entorno).
* `.gitignore` (Configuración de Git).

---
_Siguiente paso: Ver cómo automatizamos el encendido para que el servidor sea "inmortal" en [03_gestion_procesos.md](./03_gestion_procesos.md)._
