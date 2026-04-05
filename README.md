# Proyecto Final: Analisis de Tweets - Sentimiento (n8n + LLMs)

Objetivo:

•	Implementar un workflow automático y robusto en n8n que integre al menos dos nodos de Modelos de Lenguaje Grande (LLMs), condicionales basados en la salida de un LLM y un mecanismo de notificación.

•	El propósito es demostrar la capacidad de diseñar, encadenar y orquestar modelos de lenguaje dentro de un flujo automatizado que reaccione a eventos reales y dirija acciones según lógica condicional.
<img width="468" height="170" alt="image" src="https://github.com/user-attachments/assets/e4071ec6-4ea8-46b1-98d5-434206714fcb" />


Este proyecto presenta un ecosistema automatizado para la captura, análisis y gestión de menciones de marca en redes sociales. Utiliza una arquitectura de **IA Agéntica** para clasificar el sentimiento de los usuarios y disparar protocolos de respuesta inmediata ante crisis reputacionales.

---

## 🎯 Objetivo del Proyecto
El propósito es demostrar la capacidad de diseñar, encadenar y orquestar modelos de lenguaje avanzados (LLMs) dentro de un flujo automatizado que reaccione a eventos reales y dirija acciones según lógica condicional.

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
![Workflow Screenshot](assets/workflow_screenshot.png)  
*(Captura de pantalla de los nodos conectados en n8n)*.

### Demostración en Video
Puedes ver el funcionamiento del sistema, desde la carga del tweet hasta la llegada del correo de alerta, en el siguiente enlace:
[📺 Ver Video Demo](assets/demo.mp4)

---

## 🚀 Cómo Replicar este Proyecto
1.  **Importar Workflow:** Descarga el archivo `Análisis de Tweets - Sentimiento.json` e impórtalo en tu instancia de n8n.
2.  **Configurar Supabase:** Crea una tabla llamada `Tweets` con las columnas necesarias (`Tweet`, `Sentiment`, `created_at`).
3.  **Configurar Credenciales:**
    * API Key de **Google Gemini**.
    * URL y Key de **Supabase**.
    * Autorización **Gmail OAuth2** para el envío de correos.
4.  **Ejecutar:** Activa el flujo y realiza una prueba mediante el formulario de entrada.
