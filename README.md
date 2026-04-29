# Taller de Diseño y Construcción de Pipelines CI/CD

**Estudiante:** Juan David Quintero Peña  
**Asignatura:** Ingeniería de Software V  
**Institución:** Universidad (Taller Jenkins)

---

## 1. Introducción
Este proyecto consiste en una aplicación desarrollada en **Spring Boot (Java)**, cuyo ciclo de vida de desarrollo y despliegue ha sido completamente automatizado. Se ha implementado un **Pipeline Declarativo en Jenkins**, permitiendo que cada cambio en el código fuente sea validado, analizado, empaquetado y desplegado de forma segura mediante prácticas modernas de DevOps.

## 2. Arquitectura y Herramientas
Para la construcción de este ecosistema de Integración y Despliegue Continuo (CI/CD), se han integrado las siguientes tecnologías:

*   **Jenkins:** Orquestador principal encargado de ejecutar las etapas del pipeline.
*   **Docker:** Utilizado para la contenedorización de la aplicación, garantizando portabilidad entre entornos.
*   **SonarQube (SAST):** Herramienta de análisis estático de código para la detección de deuda técnica, bugs y vulnerabilidades en el código fuente.
*   **Trivy:** Escáner de seguridad especializado en buscar vulnerabilidades críticas en las capas de las imágenes Docker antes de su despliegue.
*   **Maven (maven-3):** Gestor de dependencias y construcción del proyecto Java.

## 3. Descripción del Pipeline
El archivo \Jenkinsfile\ define las siguientes etapas secuenciales:

1.  **Checkout:** Realiza la clonación automática del código desde el repositorio de GitHub (SCM).
2.  **Build:** Compila el proyecto generando el artefacto \.jar\ mediante el comando \mvn clean package\, utilizando la configuración de **maven-3**.
3.  **Static Analysis (SonarQube):** Fase de análisis estático integrada con el servidor SonarQube para asegurar la calidad del software.
4.  **Docker Build:** Construye una imagen Docker optimizada basada en la distribución **eclipse-temurin:17-jdk-alpine**.
5.  **Container Security Scan (Trivy):** Ejecuta un análisis de seguridad sobre la imagen construida, filtrando por severidades **HIGH** y **CRITICAL**.
6.  **Deploy:** Despliega automáticamente el contenedor, mapeando el servicio interno al **puerto 80** del host para su acceso público.

## 4. Personalización y Evidencias
Como parte de los requisitos del taller, se realizó una personalización en la capa de presentación de la aplicación:
*   Se modificó la vista principal (\index.html\) para mostrar el mensaje: **'Taller CI/CD - Juan'**.
*   Se ajustaron las rutas de la API en el controlador para servir correctamente el contenido estático.

## 5. Mantenimiento del Entorno
Para garantizar la eficiencia del servidor de automatización y optimizar el uso de disco, el pipeline incluye un bloque \post { always { ... } }\ que ejecuta la instrucción \cleanWs()\. Esto asegura que el workspace se limpie al finalizar cada ejecución, sin importar si el build fue exitoso o fallido.

---
*Este repositorio es parte de la entrega práctica del taller de Jenkins del curso de Ingeniería de Software V.*
