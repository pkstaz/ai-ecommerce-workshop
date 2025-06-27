# Módulo 2: Modelo Predictivo
## 📊 Pronóstico de Ventas con Random Forest, ONNX y OpenVINO

---

### 📍 Navegación
[⬅️ Anterior: Configuración del Entorno](01-configuracion-entorno.md) | [🏠 Guía Principal](../README.md) | [➡️ Siguiente: Modelo Generativo](03-modelo-generativo.md)

---

**Duración:** 45 minutos  
**Objetivo:** Construir, exportar y desplegar un modelo predictivo de alto rendimiento para pronóstico de ventas.

---

## 🎯 Resumen del Módulo

En este módulo aprenderás a:
- 🔍 **Analizar** datos de ventas y entender patrones de negocio
- ⚙️ **Ingeniar** características para machine learning
- 🤖 **Entrenar** un modelo Random Forest con ajuste de hiperparámetros
- 📦 **Exportar** el modelo a formato ONNX para optimización
- 🚀 **Desplegar** usando OpenVINO para inferencia de alto rendimiento
- ✅ **Validar** el despliegue y probar predicciones

---

## 💼 Paso 2.1: Entendiendo el Caso de Negocio

### Declaración del Problema

**Objetivo:** Predecir el volumen diario de ventas para productos de e-commerce para optimizar:

- **Gestión de Inventario** - Prevenir falta de stock y sobrestock
- **Planificación de Cadena de Suministro** - Optimizar procurement y logística
- **Campañas de Marketing** - Dirigir esfuerzos a productos de alto potencial
- **Pronóstico de Ingresos** - Habilitar planificación financiera precisa

### Métricas de Éxito

| Métrica | Objetivo | Impacto de Negocio |
|---------|----------|------------------- |
| **Precisión del Modelo** | > 85% | Predicciones confiables para planificación |
| **Tiempo de Inferencia** | < 100ms | Sistemas de recomendación en tiempo real |
| **Disponibilidad del Endpoint** | 99.9% | Confiabilidad del sistema en producción |
| **Tamaño del Modelo** | < 50MB | Despliegue eficiente y escalado |

### Comprensión de los Datos

Nuestro dataset de ventas contiene elementos clave organizados en categorías:

**Características Temporales:**
- Fechas, día de la semana, mes, trimestre
- Indicadores de días festivos y fines de semana
- Patrones estacionales y cíclicos

**Características de Productos:**
- IDs de productos, categorías, precios
- Flags de promociones y descuentos
- Atributos específicos por categoría

**Características de Clientes:**
- Segmentos de clientes, geografía
- Historial de compras y comportamiento
- Patrones de preferencias

---

## 🔍 Paso 2.2: Exploración de Datos e Ingeniería de Características

### Exploración de Datos

El notebook `2-predictive-model/notebooks/01_data_exploration.ipynb` realizará un análisis comprensivo de los datos:

#### **Análisis Exploratorio de Datos (EDA)**
- **Carga y examen** de la estructura del dataset
- **Análisis de distribuciones** de ventas y variables explicativas
- **Identificación de valores faltantes** y outliers
- **Exploración de correlaciones** entre características
- **Visualización de tendencias temporales** y patrones estacionales

#### **Insights Clave a Descubrir**
El análisis revelará patrones importantes como:
- **Tendencias Estacionales:** Picos de ventas en días festivos y fines de semana
- **Categorías de Productos:** Rendimiento diferencial entre electrónicos vs. ropa
- **Sensibilidad al Precio:** Impacto de promociones en el volumen
- **Patrones Geográficos:** Variaciones regionales en ventas

### Ingeniería de Características

El notebook `2-predictive-model/notebooks/02_feature_engineering.ipynb` creará características sofisticadas:

#### **Características Temporales**
- **Cíclicas:** Día de la semana, mes, trimestre para capturar patrones recurrentes
- **Lag Features:** Promedios móviles de 7 y 30 días para contexto histórico
- **Tendencias:** Pendientes de crecimiento y patrones temporales

#### **Características de Productos**
- **Categorización de precios:** Segmentación en bajo, medio, alto, premium
- **Flags de promoción:** Indicadores binarios de ofertas activas
- **Características de categoría:** Atributos específicos del tipo de producto

