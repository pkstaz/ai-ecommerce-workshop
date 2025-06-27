# Module 2: Predictive Model
## 📊 Sales Forecasting with Random Forest, ONNX & OpenVINO

---

### 📍 Navigation
[⬅️ Previous: Environment Setup](01-environment-setup.md) | [🏠 Main Guide](../README.md) | [➡️ Next: Generative Model](03-generative-model.md)

---

**Duration:** 45 minutes  
**Objective:** Build, export, and deploy a high-performance predictive model for sales forecasting.

---

## 🎯 Module Overview

In this module, you will:
- 🔍 **Analyze** sales data and understand business patterns
- ⚙️ **Engineer** features for machine learning
- 🤖 **Train** a Random Forest model with hyperparameter tuning
- 📦 **Export** the model to ONNX format for optimization
- 🚀 **Deploy** using OpenVINO for high-performance inference
- ✅ **Validate** deployment and test predictions

---

## 💼 Step 2.1: Understanding the Business Case

### Problem Statement

**Goal:** Predict daily sales volume for e-commerce products to optimize:

- **Inventory Management** - Prevent stockouts and overstocking
- **Supply Chain Planning** - Optimize procurement and logistics  
- **Marketing Campaigns** - Target high-potential products
- **Revenue Forecasting** - Enable accurate financial planning

### Success Metrics

| Metric | Target | Business Impact |
|--------|--------|-----------------|
| **Model Accuracy** | > 85% | Reliable predictions for planning |
| **Inference Time** | < 100ms | Real-time recommendation systems |
| **Endpoint Availability** | 99.9% | Production system reliability |
| **Model Size** | < 50MB | Efficient deployment and scaling |

### Data Understanding

Our sales dataset contains key elements organized in categories:

**Temporal Features:**
- Dates, day of week, month, quarter
- Holiday and weekend indicators
- Seasonal and cyclical patterns

**Product Features:**
- Product IDs, categories, prices
- Promotion and discount flags
- Category-specific attributes

**Customer Features:**
- Customer segments, geography
- Purchase history and behavior
- Preference patterns

---

## 🔍 Step 2.2: Data Exploration and Feature Engineering

### Data Exploration

The notebook `2-predictive-model/notebooks/01_data_exploration.ipynb` will perform comprehensive data analysis:

#### **Exploratory Data Analysis (EDA)**
- **Loading and examination** of dataset structure
- **Distribution analysis** of sales and explanatory variables
- **Missing value identification** and outliers
- **Correlation exploration** between features
- **Temporal trend visualization** and seasonal patterns

#### **Key Insights to Discover**
The analysis will reveal important patterns such as:
- **Seasonal Trends:** Holiday and weekend sales spikes
- **Product Categories:** Differential performance between electronics vs. clothing
- **Price Sensitivity:** Impact of promotions on volume
- **Geographic Patterns:** Regional sales variations

### Feature Engineering

The notebook `2-predictive-model/notebooks/02_feature_engineering.ipynb` will create sophisticated features:

#### **Temporal Features**
- **Cyclical:** Day of week, month, quarter to capture recurring patterns
- **Lag Features:** 7-day and 30-day moving averages for historical context
- **Trends:** Growth slopes and temporal patterns

#### **Product Features**
- **Price categorization:** Segmentation into low, medium, high, premium
- **Promotion flags:** Binary indicators of active offers
- **Category features:** Product type-specific attributes

#### **Interaction Features**
- **Price-promotion crossover:** Combined effects of price and discounts
- **Category-season:** Seasonal patterns specific to product type
- **Customer-product:** Affinities between segments and categories

#### **Advanced Processing**
The notebook will implement techniques such as:
- **Categorical variable encoding** using one-hot and target encoding
- **Normalization and scaling** of numerical variables
- **Outlier handling** through statistical techniques
- **Generated feature quality validation**

**✅ Checkpoint:** Features are engineered and dataset is ready for training.

---

## 🤖 Step 2.3: Model Training and Validation

### Training Process

The notebook `2-predictive-model/notebooks/03_train_model.ipynb` will implement a complete pipeline:

#### **Random Forest: Why This Choice?**
**Random Forest** is ideal for this use case because:
- **Mixed feature handling** - Numerical and categorical without extensive preprocessing
- **Robustness to outliers** - Less sensitive to extreme values than other algorithms
- **Feature importance** - Provides insights into which variables are most predictive
- **Non-parametric** - Doesn't assume specific distributions in data
- **Parallelizable** - Efficient training on multiple cores

