# Módulo 1: Configuración del Entorno
## 🛠️ Preparando tu Espacio de Trabajo en OpenShift AI

---

### 📍 Navegación
[⬅️ Volver a la Guía Principal](../README.md) | [➡️ Siguiente: Modelo Predictivo](02-modelo-predictivo.md)

---

**Duración:** 30 minutos  
**Objetivo:** Configurar un entorno de desarrollo completo para el despliegue de modelos de IA.

---

## 🎯 Resumen del Módulo

En este módulo aprenderás a:
- ✅ Verificar la instalación de OpenShift AI y sus componentes
- ✅ Crear y configurar un Proyecto de Ciencia de Datos
- ✅ Configurar Jupyter Workbench con los recursos necesarios
- ✅ Descargar datasets y instalar dependencias
- ✅ Validar la configuración completa del entorno

---

## 🔍 Paso 1.1: Verificar Instalación de OpenShift AI

### Acceso al Dashboard de OpenShift AI

**OpenShift AI** es una plataforma integral que proporciona herramientas para el ciclo completo de Machine Learning. Incluye componentes esenciales como:

- **Proyectos de Ciencia de Datos** - Espacios de trabajo organizados para equipos de ML
- **Jupyter Hub** - Entornos de desarrollo interactivos
- **Model Serving** - Despliegue de modelos con KServe
- **Model Registry** - Gestión de versiones de modelos

### Verificación de Componentes

Antes de comenzar, es importante confirmar que todos los componentes están disponibles y funcionando correctamente. Esto incluye verificar:

1. **Operadores instalados** - Los operadores de OpenShift AI deben estar activos
2. **Recursos del cluster** - Suficiente CPU, memoria y almacenamiento
3. **Acceso a GPU** - Si está disponible para modelos generativos
4. **Conectividad de red** - Para descargar modelos y datasets

**✅ Punto de Control:** Todos los componentes de OpenShift AI están disponibles y el cluster tiene recursos suficientes.

---

## 🏗️ Paso 1.2: Crear Proyecto de Ciencia de Datos

### Configuración del Proyecto

Un **Proyecto de Ciencia de Datos** en OpenShift AI proporciona:
- **Aislamiento de recursos** - Separación lógica entre proyectos
- **Gestión de permisos** - Control de acceso basado en roles
- **Organización de workloads** - Agrupación de notebooks, modelos y datos
- **Políticas de red** - Seguridad y comunicación entre servicios

### Detalles de Configuración

Para el workshop, crearemos un proyecto llamado `ecommerce-ai-workshop` que incluirá:
- **Configuración de cuotas** - Límites de recursos si los requiere el administrador
- **Políticas de red** - Reglas de comunicación entre servicios
- **Gestión de usuarios** - Permisos para miembros del equipo

El proyecto servirá como contenedor para todos los recursos del taller, incluyendo notebooks, modelos desplegados, y servicios de monitoreo.

**✅ Punto de Control:** El proyecto está creado y configurado correctamente.

---

## 💻 Paso 1.3: Configurar Jupyter Workbench

### Arquitectura del Workbench

**Jupyter Workbench** es el entorno principal de desarrollo que proporciona:
- **Jupyter Lab** - Interfaz web interactiva para desarrollo
- **Kernels preconfigurados** - Entornos Python con librerías de ML
- **Almacenamiento persistente** - Conservación de trabajo entre sesiones
- **Recursos escalables** - CPU, memoria y GPU según necesidades

### Configuración de Recursos

Para el workshop, configuraremos un workbench con:

| Recurso | Valor | Justificación |
|---------|-------|---------------|
| **CPU** | 4 cores | Procesamiento de datos y entrenamiento |
| **Memoria** | 8GB RAM | Carga de datasets y modelos |
| **Almacenamiento** | 20GB | Notebooks, datos y modelos exportados |
| **Imagen** | Standard Data Science | Librerías ML preinstaladas |

### Variables de Entorno

Se configurarán variables clave para el workshop:
- **PYTHONPATH** - Rutas de módulos Python
- **MODEL_ENDPOINT_URL** - URLs de servicios de modelos
- **WORKSHOP_PATH** - Directorio base del taller

**✅ Punto de Control:** Jupyter workbench está ejecutándose y es accesible.

---

## 📊 Paso 1.4: Preparar Materiales del Taller

### Descarga de Datasets

El notebook `1-environment/download_datasets.ipynb` realizará la descarga y validación de tres datasets principales:

#### **Dataset de Ventas Históricas**
- **Contenido:** Transacciones de ventas con patrones temporales
- **Tamaño:** ~50MB con 10,000+ registros
- **Características:** Fechas, productos, cantidades, precios, categorías
- **Uso:** Entrenamiento del modelo predictivo de pronóstico de ventas

#### **Catálogo de Productos**
- **Contenido:** Metadatos y categorías de productos
- **Tamaño:** ~5MB con 1,000+ productos
- **Características:** IDs, nombres, descripciones, categorías, precios
- **Uso:** Generación de descripciones y análisis de productos

