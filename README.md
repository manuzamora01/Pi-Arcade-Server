# 🎮 Pi Arcade Server

> **Un servidor doméstico robusto para desplegar juegos multijugador en tiempo real usando Raspberry Pi 5, Node.js y túneles seguros.**

![Raspberry Pi](https://img.shields.io/badge/-Raspberry_Pi_5-C51A4A?style=for-the-badge&logo=Raspberry%20Pi&logoColor=white)
![Node.js](https://img.shields.io/badge/-Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white)
![PM2](https://img.shields.io/badge/-PM2-2B037A?style=for-the-badge&logo=pm2&logoColor=white)
![Ngrok](https://img.shields.io/badge/-Ngrok-1F1E37?style=for-the-badge&logo=ngrok&logoColor=white)

## 📋 Sobre el Proyecto

Este repositorio documenta la configuración completa de un servidor de juegos casero capaz de alojar aplicaciones web multijugador (como clones de *Among Us*, *Bingo*, etc.) desarrolladas en **TypeScript/Node.js**.

El objetivo es transformar una **Raspberry Pi 5** en una consola servidor "inmortal", accesible desde cualquier lugar del mundo mediante una URL fija, y gestionable remotamente desde un teléfono móvil sin necesidad de periféricos.

## 🏗️ Arquitectura del Sistema

El sistema funciona con una arquitectura de **"Cartucho Único"** sobre el puerto 5000, expuesto a internet mediante un túnel seguro.

```mermaid
graph TD
    Usuario_Internet["📱 Jugador (Internet)"] -->|"HTTPS"| Ngrok_Tunnel
    Ngrok_Tunnel["☁️ Ngrok (Dominio Fijo)"] -->|"Puerto 5000"| Raspberry_Pi
    
    subgraph Raspberry_Pi ["Raspberry Pi 5"]
        PM2_Gestor["⚙️ PM2 Process Manager"]
        PM2_Gestor -->|"Mantiene vivo"| Juego_Node["👾 Juego Node.js (Puerto 5000)"]
        PM2_Gestor -->|"Mantiene vivo"| Tunel_Ngrok["🚇 Túnel Ngrok"]
    end
    
    Admin_Movil["📲 Admin (Móvil/Termius)"] -.->|"VPN Segura"| Tailscale
    Tailscale["🔒 Tailscale VPN"] -.->|"SSH"| Raspberry_Pi
