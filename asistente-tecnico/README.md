# 🤖 Tech Assist-Bot: Asistente de Soporte Técnico Bilingüe

![Status](https://img.shields.io/badge/Status-Active-success)
![Docker](https://img.shields.io/badge/Docker-Enabled-blue)
![N8N](https://img.shields.io/badge/Orchestrator-n8n-orange)
![AI](https://img.shields.io/badge/Powered%20by-Google%20Gemini-4285F4)

**Tech Assist-Bot** es un sistema automatizado de soporte técnico de nivel 1, diseñado para responder consultas sobre instalación de software, comandos de terminal y errores comunes.

Desplegado sobre una arquitectura de **microservicios en Docker**, este bot es capaz de mantener conversaciones fluidas en **Español e Inglés**, gestionar el contexto de la conversación y registrar todas las interacciones en una base de datos para auditoría y monitoreo en tiempo real.

---

## 🎯 Características Principales

* **🧠 Inteligencia Artificial Generativa:** Utiliza **Google Gemini** para interpretar la intención del usuario y generar respuestas técnicas precisas paso a paso.
* **🌍 Soporte Bilingüe:** Detecta y responde automáticamente en el idioma del usuario (EN/ES) sin configuración adicional.
* **🛡️ Infraestructura Robusta:** Maneja división de mensajes largos (Chunking) para cumplir con los límites de Telegram.
* **📊 Auditoría y Métricas:** Cada interacción (pregunta/respuesta) se registra automáticamente en **PostgreSQL** y se visualiza en **Grafana**.
* **🔒 Acceso Seguro:** Expuesto a internet mediante **Cloudflare Tunnel**, manteniendo la seguridad de la red local sin abrir puertos.
* **⚡ Despliegue Simplificado:** Todo el stack se levanta con un único comando de `docker compose`.

---

## 🏗️ Arquitectura del Sistema

El flujo de datos sigue el siguiente proceso:

1.  **Entrada:** El usuario envía un mensaje vía **Telegram**.
2.  **Acceso:** **Cloudflare Tunnel** recibe el webhook y lo redirige seguramente al contenedor local.
3.  **Orquestación (n8n):**
    * **Trigger:** Recibe el mensaje.
    * **Rate Limit:** Verifica que el usuario no exceda el límite de consultas.
    * **AI Agent:** Procesa el texto usando el modelo **Google Gemini**.
    * **Logging:** Guarda la consulta y la respuesta en **PostgreSQL**.
    * **Enriquecimiento:** Formatea la respuesta y añade recursos estáticos si es necesario.
    * **Entrega:** Divide la respuesta si es muy larga y la envía a Telegram.
4.  **Monitoreo:** **Grafana** lee la base de datos y actualiza los dashboards de uso.

---

## 🛠️ Stack Tecnológico

| Componente | Tecnología | Uso |
| :--- | :--- | :--- |
| **Orquestador** | [n8n](https://n8n.io/) | Lógica del flujo de trabajo y conexiones API. |
| **Modelo LLM** | [Google Gemini](https://deepmind.google/technologies/gemini/) | Cerebro del agente para generación de texto. |
| **Base de Datos** | [PostgreSQL 14](https://www.postgresql.org/) | Almacenamiento persistente de logs y auditoría. |
| **Visualización** | [Grafana OSS](https://grafana.com/) | Dashboards de métricas y KPIs. |
| **Administración DB** | [pgAdmin 4](https://www.pgadmin.org/) | Gestión visual de la base de datos. |
| **Contenedores** | [Docker Compose](https://docs.docker.com/compose/) | Despliegue y gestión de servicios. |

---

## 📂 Estructura del Proyecto

```text
tech-assist-bot/
├── docker-compose.yml      # Definición de todos los servicios
├── .env                    # Variables de entorno (Credenciales)
├── INSTALACION.md          # Guía paso a paso para el despliegue
├── README.md               # Documentación general
├── workflows/              # Copias de seguridad de los flujos de n8n (.json)
│   └── main_flow.json
└── data/                   # Directorios de persistencia (creados al desplegar)
    ├── n8n_data/
    ├── postgres_data/
    ├── pgadmin_data/
    └── grafana_data/

##Dashboard Usando Grafana##
http://dash.wfgongora.work/dashboard/snapshot/FFR1p60ykiZSRz9O95zT0MHFHbrN5GeX


## 🤝 Contribuciones

Para reportar bugs o sugerir mejoras, por favor abra un *Issue*.

**Desarrollado por:** William Gongora y Cristian Carabali