#### **Datos de Comportamiento del Cliente**
- **Contenido:** Interacciones y datos de comportamiento de usuarios
- **Tamaño:** ~20MB con 5,000+ registros
- **Características:** Patrones de navegación, preferencias, historial
- **Uso:** Recomendaciones personalizadas y análisis de clientes

### Instalación de Dependencias

El notebook `1-environment/install_requirements.ipynb` instalará las librerías esenciales:

#### **Librerías de Machine Learning**
- **scikit-learn** - Algoritmos de aprendizaje automático
- **pandas/numpy** - Manipulación y análisis de datos
- **matplotlib/plotly** - Visualización de datos

#### **Librerías de Exportación y Despliegue**
- **onnx/skl2onnx** - Conversión de modelos a formato ONNX
- **onnxruntime** - Ejecución optimizada de modelos ONNX

#### **Librerías de Orquestación e Integración**
- **langchain** - Framework de orquestación de LLM
- **openai** - Cliente para APIs compatibles con OpenAI
- **requests** - Cliente HTTP para comunicación con APIs

#### **Librerías de Interface y Dashboard**
- **gradio** - Creación de interfaces web interactivas
- **streamlit** - Alternativa para dashboards
- **ipywidgets** - Widgets interactivos para Jupyter

### Validación del Entorno

El notebook `1-environment/verify_environment.ipynb` realizará verificaciones comprensivas:

#### **Verificación de Librerías**
- Importación exitosa de todas las dependencias
- Verificación de versiones compatibles
- Pruebas básicas de funcionalidad

#### **Verificación de Datasets**
- Integridad de archivos descargados
- Validación de schemas de datos
- Análisis de calidad de datos básico

#### **Verificación de Recursos**
- Disponibilidad de memoria RAM
- Espacio en disco disponible
- Acceso a CPU y GPU (si aplica)

**✅ Punto de Control:** Todas las librerías están instaladas y los datasets son accesibles.

---

## 🔧 Solución de Problemas Comunes

### Problema 1: Workbench No Inicia

**Síntomas:** El workbench se queda en estado "Starting" o "Error"

**Diagnóstico:**
El problema usualmente se debe a recursos insuficientes o problemas de conectividad. Es importante revisar los logs del pod y los eventos del namespace.

**Soluciones:**
- Verificar que hay recursos suficientes en el cluster
- Revisar políticas de pull de imágenes
- Validar claims de volúmenes persistentes
- Reiniciar el workbench si es necesario

### Problema 2: Fallos en Descarga de Datasets

**Síntomas:** Errores de red o descargas incompletas

**Causa Común:**
Los problemas de descarga pueden deberse a restricciones de red, proxies corporativos, o problemas temporales de conectividad.

**Enfoque de Solución:**
El notebook incluye funciones de verificación de conectividad y mecanismos de reintento. También proporciona checksums para validar la integridad de los datos descargados.

### Problema 3: Conflictos de Paquetes

**Síntomas:** Errores de resolución de dependencias o fallos en importaciones

**Estrategia de Resolución:**
El notebook utiliza un enfoque sistemático para la instalación, incluyendo limpieza de cache y reinstalación forzada cuando es necesario. También considera el uso de conda como alternativa a pip.

---

## 📝 Resumen de Configuración del Entorno

Después de completar este módulo, tu entorno tendrá:

### ✅ **Infraestructura Lista**
- Proyecto OpenShift AI: `ecommerce-ai-workshop`
- Jupyter workbench: `workshop-notebook` con recursos adecuados
- Almacenamiento persistente: 20GB montado y accesible

### ✅ **Datos Disponibles**
- Datos históricos de ventas: 10,000+ transacciones
- Catálogo de productos: 1,000+ productos con metadatos
- Comportamiento de clientes: 5,000+ registros de interacción

### ✅ **Entorno de Desarrollo**
- Todas las librerías Python requeridas instaladas
- Variables de entorno configuradas
- Datasets validados y accesibles

### ✅ **Verificación Completa**
- Recursos del sistema confirmados como adecuados
- Importaciones de librerías exitosas
- Listo para desarrollo de modelos

---

## 🚀 Próximos Pasos

¡Tu entorno está ahora listo para el desarrollo de modelos de IA! En el siguiente módulo aprenderás:

1. **Explorar el dataset de ventas** y entender patrones de negocio
2. **Ingeniar características** para modelado predictivo
3. **Entrenar un modelo Random Forest** para pronóstico de ventas
4. **Exportar el modelo a formato ONNX** para servicio optimizado
5. **Desplegar con OpenVINO** para inferencia de alto rendimiento

---

### 📁 Archivos Referenciados en Este Módulo
- `1-environment/download_datasets.ipynb`
- `1-environment/install_requirements.ipynb`
- `1-environment/verify_environment.ipynb`

---

### 📍 Navegación
[⬅️ Volver a la Guía Principal](../README.md) | [➡️ Siguiente: Modelo Predictivo](02-modelo-predictivo.md)

---

*¡Continúa al Módulo 2 para comenzar a construir tu primer modelo de IA para predicción de ventas!*