#### **Características de Interacción**
- **Cruce precio-promoción:** Efectos combinados de precio y descuentos
- **Categoría-estación:** Patrones estacionales específicos por tipo de producto
- **Cliente-producto:** Afinidades entre segmentos y categorías

#### **Procesamiento Avanzado**
El notebook implementará técnicas como:
- **Encoding de variables categóricas** usando one-hot y target encoding
- **Normalización y escalado** de variables numéricas
- **Manejo de valores atípicos** mediante técnicas estadísticas
- **Validación de calidad** de características generadas

**✅ Punto de Control:** Las características están ingenieradas y el dataset está listo para entrenamiento.

---

## 🤖 Paso 2.3: Entrenamiento y Validación del Modelo

### Proceso de Entrenamiento

El notebook `2-predictive-model/notebooks/03_train_model.ipynb` implementará un pipeline completo:

#### **Random Forest: ¿Por Qué Esta Elección?**
**Random Forest** es ideal para este caso de uso porque:
- **Manejo de características mixtas** - Numéricas y categóricas sin preprocessing extenso
- **Robustez a outliers** - Menos sensible a valores extremos que otros algoritmos
- **Importancia de características** - Proporciona insights sobre qué variables son más predictivas
- **No paramétrico** - No asume distribuciones específicas en los datos
- **Paralelizable** - Entrenamiento eficiente en múltiples cores

#### **Pipeline de Entrenamiento**
1. **División de datos** - 70% entrenamiento, 15% validación, 15% prueba
2. **Preprocesamiento** - Escalado y transformaciones finales
3. **Ajuste de hiperparámetros** - Grid search o random search
4. **Entrenamiento del modelo** - Con mejores parámetros encontrados
5. **Evaluación del rendimiento** - Métricas comprensivas

#### **Estrategia de Validación**
- **Validación cruzada temporal** - Respeta el orden cronológico de los datos
- **Métricas múltiples** - R², MAE, RMSE, MAPE para evaluación completa
- **Análisis de residuos** - Identificación de patrones en errores
- **Validación de generalización** - Rendimiento en datos no vistos

### Evaluación del Rendimiento

El modelo será evaluado usando métricas estándar:

| Métrica | Descripción | Rango Esperado |
|---------|-------------|----------------|
| **R² Score** | Varianza explicada | > 0.85 |
| **MAE** | Error Absoluto Medio | < 10% de la media de ventas |
| **RMSE** | Raíz del Error Cuadrático Medio | < 15% de la media de ventas |
| **MAPE** | Error Porcentual Absoluto Medio | < 12% |

**✅ Punto de Control:** El modelo alcanza las métricas objetivo y está listo para exportación.

---

## 📦 Paso 2.4: Proceso de Exportación ONNX

### Entendiendo los Beneficios de ONNX

**ONNX (Open Neural Network Exchange)** proporciona ventajas significativas:

#### **Compatibilidad Multiplataforma**
- **Interoperabilidad** - Funciona en diferentes frameworks y sistemas operativos
- **Estandarización** - Formato común para intercambio de modelos
- **Flexibilidad de despliegue** - Desde edge devices hasta servidores enterprise

#### **Optimización de Rendimiento**
- **Aceleración por hardware** - Aprovecha CPU, GPU y hardware especializado
- **Optimizaciones de grafo** - Fusión de operaciones y eliminación de redundancias
- **Cuantización** - Reducción de precisión para mayor velocidad

#### **Reducción de Dependencias**
- **Runtime ligero** - No requiere framework de entrenamiento original
- **Menor huella de memoria** - Modelos más eficientes en memoria
- **Despliegue simplificado** - Menos complejidad en producción

### Conversión del Modelo

El notebook `2-predictive-model/notebooks/04_export_onnx.ipynb` manejará la conversión:

#### **Proceso de Conversión**
1. **Carga del modelo entrenado** - Recuperación del modelo scikit-learn
2. **Definición de esquema de entrada** - Tipos y formas de datos de entrada
3. **Conversión usando skl2onnx** - Transformación al formato ONNX
4. **Validación de predicciones** - Verificación que ONNX coincide con sklearn
5. **Optimización del modelo** - Aplicación de optimizaciones ONNX
6. **Guardado del modelo** - Serialización del archivo .onnx

