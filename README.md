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
