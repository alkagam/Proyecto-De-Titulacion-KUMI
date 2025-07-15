# Proyecto-De-Titulacion-KUMI

Un Sistema Conversacional Autónomo para la Interacción Proactiva con el Cliente
Autor: Saul Andrawi Lopez Martinez

Institución: Instituto Politécnico Nacional - Escuela Superior de Ingeniería Eléctrica y Mecánica

1. Descripción del Proyecto
Este proyecto de titulación presenta el diseño e implementación de un sistema conversacional autónomo y omnicanal. A diferencia de los chatbots tradicionales que siguen flujos predefinidos, este sistema utiliza Modelos de Lenguaje Extensos (LLMs) y una arquitectura de Generación Aumentada por Recuperación (RAG) para ofrecer interacciones inteligentes, contextualizadas y proactivas.

El sistema es capaz de comunicarse con los clientes a través de múltiples canales (llamadas de voz, WhatsApp y chat web) y se alimenta de una base de conocimiento dinámica que incluye documentos internos (PDFs) y datos extraídos en tiempo real de la web mediante web scraping. El objetivo es crear un asistente de ventas y soporte que pueda resolver dudas, ofrecer recomendaciones y mejorar la experiencia del cliente de manera significativa.

2. Características Principales
Omnicanalidad: Interacción unificada a través de llamadas de voz, WhatsApp y un chat web integrado.

Base de Conocimiento Dinámica: El sistema aprende de documentos PDF, archivos de texto y contenido web actualizado mediante web scraping.

Inteligencia Conversacional (RAG): No depende de flujos rígidos. Comprende la intención del usuario y busca en su base de conocimiento para generar respuestas relevantes y precisas.

Procesamiento de Voz: Capacidad de mantener conversaciones habladas fluidas gracias a la integración de tecnologías de transcripción y síntesis de voz.

Dashboard de Analítica: Una interfaz de administración para monitorear KPIs, analizar conversaciones y obtener insights para la mejora continua.

Arquitectura Escalable y Moderna: Construido con un stack tecnológico moderno, utilizando microservicios "contenerizados" con Docker y un pipeline de CI/CD para despliegues automatizados.

3. Arquitectura del Sistema
El sistema sigue una arquitectura de microservicios orquestada por un núcleo de IA.

Canales de Entrada: Twilio gestiona las interacciones de Voz y WhatsApp. Un frontend de JavaScript maneja el Chat Web.

API Gateway (FastAPI): Un backend de Python recibe todas las solicitudes de los canales.

Núcleo de IA (LangChain):

Orquestación: LangChain maneja el flujo de la conversación.

Ingesta: BeautifulSoup (Web Scraping) y PyPDF2 (Documentos) alimentan el sistema.

Memoria Semántica: Sentence-Transformers convierte el conocimiento en embeddings, que se almacenan en la base de datos vectorial ChromaDB.

Razonamiento: Se construye un prompt con el contexto recuperado y se envía a un LLM (OpenAI API) para generar la respuesta.

Canales de Salida: La respuesta se devuelve al usuario a través del canal original. Para la voz, se utiliza la API de Google Cloud Text-to-Speech.

Dashboard (React): Un frontend separado consume una API de analítica para visualizar las interacciones y el rendimiento del sistema.

4. Stack Tecnológico
Área

Tecnologías

Backend

Python, FastAPI

Frontend (Dashboard)

React, JavaScript, HTML5, CSS3, Tailwind CSS

IA y Machine Learning

LangChain, OpenAI API, Sentence-Transformers, PyPDF2, BeautifulSoup

Bases de Datos

ChromaDB (Vectorial), Pandas (para análisis de datos)

Comunicaciones

Twilio (Voz, WhatsApp), Google Cloud (Speech-to-Text, Text-to-Speech)

DevOps

Docker, Docker Compose, GitHub Actions (CI/CD)

5. Instalación y Puesta en Marcha
Sigue estos pasos para configurar y ejecutar el proyecto en un entorno local.

Prerrequisitos
Tener instalado Docker y Docker Compose.

Tener una cuenta de OpenAI y una clave de API.

Tener una cuenta de Twilio con un número de teléfono y el Sandbox de WhatsApp activado.

Tener una cuenta de Google Cloud Platform con las APIs de Speech-to-Text y Text-to-Speech habilitadas.

Tener instalado Git.

Pasos de Instalación
Clonar el repositorio:

git clone https://github.com/tu-usuario/tu-repositorio.git
cd tu-repositorio

Configurar las variables de entorno:

Crea una copia del archivo .env.example y renómbrala a .env.

Abre el archivo .env y rellena todas las claves de API y credenciales de tus cuentas (OpenAI, Twilio, Google Cloud).

# .env
OPENAI_API_KEY="sk-..."
TWILIO_ACCOUNT_SID="..."
TWILIO_AUTH_TOKEN="..."
TWILIO_PHONE_NUMBER="..."
GOOGLE_APPLICATION_CREDENTIALS_JSON="..."

Poblar la base de conocimiento:

Coloca tus archivos PDF y de texto en la carpeta documents.

Configura las URLs que quieres "scrapear" en el script de ingesta.

Construir y ejecutar los contenedores:

Abre una terminal en la raíz del proyecto y ejecuta el siguiente comando:

docker-compose up --build

Este comando construirá las imágenes de Docker para cada servicio (backend, frontend, etc.) y los iniciará.

Configurar los Webhooks (usando ngrok):

Una vez que los contenedores estén en funcionamiento, el backend estará disponible en http://localhost:8000.

Para que Twilio pueda comunicarse con tu máquina local, necesitas exponer este puerto a internet. Abre una nueva terminal y ejecuta:

ngrok http 8000

Ngrok te dará una URL pública (ej. https://aleatorio.ngrok.io).

Ve a tu dashboard de Twilio y actualiza los webhooks de tu número de teléfono y del sandbox de WhatsApp para que apunten a https://aleatorio.ngrok.io/api/voice y https://aleatorio.ngrok.io/api/whatsapp respectivamente.

6. Uso del Sistema
Canal de Voz: Llama al número de teléfono que configuraste en Twilio.

Canal de WhatsApp: Envía un mensaje al número del sandbox de WhatsApp de Twilio.

Chat Web: Abre http://localhost:3001 (o el puerto que hayas configurado para el chat web) en tu navegador.

Dashboard de Analítica: Abre http://localhost:3000 (o el puerto del dashboard) para ver las métricas y conversaciones.

7. Estructura del Proyecto
.
├── backend/            # Código del backend en FastAPI
│   ├── app/
│   │   ├── api/        # Endpoints de la API
│   │   ├── core/       # Lógica de negocio, IA
│   │   └── services/   # Clientes para APIs externas
│   └── Dockerfile
├── dashboard/          # Código del frontend en React
│   ├── src/
│   └── Dockerfile
├── documents/          # PDFs y otros documentos para la base de conocimiento
├── .env                # Variables de entorno (NO subir a Git)
├── .env.example        # Plantilla para las variables de entorno
├── docker-compose.yml  # Orquestación de los contenedores
└── README.md
