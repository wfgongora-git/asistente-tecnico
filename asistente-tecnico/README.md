# 🤖 Tech Assist-Bot: Asistente de Soporte Técnico UJaveriana (v1.0)

Este repositorio contiene la implementación del **Asistente de Soporte Técnico**, un sistema automatizado diseñado para procesar consultas técnicas comunes (software, comandos, errores) y proporcionar respuestas precisas y útiles a través de Telegram.

El sistema  está construido sobre un stack de contenedores Docker para asegurar portabilidad y escalabilidad.

---

## 🎯 Criterios del Proyecto

El proyecto cumple con los siguientes requisitos funcionales y técnicos:

| Componente | Tecnología | Propósito |
| :--- | :--- | :--- |
| **Orquestación** | `n8n` | Flujo de trabajo y lógica del Agente de IA. |
| **Modelo Base** | `OpenAI` | Inteligencia y capacidad de respuesta bilingüe. |
| **Base de Conocimiento** | `PostgreSQL Chat Memory` | Recuerda las conversaciones de cada chat ID |
| **Mensajería** | `Telegram API` | Interfaz de usuario final. |
| **Visualización** | `Grafana` | Métricas y auditoría de uso del bot. |

---

## ✨ Características Clave

* **Respuesta Bilingüe:** Soporte nativo en **Español e Inglés**.
* **Tono Consistente:** Configurado con el *System Prompt* "Tech Assist-Bot" para ofrecer soluciones claras, directas y en formato de pasos.
* **Auditoría:** Todas las interacciones se registran en PostgreSQL para métricas y seguimiento.

---

## 🚀 Instalación y Despliegue

Sigue la guía detallada en `INSTALACION.md` para desplegar el stack completo de Docker.

**Requisitos Previos:**
* Docker y Docker Compose
* Acceso a un servidor Linux (ej. Proxmox LXC)

### 🛠️ Configuración Rápida

1.  Clonar el repositorio.
2.  Configurar las claves de API de **Telegram**  y **OpenAI**
3.  Ejecutar el stack: `docker compose up -d`

---

## 🤝 Contribuciones

Para reportar bugs o sugerir mejoras, por favor abra un *Issue*.

**Desarrollado por:** William Gongora y Cristian Carabali