#### **Training Pipeline**
1. **Data splitting** - 70% training, 15% validation, 15% test
2. **Preprocessing** - Scaling and final transformations
3. **Hyperparameter tuning** - Grid search or random search
4. **Model training** - With best parameters found
5. **Performance evaluation** - Comprehensive metrics

#### **Validation Strategy**
- **Temporal cross-validation** - Respects chronological order of data
- **Multiple metrics** - R², MAE, RMSE, MAPE for complete evaluation
- **Residual analysis** - Identification of error patterns
- **Generalization validation** - Performance on unseen data

### Performance Evaluation

The model will be evaluated using standard metrics:

| Metric | Description | Expected Range |
|---------|-------------|----------------|
| **R² Score** | Explained variance | > 0.85 |
| **MAE** | Mean Absolute Error | < 10% of mean sales |
| **RMSE** | Root Mean Square Error | < 15% of mean sales |
| **MAPE** | Mean Absolute Percentage Error | < 12% |

**✅ Checkpoint:** Model achieves target performance metrics and is ready for export.

---

## 📦 Step 2.4: ONNX Export Process

### Understanding ONNX Benefits

**ONNX (Open Neural Network Exchange)** provides significant advantages:

#### **Cross-platform Compatibility**
- **Interoperability** - Works across different frameworks and operating systems
- **Standardization** - Common format for model exchange
- **Deployment flexibility** - From edge devices to enterprise servers

#### **Performance Optimization**
- **Hardware acceleration** - Leverages CPU, GPU and specialized hardware
- **Graph optimizations** - Operation fusion and redundancy elimination
- **Quantization** - Precision reduction for increased speed

#### **Dependency Reduction**
- **Lightweight runtime** - Doesn't require original training framework
- **Smaller memory footprint** - More memory-efficient models
- **Simplified deployment** - Less complexity in production

### Model Conversion

The notebook `2-predictive-model/notebooks/04_export_onnx.ipynb` will handle conversion:

#### **Conversion Process**
1. **Load trained model** - Retrieve the scikit-learn model
2. **Define input schema** - Data types and shapes for entry
3. **Convert using skl2onnx** - Transform to ONNX format
4. **Validate predictions** - Verify ONNX matches sklearn
5. **Model optimization** - Apply ONNX optimizations
6. **Save model** - Serialize the .onnx file

#### **Critical Validations**
- **Numerical equivalence** - ONNX predictions must be identical
- **Schema validation** - Input and output correctly defined
- **Performance tests** - Speed comparison sklearn vs ONNX
- **Integrity verification** - ONNX file is well-formed

### ONNX Validation

The notebook `2-predictive-model/notebooks/05_validate_onnx.ipynb` will ensure:

- ✅ **Prediction Accuracy** - Exact match with original model
- ✅ **Performance Improvement** - Inference speed comparison
- ✅ **Schema Validation** - Input/output format verification
- ✅ **Model Integrity** - File size and structure validation

**✅ Checkpoint:** ONNX model is exported and validated successfully.

---

## 🚀 Step 2.5: OpenVINO Deployment

### OpenVINO Benefits

**OpenVINO (Open Visual Inference and Neural Network Optimization)** provides:

#### **Intel CPU Optimization**
- **Advanced instruction leverage** - AVX, SSE and specialized extensions
- **Memory optimization** - Efficient cache and memory management
- **Intelligent parallelization** - Optimal multi-core usage

#### **Superior Performance**
- **Reduced latency** - Up to 10x faster than native inference
- **Higher throughput** - Processing more requests per second
- **Efficient resource usage** - Lower CPU and memory consumption

#### **Enterprise Features**
- **Automatic quantization** - Precision reduction without significant loss
- **Batch processing** - Efficient handling of multiple requests
- **Integrated monitoring** - Performance and health metrics

### Deployment Configuration

#### **Custom ServingRuntime**
The file `2-predictive-model/deployment/serving-runtime.yaml` defines:
- **Container image** - OpenVINO Model Server optimized
- **Resource configuration** - CPU, memory and scaling policies
- **Server parameters** - Ports, model paths and configurations
- **Health checks** - Endpoints for health monitoring

#### **InferenceService**
The file `2-predictive-model/deployment/inference-service.yaml` specifies:
- **Model reference** - ONNX file location
- **Runtime used** - Connection with OpenVINO ServingRuntime
- **Allocated resources** - CPU/memory limits and requests
- **Scaling policies** - Auto-scaling configuration

### Deployment Verification

The notebook `2-predictive-model/deployment/test_deployment.ipynb` will perform comprehensive verifications:

