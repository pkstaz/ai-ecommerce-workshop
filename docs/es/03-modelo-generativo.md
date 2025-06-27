# Módulo 3: Modelo Generativo
## 🤖 Granite 3.1 8B Instruct con Servicio vLLM

---

### 📍 Navegación
[⬅️ Anterior: Modelo Predictivo](02-modelo-predictivo.md) | [🏠 Guía Principal](../README.md) | [➡️ Siguiente: Integración LangChain](04-integracion-langchain.md)

---

**Duración:** 45 minutos  
**Objetivo:** Desplegar y servir un modelo de lenguaje grande para generar descripciones de productos y recomendaciones.

---

## 🎯 Resumen del Módulo

En este módulo aprenderás a:
- 🔍 **Comprender** las capacidades y casos de uso de Granite 3.1 8B
- 📊 **Planificar** los requisitos de recursos para despliegue de LLM
- 🚀 **Desplegar** Granite 3.1 8B usando servicio vLLM
- ⚡ **Optimizar** rendimiento y throughput
- 🧪 **Probar** capacidades de generación de texto
- 📝 **Ingeniar** prompts para aplicaciones de e-commerce

---

## 🧠 Paso 3.1: Descripción de Granite 3.1 8B Instruct

### Capacidades del Modelo

**Granite 3.1 8B Instruct** es un modelo de lenguaje de grado empresarial diseñado para:

#### **Casos de Uso en E-commerce**
- **Descripciones de Productos** - Generar copy atractivo que aumenta conversiones
- **Recomendaciones Personalizadas** - Crear sugerencias dirigidas que mejoran engagement
- **Análisis de Clientes** - Sintetizar análisis de comportamiento para decisiones basadas en datos
- **Soporte Conversacional** - Servicio al cliente inteligente que reduce costos de soporte
- **Generación de Contenido** - Materiales de marketing y blogs que escalan la creación

#### **Ventajas Técnicas**
- **Arquitectura Transformer** - Tecnología de vanguardia para comprensión y generación
- **Ajuste por Instrucciones** - Entrenado específicamente para seguir instrucciones complejas
- **Contexto Extendido** - Capacidad de procesar hasta 4,096 tokens
- **Optimización de Razonamiento** - Excelente para tareas analíticas y de síntesis

### Especificaciones Técnicas

| Característica | Especificación | Beneficio |
|----------------|----------------|-----------|
| **Parámetros** | 8 mil millones | Balance entre capacidad y eficiencia |
| **Contexto Máximo** | 4,096 tokens | Procesamiento de documentos largos |
| **Precisión** | FP16/INT8 | Flexibilidad entre calidad y velocidad |
| **Memoria Base** | ~16GB (FP16) | Requisitos moderados de hardware |

### Beneficios Empresariales

✅ **Soporte Red Hat** - Soporte empresarial y certificación  
✅ **Optimizado para Razonamiento** - Excelente para tareas analíticas  
✅ **Alineado con Seguridad** - Outputs reducidos de contenido dañino  
✅ **Licencia Comercial** - Licenciamiento claro para uso empresarial  
✅ **Optimizado en Rendimiento** - Balance entre precisión y velocidad  

---

## 📊 Paso 3.2: Planificación de Recursos y Configuración vLLM

### Entendiendo las Ventajas de vLLM

**vLLM (Very Large Language Model serving)** proporciona características avanzadas:

#### **Tecnologías de Optimización**
- **Continuous Batching** - Procesa múltiples requests de manera eficiente
- **PagedAttention** - Gestión optimizada de memoria para atención
- **API Compatible con OpenAI** - Interfaz estándar para fácil integración
- **Aceleración GPU** - Optimización de hardware para 10x más velocidad

#### **Beneficios de Rendimiento**
| Característica | Beneficio | Impacto |
|----------------|-----------|---------|
| **Batching Continuo** | Procesa requests eficientemente | 2-3x mayor throughput |
| **PagedAttention** | Gestión optimizada de memoria | 2x más usuarios concurrentes |
| **GPU Acceleration** | Optimización de hardware | 10x más rápido que CPU |
| **API OpenAI** | Interfaz estándar | Integración simplificada |

### Planificación de Recursos

El notebook `3-generative-model/notebooks/01_resource_planning.ipynb` calculará requisitos específicos:

