# Proyecto Final: Analisis de Tweets - Sentimiento (n8n + LLMs)

Este proyecto automatiza el procesamiento de tweets utilizando n8n, IA (Gemini) y Supabase. El flujo captura cada mensaje, clasifica su sentimiento (Positivo/Negativo) mediante un LLM y genera un análisis detallado en caso de detectar críticas. Todos los datos y resultados se persisten de forma estructurada en una base de datos SQL para su posterior consulta.

---

## 🎯 Objetivo del Proyecto
•	Implementar un workflow automático y robusto en n8n que integre al menos dos nodos de Modelos de Lenguaje Grande (LLMs), condicionales basados en la salida de un LLM y un mecanismo de notificación.

•	El propósito es demostrar la capacidad de diseñar, encadenar y orquestar modelos de lenguaje dentro de un flujo automatizado que reaccione a eventos reales y dirija acciones según lógica condicional.

## 🛠️ Stack Tecnológico
* **Orquestador:** n8n (Workflow Automation).
* **IA / LLM:** Google Gemini 2.5 Flash.
* **Base de Datos:** Supabase (PostgreSQL).
* **Notificaciones:** Gmail API.
* **Lenguajes:** JavaScript (para transformación de datos y limpieza de JSON).

---

## 🏗️ Arquitectura del Workflow (Paso a Paso)

### 1. Ingesta y Persistencia
El flujo inicia mediante un **Webhook (Form Trigger)** que simula la recepción de un Tweet.
* Se realiza una limpieza de datos mediante un nodo de **JavaScript**.
* El tweet se inserta inmediatamente en una tabla de **Supabase** para mantener trazabilidad histórica.

### 2. Clasificador de Sentimiento (LLM Nodo 1)
Se utiliza un nodo de **Gemini** con un rol especializado en análisis emocional de textos en español latinoamericano.
* **Input:** Texto del tweet.
* **Output esperado:** `POSITIVO` o `NEGATIVO`.
* Un nodo de código normaliza la respuesta para asegurar la robustez del flujo ante variaciones de la IA.

### 3. Lógica de Negocio (Condicionales)
Mediante un nodo **Switch**, el sistema decide el camino a seguir basándose en la salida del primer LLM:
* **Ruta Positiva:** Registra un log de éxito para métricas de BI y finaliza la ejecución.
* **Ruta Negativa:** Activa el protocolo de análisis profundo de crisis.

### 4. Analista de Crisis y Alerta (LLM Nodo 2)
Para tweets negativos, entra en juego un segundo nodo de **Gemini** con el rol de **Analista de Redes Sociales**.
* Genera un resumen ejecutivo de la queja.
* Identifica los motivos específicos del descontento.
* Entrega la información en formato **JSON estructurado**.

### 5. Notificación Automatizada
El sistema construye un cuerpo de correo dinámico y lo envía vía **Gmail** al equipo de soporte, permitiendo una reacción en minutos ante comentarios críticos.

---

## 📸 Evidencia Visual

### Diagrama del Workflow
<img width="1220" height="379" alt="image" src="https://github.com/user-attachments/assets/04dbd41e-fb5c-482f-ae74-002a96028c89" />

### Formulario para publicar Tweets
<img width="474" height="288" alt="image" src="https://github.com/user-attachments/assets/b3252021-8e56-4853-b842-c92c24c8e586" />

### Tabla de Base de Datos Supabase
<img width="1122" height="365" alt="image" src="https://github.com/user-attachments/assets/113ab1c9-1f32-4a75-a7f5-2b5b19cd592d" />

### Prompts LLM
- Nodo 1 - Clasificación

Eres un clasificador de sentimiento especializado en tweets en español latinoamericano. Devuelve únicamente NEGATIVO o POSITIVO en mayúsculas. Tweet: {{ $json.Tweet }}

- Nodo 2 - Resumen - Explicación

Eres un asistente de análisis de sentimientos.

Se te proporciona el siguiente tweet:
"{{ $json.tweet }}"

Este tweet fue clasificado como NEGATIVO.

Tu tarea es:
1. Escribir un resumen conciso del tweet (máximo 2 oraciones).
2. Explicar por qué fue clasificado como negativo, identificando las causas o motivos específicos presentes en el texto.

Responde ÚNICAMENTE en el siguiente formato JSON, sin texto adicional ni bloques de código:
{
  "resumen": "...",
  "explicacion": "..."
}

### Ejecuciones

<img width="1037" height="723" alt="image" src="https://github.com/user-attachments/assets/0e55e4f2-661a-4074-b284-c7f763dd9acd" />

### Form

<img width="841" height="144" alt="image" src="https://github.com/user-attachments/assets/194aa2a6-025b-4cd2-ab3c-c3302043cbfe" />

### Code Javascript

<img width="843" height="129" alt="image" src="https://github.com/user-attachments/assets/3dbc6a9b-928b-48db-bb79-69deacc7d5e3" />

### Creacion Fila in Supabase

<img width="841" height="171" alt="image" src="https://github.com/user-attachments/assets/e68092f9-7adb-48dd-ab5f-2b802ced9dad" />

### Nodo LLM 1

<img width="844" height="167" alt="image" src="https://github.com/user-attachments/assets/672253fe-0f7f-4ee0-87ee-43e5bc5a29cc" />

### Parsear Sentimiento

<img width="853" height="232" alt="image" src="https://github.com/user-attachments/assets/a611d6a6-1263-45eb-b45c-099140bfc13e" />

### Actualización Columna Sentiment en Base de Datos

<img width="839" height="142" alt="image" src="https://github.com/user-attachments/assets/06e3100c-5768-4b14-afd1-c7e14315349c" />

### Nodo LLM 2

<img width="842" height="152" alt="image" src="https://github.com/user-attachments/assets/8a954fcc-bd0d-44ba-b765-0240dca493e6" />

### Armar Email 

<img width="846" height="267" alt="image" src="https://github.com/user-attachments/assets/bb0ad3a5-5b86-4971-8c14-f7ab87049c80" />

### Envío Email

<img width="842" height="241" alt="image" src="https://github.com/user-attachments/assets/6bc3021e-fb3e-40fc-8d03-fd59419deacc" />








