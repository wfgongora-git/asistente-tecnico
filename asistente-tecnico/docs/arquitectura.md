# Arquitectura del Chatbot de Telegram

## Descripción General

Este diagrama representa la arquitectura del flujo de n8n que implementa un chatbot conversacional con memoria persistente.

## Componentes Principales

### 1. Entrada
El bot de Telegram recibe mensajes de los usuarios.

### 2. Procesamiento Central
- **Telegram Trigger**: Captura los mensajes entrantes
- **AI Agent**: Procesa las solicitudes utilizando un modelo de OpenAI y mantiene el contexto de la conversación en PostgreSQL

### 3. Acciones Secuenciales
- Inserción de datos en PostgreSQL
- Procesamiento con código JavaScript personalizado
- Envío de respuesta al usuario vía Telegram

### 4. Salida
El mensaje procesado se envía de vuelta al usuario en Telegram.

## Características del Flujo

El flujo está diseñado como un chatbot conversacional con memoria persistente que puede ejecutar acciones como guardar información en base de datos y procesar lógica personalizada antes de responder.

```mermaid
graph TB
    subgraph "Entrada"
        TG[Telegram Bot]
    end
    
    subgraph "Procesamiento"
        TRIGGER[Telegram Trigger<br/>Updates: message]
        AGENT[AI Agent<br/>Chat Model + Memory + Tool]
        OCM[(OpenAI Chat Model)]
        PCM[(Postgres Chat Memory)]
    end
    
    subgraph "Acciones"
        INSERT[Insert rows in a table<br/>PostgreSQL: Insert]
        CODE[Code in JavaScript<br/>Procesamiento de datos]
        SEND[Send a text message<br/>sendMessage: message]
    end
    
    subgraph "Salida"
        TG_OUT[Telegram Bot<br/>Respuesta al usuario]
    end
    
    TG -->|Mensaje del usuario| TRIGGER
    TRIGGER -->|Activa flujo| AGENT
    
    OCM -.->|Modelo de IA| AGENT
    PCM -.->|Historial de conversación| AGENT
    
    AGENT -->|Procesa mensaje| INSERT
    INSERT -->|Guarda en DB| CODE
    CODE -->|Prepara respuesta| SEND
    SEND -->|Envía mensaje| TG_OUT
    
    AGENT -.->|Usa herramientas| INSERT
    
    style TRIGGER fill:#345e5s
    style AGENT fill:#345e5t
    style OCM fill:#8E44AD
    style PCM fill:#3498DB
    style INSERT fill:#16A085
    style CODE fill:#F39C12
    style SEND fill:#2980B9
    style TG fill:#0088cc
    style TG_OUT fill:#0088cc
```