#### **Factores de Estimación de Recursos**
- **Tamaño del modelo y precisión** - FP16 vs INT8 afecta memoria requerida
- **Tamaño de batch y longitud de secuencia** - Impacta memoria y throughput
- **Requisitos de memoria GPU** - Cálculos específicos para hardware
- **CPU y memoria del sistema** - Recursos complementarios necesarios
- **Almacenamiento** - Espacio para archivos de modelo y cache

#### **Configuraciones Recomendadas**

| Configuración | Memoria GPU | CPU | RAM Sistema | Caso de Uso |
|---------------|-------------|-----|-------------|-------------|
| **Mínima** | 8GB | 4 cores | 16GB | Desarrollo/Pruebas |
| **Producción** | 16GB | 8 cores | 32GB | Cargas de trabajo productivas |
| **Alto Rendimiento** | 24GB+ | 16 cores | 64GB | Servicio de alto throughput |

#### **Evaluación de Compatibilidad GPU**
El notebook evaluará compatibilidad con GPUs disponibles:
- **NVIDIA A100** (40GB/80GB) - Óptima para todos los casos
- **NVIDIA V100** (32GB) - Buena para producción
- **NVIDIA RTX 4090** (24GB) - Buena para desarrollo avanzado
- **NVIDIA T4** (16GB) - Mínima con INT8

**✅ Punto de Control:** Los requisitos de recursos están calculados y la disponibilidad de GPU confirmada.

---

## 🚀 Paso 3.3: Despliegue del Modelo vLLM

### Preparar ServingRuntime vLLM

El archivo `3-generative-model/deployment/serving-runtime.yaml` define la configuración:

#### **Componentes Clave del Runtime**
- **Imagen de contenedor** - vLLM optimizado con OpenAI API
- **Argumentos de comando** - Parámetros específicos para Granite 3.1 8B
- **Variables de entorno** - Tokens de acceso y configuraciones
- **Recursos de GPU** - Asignación y límites de hardware

#### **Parámetros Críticos de Configuración**

| Parámetro | Valor | Propósito |
|-----------|-------|-----------|
| `tensor-parallel-size` | 1 | Número de GPUs para sharding del modelo |
| `gpu-memory-utilization` | 0.8 | Uso de memoria GPU (80%) |
| `max-model-len` | 4096 | Longitud máxima de secuencia |
| `served-model-name` | granite-3-1-8b | Identificador del modelo para API |

### Desplegar InferenceService

El archivo `3-generative-model/deployment/inference-service.yaml` especifica:

#### **Configuración del Servicio**
- **Referencia al modelo** - Ubicación en Hugging Face Hub
- **Runtime utilizado** - Conexión con vLLM ServingRuntime
- **Política de escalado** - Configuración min/max replicas
- **Recursos asignados** - GPU, CPU y memoria

#### **Monitoreo del Despliegue**
El proceso de despliegue incluye:
- **Descarga del modelo** - Puede tomar 5-10 minutos inicialmente
- **Inicialización vLLM** - Carga en memoria GPU
- **Validación de salud** - Verificación de endpoints
- **Estado de disponibilidad** - Confirmación "Ready"

### Verificación de Carga del Modelo

El notebook `3-generative-model/deployment/verify_deployment.ipynb` realizará verificaciones:

#### **Verificaciones de Despliegue**
- **Estado del InferenceService** - Confirmar estado "Ready"
- **Accesibilidad del endpoint API** - Pruebas de conectividad
- **Carga completa del modelo** - Validación que está en memoria
- **Prueba básica de generación** - Test de funcionalidad
- **Validación de formato de respuesta** - Estructura de outputs

**✅ Punto de Control:** Granite 3.1 8B está desplegado y listo para servir requests.

---

## 🧪 Paso 3.4: Pruebas de API y Benchmarking de Rendimiento

### Pruebas Básicas de API

El notebook `3-generative-model/notebooks/02_api_testing.ipynb` demostrará:

#### **Capacidades de Generación**
- **Generación básica de texto** - Pruebas de funcionalidad core
- **Respuesta a instrucciones** - Validación de capacidades de seguimiento
- **Generación condicionada** - Outputs basados en prompts específicos
- **Variabilidad creativa** - Ajuste de parámetros de temperatura

#### **Optimización de Parámetros de API**