#### **Validaciones Críticas**
- **Equivalencia numérica** - Las predicciones ONNX deben ser idénticas
- **Validación de esquema** - Entrada y salida correctamente definidas
- **Pruebas de rendimiento** - Comparación de velocidad sklearn vs ONNX
- **Verificación de integridad** - El archivo ONNX está bien formado

### Validación ONNX

El notebook `2-predictive-model/notebooks/05_validate_onnx.ipynb` asegurará:

- ✅ **Precisión de Predicciones** - Coincidencia exacta con el modelo original
- ✅ **Mejora de Rendimiento** - Comparación de velocidad de inferencia
- ✅ **Validación de Esquema** - Verificación del formato de entrada/salida
- ✅ **Integridad del Modelo** - Validación del tamaño y estructura del archivo

**✅ Punto de Control:** El modelo ONNX está exportado y validado exitosamente.

---

## 🚀 Paso 2.5: Despliegue con OpenVINO

### Beneficios de OpenVINO

**OpenVINO (Open Visual Inference and Neural Network Optimization)** proporciona:

#### **Optimización para CPU Intel**
- **Aprovechamiento de instrucciones avanzadas** - AVX, SSE y extensiones especializadas
- **Optimización de memoria** - Gestión eficiente de cache y memoria
- **Paralelización inteligente** - Uso óptimo de múltiples cores

#### **Rendimiento Superior**
- **Latencia reducida** - Hasta 10x más rápido que inferencia nativa
- **Mayor throughput** - Procesamiento de más requests por segundo
- **Uso eficiente de recursos** - Menor consumo de CPU y memoria

#### **Características Enterprise**
- **Cuantización automática** - Reducción de precisión sin pérdida significativa
- **Batch processing** - Procesamiento eficiente de múltiples requests
- **Monitoreo integrado** - Métricas de rendimiento y salud

### Configuración del Despliegue

#### **ServingRuntime Personalizado**
El archivo `2-predictive-model/deployment/serving-runtime.yaml` define:
- **Imagen del contenedor** - OpenVINO Model Server optimizado
- **Configuración de recursos** - CPU, memoria y políticas de escalado
- **Parámetros del servidor** - Puertos, rutas de modelos y configuraciones
- **Health checks** - Endpoints para monitoreo de salud

#### **InferenceService**
El archivo `2-predictive-model/deployment/inference-service.yaml` especifica:
- **Referencia al modelo** - Ubicación del archivo ONNX
- **Runtime utilizado** - Conexión con el ServingRuntime de OpenVINO
- **Recursos asignados** - Límites y requests de CPU/memoria
- **Políticas de escalado** - Configuración de auto-scaling

### Verificación del Despliegue

El notebook `2-predictive-model/deployment/test_deployment.ipynb` realizará verificaciones comprensivas:

#### **Pruebas de Conectividad**
- **Estado del InferenceService** - Verificación que está "Ready"
- **Accesibilidad del endpoint** - Pruebas de conectividad HTTP
- **Tiempo de respuesta** - Medición de latencia de predicciones

#### **Validación Funcional**
- **Precisión de predicciones** - Comparación con resultados esperados
- **Manejo de errores** - Respuesta a inputs inválidos
- **Formato de respuesta** - Estructura correcta de outputs

#### **Benchmarking de Rendimiento**
- **Throughput** - Requests por segundo soportados
- **Latencia** - Tiempo de respuesta bajo carga
- **Utilización de recursos** - Uso de CPU y memoria durante operación

**✅ Punto de Control:** El modelo está desplegado y sirviendo predicciones vía API REST.

---

## 🧪 Paso 2.6: Pruebas y Validación de Rendimiento

### Pruebas de la API

El notebook de pruebas ejecutará escenarios comprensivos:

#### **Pruebas Funcionales**
- **Casos de prueba estándar** - Inputs típicos y outputs esperados
- **Casos límite** - Valores extremos y edge cases
- **Manejo de errores** - Respuestas a datos malformados o faltantes

#### **Pruebas de Carga**
- **Requests concurrentes** - Múltiples llamadas simultáneas
- **Sostenibilidad** - Rendimiento bajo carga prolongada
- **Recuperación** - Comportamiento después de picos de tráfico

### Métricas de Rendimiento Esperadas