#### **Connectivity Tests**
- **InferenceService status** - Verification it's "Ready"
- **Endpoint accessibility** - HTTP connectivity tests
- **Response time** - Prediction latency measurement

#### **Functional Validation**
- **Prediction accuracy** - Comparison with expected results
- **Error handling** - Response to invalid inputs
- **Response format** - Correct output structure

#### **Performance Benchmarking**
- **Throughput** - Requests per second supported
- **Latency** - Response time under load
- **Resource utilization** - CPU and memory usage during operation

**✅ Checkpoint:** Model is deployed and serving predictions via REST API.

---

## 🧪 Step 2.6: Testing and Performance Validation

### API Testing

The testing notebook will execute comprehensive scenarios:

#### **Functional Tests**
- **Standard test cases** - Typical inputs and expected outputs
- **Edge cases** - Extreme values and edge cases
- **Error handling** - Responses to malformed or missing data

#### **Load Tests**
- **Concurrent requests** - Multiple simultaneous calls
- **Sustainability** - Performance under prolonged load
- **Recovery** - Behavior after traffic spikes

### Expected Performance Metrics

| Metric | Target | Typical Result |
|---------|--------|----------------|
| **Response Time** | < 100ms | ~50-80ms |
| **Throughput** | > 100 RPS | ~200-500 RPS |
| **CPU Usage** | < 50% | ~30-40% |
| **Memory Usage** | < 2GB | ~1-1.5GB |

---

## 🔧 Troubleshooting Guide

### Common ONNX Conversion Errors

**Issue:** Model conversion failures

**Typical Causes:**
- **Version incompatibility** between scikit-learn and skl2onnx
- **Inconsistent data types** in features
- **Unsupported features** by ONNX converter

**Solution Strategies:**
The notebook includes compatibility checks and alternatives for problematic features.

### InferenceService Issues

**Issue:** Service doesn't reach "Ready" state

**Typical Diagnosis:**
- **Insufficient resources** in cluster
- **Inaccessible or corrupt model file**
- **Incorrect runtime configuration**

**Solution Approach:**
Diagnostic commands and systematic steps to identify and resolve deployment issues are provided.

### Latency Optimization

**Issue:** Response time greater than 200ms

**Optimization Strategies:**
- **Resource adjustment** - Increase allocated CPU
- **Batch size optimization** - Configuration for individual requests
- **OpenVINO tuning** - Runtime-specific parameters

---

## 📊 Module Summary

### What You've Accomplished

✅ **Complete Data Analysis**
- Explored 10,000+ sales records
- Identified key patterns and trends
- Engineered meaningful features

✅ **Successful Model Development**
- Trained Random Forest with >85% accuracy
- Implemented proper validation methodology
- Optimized hyperparameters

✅ **ONNX Export Mastery**
- Successfully converted sklearn to ONNX
- Validated prediction accuracy
- Reduced deployment complexity

✅ **Production Deployment**
- Deployed with OpenVINO optimization
- Achieved <100ms response time
- Implemented monitoring and health checks

### Key Performance Results

| Metric | Achievement |
|--------|-------------|
| **Model Accuracy** | ~87% R² score |
| **Inference Speed** | ~60ms average |
| **Model Size** | ~15MB ONNX file |
| **Deployment Time** | ~3 minutes |

---

## 🚀 Next Steps

Your predictive model is now serving sales forecasts in production! In the next module you'll learn:

1. **Deploy Granite 3.1 8B** - Large language model for text generation
2. **Configure vLLM serving** - High-performance LLM inference
3. **Test generation capabilities** - Product descriptions and recommendations
4. **Optimize prompt engineering** - Fine-tune model outputs
5. **Benchmark performance** - Measure throughput and latency

The combination of predictive and generative models will create a powerful AI system for e-commerce intelligence!

---

### 📁 Files Referenced in This Module
- `2-predictive-model/notebooks/01_data_exploration.ipynb`
- `2-predictive-model/notebooks/02_feature_engineering.ipynb`
- `2-predictive-model/notebooks/03_train_model.ipynb`
- `2-predictive-model/notebooks/04_export_onnx.ipynb`
- `2-predictive-model/notebooks/05_validate_onnx.ipynb`
- `2-predictive-model/deployment/serving-runtime.yaml`
- `2-predictive-model/deployment/inference-service.yaml`
- `2-predictive-model/deployment/test_deployment.ipynb`

---

### 📍 Navigation
[⬅️ Previous: Environment Setup](01-environment-setup.md) | [🏠 Main Guide](README.md) | [➡️ Next: Generative Model](03-generative-model.md)

---

*Continue to Module 3 to add powerful text generation capabilities to your AI system!*