| Parámetro | Rango | Efecto | Recomendación |
|-----------|-------|--------|---------------|
| `temperature` | 0.0-1.0 | Creatividad vs consistencia | 0.7 para descripciones |
| `top_p` | 0.1-1.0 | Diversidad de selección de tokens | 0.9 para calidad |
| `max_tokens` | 50-500 | Longitud de respuesta | 150-200 para productos |
| `frequency_penalty` | 0.0-2.0 | Reducción de repetición | 0.1-0.3 |

#### **Casos de Uso Específicos**
El notebook incluirá ejemplos para:
- **Descripciones de productos** - Copy atractivo y persuasivo
- **Recomendaciones personalizadas** - Sugerencias basadas en perfil
- **Análisis de mercado** - Síntesis de tendencias y datos
- **Contenido de marketing** - Materiales promocionales

### Benchmarking de Rendimiento

El notebook `3-generative-model/notebooks/03_performance_benchmark.ipynb` medirá:

#### **Métricas de Rendimiento Clave**

| Métrica | Descripción | Objetivo |
|---------|-------------|----------|
| **Latencia del Primer Token** | Tiempo hasta el primer token de respuesta | < 500ms |
| **Tokens por Segundo** | Throughput de generación | > 50 TPS |
| **Requests Concurrentes** | Capacidad de procesamiento paralelo | > 10 concurrentes |
| **Utilización GPU** | Eficiencia de hardware | 70-90% |

#### **Resultados Esperados de Benchmark**
- **Latencia del Primer Token:** 200-400ms
- **Velocidad de Generación:** 50-80 tokens/segundo
- **Usuarios Concurrentes:** 10-20 (dependiendo de GPU)
- **Uso de Memoria GPU:** 12-16GB

**✅ Punto de Control:** Los benchmarks de rendimiento cumplen con requisitos de producción.

---

## 📝 Paso 3.5: Ingeniería de Prompts para E-commerce

### Plantillas de Prompts Especializadas

El notebook `3-generative-model/notebooks/04_prompt_optimization.ipynb` creará plantillas para:

#### **Generación de Descripciones de Productos**
Las plantillas incluirán elementos estructurados para:
- **Información del producto** - Nombre, categoría, características clave
- **Rango de precios** - Contexto de posicionamiento
- **Audiencia objetivo** - Segmentación de clientes
- **Directrices de tono** - Profesional, casual, técnico, etc.

#### **Recomendaciones Personalizadas**
Prompts que incorporen:
- **Perfil del cliente** - Historial de compras y preferencias
- **Contexto de navegación** - Productos vistos y comportamiento
- **Restricciones de presupuesto** - Rangos de precio apropiados
- **Formato de output** - Estructura de recomendaciones

#### **Análisis de Mercado**
Plantillas para:
- **Análisis competitivo** - Comparación de productos y posicionamiento
- **Identificación de tendencias** - Patrones en datos de ventas
- **Oportunidades de crecimiento** - Gaps de mercado y nichos
- **Estrategias de pricing** - Recomendaciones de precios

### Técnicas Avanzadas de Prompting

#### **Few-Shot Learning**
Implementación de ejemplos para asegurar:
- **Consistencia de formato** - Outputs estructurados predeciblemente
- **Calidad de contenido** - Ejemplos de alta calidad como referencia
- **Adherencia a guidelines** - Seguimiento de estándares de marca

#### **Chain-of-Thought Prompting**
Para análisis complejos que requieren:
- **Razonamiento paso a paso** - Desglose de análisis complejos
- **Justificación transparente** - Explicación del proceso de pensamiento
- **Conclusiones fundamentadas** - Recomendaciones basadas en evidencia

#### **Prompt Templates Dinámicos**
Sistemas que permiten:
- **Personalización automática** - Ajuste basado en contexto
- **Escalabilidad** - Generación masiva con calidad consistente
- **A/B testing** - Comparación de diferentes enfoques de prompting

**✅ Punto de Control:** Las plantillas de prompts están optimizadas para casos de uso de e-commerce.

---

## 🔧 Guía de Solución de Problemas

### Timeout de Carga del Modelo

**Síntomas:** El modelo no se carga dentro del tiempo esperado