| Métrica | Objetivo | Resultado Típico |
|---------|----------|------------------|
| **Tiempo de Respuesta** | < 100ms | ~50-80ms |
| **Throughput** | > 100 RPS | ~200-500 RPS |
| **Uso de CPU** | < 50% | ~30-40% |
| **Uso de Memoria** | < 2GB | ~1-1.5GB |

---

## 🔧 Guía de Solución de Problemas

### Errores Comunes de Conversión ONNX

**Problema:** Fallos en la conversión del modelo

**Causas Típicas:**
- **Incompatibilidad de versiones** entre scikit-learn y skl2onnx
- **Tipos de datos inconsistentes** en características
- **Características no soportadas** por el conversor ONNX

**Estrategias de Solución:**
El notebook incluye verificaciones de compatibilidad y alternativas para características problemáticas.

### Problemas de InferenceService

**Problema:** El servicio no alcanza estado "Ready"

**Diagnóstico Típico:**
- **Recursos insuficientes** en el cluster
- **Archivo de modelo inaccesible** o corrupto
- **Configuración incorrecta** del runtime

**Enfoque de Solución:**
Se proporcionan comandos de diagnóstico y pasos sistemáticos para identificar y resolver problemas de despliegue.

### Optimización de Latencia

**Problema:** Tiempo de respuesta mayor a 200ms

**Estrategias de Optimización:**
- **Ajuste de recursos** - Incremento de CPU asignada
- **Optimización de batch size** - Configuración para requests individuales
- **Tuning de OpenVINO** - Parámetros específicos del runtime

---

## 📊 Resumen del Módulo

### Lo Que Has Logrado

✅ **Análisis Completo de Datos**
- Explorado 10,000+ registros de ventas
- Identificado patrones clave y tendencias
- Ingenierado características significativas

✅ **Desarrollo Exitoso del Modelo**
- Entrenado Random Forest con >85% de precisión
- Implementado metodología de validación apropiada
- Optimizado hiperparámetros

✅ **Dominio de Exportación ONNX**
- Convertido exitosamente sklearn a ONNX
- Validado precisión de predicciones
- Reducido complejidad de despliegue

✅ **Despliegue en Producción**
- Desplegado con optimización OpenVINO
- Logrado tiempo de respuesta <100ms
- Implementado monitoreo y health checks

### Resultados Clave de Rendimiento

| Métrica | Logro |
|---------|-------|
| **Precisión del Modelo** | ~87% R² score |
| **Velocidad de Inferencia** | ~60ms promedio |
| **Tamaño del Modelo** | ~15MB archivo ONNX |
| **Tiempo de Despliegue** | ~3 minutos |

---

## 🚀 Próximos Pasos

¡Tu modelo predictivo está ahora sirviendo pronósticos de ventas en producción! En el siguiente módulo aprenderás:

1. **Desplegar Granite 3.1 8B** - Modelo de lenguaje grande para generación de texto
2. **Configurar servicio vLLM** - Inferencia de LLM de alto rendimiento
3. **Probar capacidades de generación** - Descripciones de productos y recomendaciones
4. **Optimizar ingeniería de prompts** - Ajustar outputs del modelo
5. **Realizar benchmark de rendimiento** - Medir throughput y latencia

¡La combinación de modelos predictivos y generativos creará un sistema de IA poderoso para inteligencia de e-commerce!

---

### 📁 Archivos Referenciados en Este Módulo
- `2-predictive-model/notebooks/01_data_exploration.ipynb`
- `2-predictive-model/notebooks/02_feature_engineering.ipynb`
- `2-predictive-model/notebooks/03_train_model.ipynb`
- `2-predictive-model/notebooks/04_export_onnx.ipynb`
- `2-predictive-model/notebooks/05_validate_onnx.ipynb`
- `2-predictive-model/deployment/serving-runtime.yaml`
- `2-predictive-model/deployment/inference-service.yaml`
- `2-predictive-model/deployment/test_deployment.ipynb`

---

### 📍 Navegación
[⬅️ Anterior: Configuración del Entorno](01-configuracion-entorno.md) | [🏠 Guía Principal](../README.md) | [➡️ Siguiente: Modelo Generativo](03-modelo-generativo.md)

---

*¡Continúa al Módulo 3 para agregar poderosas capacidades de generación de texto a tu sistema de IA!*