**Causas Comunes:**
- **Descarga lenta** - Conectividad limitada a Hugging Face Hub
- **Memoria insuficiente** - GPU no tiene capacidad para el modelo
- **Configuración incorrecta** - Parámetros de vLLM incompatibles

**Estrategias de Solución:**
El notebook incluye verificaciones de conectividad y alternativas de configuración para diferentes limitaciones de hardware.

### Problemas de Alta Latencia

**Síntomas:** Tiempo de respuesta > 2 segundos

**Factores Contribuyentes:**
- **Configuración subóptima** - Parámetros de vLLM no optimizados
- **Recursos limitados** - CPU o GPU insuficientes
- **Concurrencia alta** - Demasiados requests simultáneos

**Optimizaciones Disponibles:**
- **Ajuste de memoria GPU** - Incremento de utilización permitida
- **Optimización de batch** - Configuración de procesamiento por lotes
- **Tuning de secuencias** - Ajuste de longitudes máximas

### Errores de Memoria

**Síntomas:** `CUDA out of memory` u errores similares

**Soluciones Progresivas:**
- **Cuantización INT8** - Reducción de precisión del modelo
- **Reducción de contexto** - Longitudes de secuencia más cortas
- **Gestión conservadora** - Uso más conservador de memoria GPU

### Optimización de Velocidad de Generación

**Síntomas:** < 20 tokens por segundo

**Técnicas de Optimización:**
- **Parámetros de generación** - Ajuste de max_tokens y early stopping
- **Configuración de hardware** - Optimización de configuraciones vLLM
- **Prompt engineering** - Prompts más eficientes que generan menos tokens

---

## 📊 Resumen del Módulo

### Lo Que Has Logrado

✅ **Despliegue Exitoso del Modelo**
- Desplegado Granite 3.1 8B con servicio vLLM
- Configurado asignación óptima de recursos
- Logrado rendimiento listo para producción

✅ **Optimización de Rendimiento**
- Medido métricas clave de rendimiento
- Optimizado parámetros de generación
- Implementado manejo de requests concurrentes

✅ **Integración Lista para E-commerce**
- Desarrollado plantillas especializadas de prompts
- Creado generadores de descripciones de productos
- Construido sistemas de recomendaciones

✅ **Preparación para Producción**
- Implementado monitoreo y health checks
- Configurado políticas de auto-scaling
- Validado compatibilidad de API

### Resultados Clave de Rendimiento

| Métrica | Logro |
|---------|-------|
| **Tiempo de Despliegue** | ~8 minutos (incluyendo descarga del modelo) |
| **Latencia del Primer Token** | ~300ms promedio |
| **Velocidad de Generación** | ~65 tokens/segundo |
| **Capacidad Concurrente** | 15+ requests paralelos |
| **Eficiencia GPU** | 75-85% eficiente |

---

## 🚀 Próximos Pasos

¡Tu modelo de IA generativa está ahora sirviendo generación de texto de alta calidad! En el siguiente módulo aprenderás:

1. **Integrar con LangChain** - Framework unificado para orquestación de IA
2. **Conectar ambos modelos** - Combinar capacidades predictivas y generativas
3. **Construir chains de IA** - Crear workflows sofisticados
4. **Desarrollar dashboard** - Interfaz de usuario interactiva
5. **Probar extremo a extremo** - Validación completa del sistema

¡La integración permitirá casos de uso poderosos como predecir ventas Y generar explicaciones, o crear recomendaciones de productos con descripciones atractivas!

---

### 📁 Archivos Referenciados en Este Módulo
- `3-generative-model/notebooks/01_resource_planning.ipynb`
- `3-generative-model/notebooks/02_api_testing.ipynb`
- `3-generative-model/notebooks/03_performance_benchmark.ipynb`
- `3-generative-model/notebooks/04_prompt_optimization.ipynb`
- `3-generative-model/deployment/serving-runtime.yaml`
- `3-generative-model/deployment/inference-service.yaml`
- `3-generative-model/deployment/verify_deployment.ipynb`

---

### 📍 Navegación
[⬅️ Anterior: Modelo Predictivo](02-modelo-predictivo.md) | [🏠 Guía Principal](../README.md) | [➡️ Siguiente: Integración LangChain](04-integracion-langchain.md)

---

*¡Continúa al Módulo 4 para orquestar ambos modelos en un sistema de IA unificado!*