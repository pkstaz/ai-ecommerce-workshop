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

Our sales dataset contains these key elements:

```python
# Dataset structure overview
Sales Data Features:
├── Temporal: date, day_of_week, month, quarter, holiday_flag
├── Product: product_id, category, price, promotion_flag
├── Customer: customer_segment, geography, purchase_history
└── Target: daily_sales_volume (what we want to predict)
```

---

## 🔍 Step 2.2: Data Exploration and Feature Engineering

### Data Exploration

1. **Execute Data Exploration Notebook**
   - Open and run: `2-predictive-model/notebooks/01_data_exploration.ipynb`
   
   This notebook performs:
   ```python
   # Key exploration steps
   # 1. Load and examine dataset structure
   # 2. Analyze sales patterns and seasonality
   # 3. Identify missing values and outliers
   # 4. Explore correlations between features
   # 5. Visualize temporal trends and distributions
   ```

2. **Key Insights Discovery**
   The notebook will reveal patterns such as:
   - **Seasonal Trends:** Holiday and weekend sales spikes
   - **Product Categories:** Electronics vs. clothing performance
   - **Price Sensitivity:** Impact of promotions on volume
   - **Geographic Patterns:** Regional sales variations

### Feature Engineering

1. **Create Enhanced Features**
   - Run: `2-predictive-model/notebooks/02_feature_engineering.ipynb`
   
   This creates sophisticated features:

   | Feature Type | Examples | Purpose |
   |--------------|----------|---------|
   | **Temporal** | `day_of_week`, `month`, `quarter` | Capture cyclical patterns |
   | **Lag Features** | `sales_7d_avg`, `sales_30d_avg` | Historical context |
   | **Product** | `price_category`, `promotion_flag` | Product characteristics |
   | **Seasonal** | `holiday_flag`, `weekend_flag` | Special periods |
   | **Interaction** | `price_x_promotion`, `category_x_season` | Combined effects |

2. **Feature Engineering Code Examples**
   ```python
   # Temporal features
   df['day_of_week'] = df['date'].dt.dayofweek
   df['month'] = df['date'].dt.month
   df['quarter'] = df['date'].dt.quarter
   
   # Lag features for trend analysis
   df['sales_7d_avg'] = df['sales_volume'].rolling(7).mean()
   df['sales_30d_avg'] = df['sales_volume'].rolling(30).mean()
   
   # Price categorization
   df['price_category'] = pd.cut(df['price'], 
                                bins=[0, 50, 200, 1000, float('inf')], 
                                labels=['Low', 'Medium', 'High', 'Premium'])
   ```

**✅ Checkpoint:** Features are engineered and dataset is ready for model training.

---

## 🤖 Step 2.3: Model Training and Validation

### Model Training Process

1. **Execute Model Training**
   - Run: `2-predictive-model/notebooks/03_train_model.ipynb`
   
   This notebook implements:

   ```python
   # Training pipeline overview
   # 1. Data splitting (70% train, 15% validation, 15% test)
   # 2. Feature scaling and preprocessing
   # 3. Hyperparameter tuning with cross-validation
   # 4. Model training with best parameters
   # 5. Performance evaluation and validation
   ```

2. **Model Configuration**
   ```python
   # Random Forest configuration
   from sklearn.ensemble import RandomForestRegressor
   from sklearn.model_selection import RandomizedSearchCV
   
   # Hyperparameter search space
   param_grid = {
       'n_estimators': [100, 200, 300, 500],
       'max_depth': [10, 20, 30, None],
       'min_samples_split': [2, 5, 10],
       'min_samples_leaf': [1, 2, 4],
       'bootstrap': [True, False]
   }
   ```

### Model Performance Evaluation

The training notebook will provide comprehensive evaluation:

| Metric | Description | Expected Range |
|--------|-------------|----------------|
| **R² Score** | Explained variance | > 0.85 |
| **MAE** | Mean Absolute Error | < 10% of mean sales |
| **RMSE** | Root Mean Square Error | < 15% of mean sales |
| **MAPE** | Mean Absolute Percentage Error | < 12% |

**✅ Checkpoint:** Model achieves target performance metrics and is ready for export.

---

## 📦 Step 2.4: ONNX Export Process

### Understanding ONNX Benefits

**ONNX (Open Neural Network Exchange)** provides:
- **Cross-platform compatibility** - Deploy on any system
- **Optimized inference** - Hardware-specific acceleration
- **Standardized format** - Interoperability between frameworks
- **Reduced dependencies** - Lighter deployment footprint

### Model Conversion

1. **Export to ONNX Format**
   - Execute: `2-predictive-model/notebooks/04_export_onnx.ipynb`
   
   This notebook handles:
   ```python
   # ONNX conversion process
   # 1. Load trained scikit-learn model
   # 2. Define input schema and data types
   # 3. Convert using skl2onnx library
   # 4. Validate ONNX predictions match sklearn
   # 5. Optimize model for inference
   # 6. Save ONNX model file
   ```

2. **Conversion Code Example**
   ```python
   from skl2onnx import to_onnx
   import onnx
   
   # Define input types
   initial_type = [('float_input', FloatTensorType([None, n_features]))]
   
   # Convert to ONNX
   onnx_model = to_onnx(rf_model, initial_type, 
                       target_opset=11,
                       options={id(rf_model): {'zipmap': False}})
   
   # Save optimized model
   onnx.save_model(onnx_model, 'sales_forecast_model.onnx')
   ```

### ONNX Validation

1. **Validate Conversion**
   - Run: `2-predictive-model/notebooks/05_validate_onnx.ipynb`
   
   This ensures:
   - ✅ **Prediction Accuracy:** ONNX predictions match sklearn exactly
   - ✅ **Performance Improvement:** Inference speed comparison
   - ✅ **Schema Validation:** Input/output format verification
   - ✅ **Model Integrity:** File size and structure validation

**✅ Checkpoint:** ONNX model exported and validated successfully.

---

## 🚀 Step 2.5: OpenVINO Deployment

### OpenVINO Serving Benefits

**OpenVINO** provides:
- **Intel CPU Optimization** - Leverage advanced CPU features
- **Reduced Latency** - Up to 10x faster inference
- **Lower Resource Usage** - Efficient memory utilization
- **Built-in Quantization** - Model compression support

### Deployment Configuration

1. **Create Custom ServingRuntime**
   - Apply: `2-predictive-model/deployment/serving-runtime.yaml`
   
   ```yaml
   apiVersion: serving.kserve.io/v1alpha1
   kind: ServingRuntime
   metadata:
     name: openvino-serving-runtime
   spec:
     supportedModelFormats:
       - name: onnx
         version: "1"
         autoSelect: true
     containers:
       - name: kserve-container
         image: openvino/model_server:latest
         args:
           - --model_name={{.Name}}
           - --model_path=/mnt/models
           - --port=8080
   ```

2. **Deploy InferenceService**
   - Apply: `2-predictive-model/deployment/inference-service.yaml`
   
   ```yaml
   apiVersion: serving.kserve.io/v1beta1
   kind: InferenceService
   metadata:
     name: sales-forecasting-model
   spec:
     predictor:
       model:
         modelFormat:
           name: onnx
         runtime: openvino-serving-runtime
         storageUri: "pvc://models/sales_forecast_model.onnx"
       resources:
         requests:
           cpu: 1
           memory: 2Gi
         limits:
           cpu: 2
           memory: 4Gi
   ```

### Deployment Verification

1. **Test Deployment**
   - Execute: `2-predictive-model/deployment/test_deployment.ipynb`
   
   This notebook verifies:

   ```python
   # Deployment testing checklist
   # 1. Check InferenceService status
   # 2. Test endpoint connectivity
   # 3. Validate prediction accuracy
   # 4. Measure response time
   # 5. Test error handling
   # 6. Performance benchmarking
   ```

2. **Monitoring Deployment Status**
   ```bash
   # Check deployment status
   oc get inferenceservice sales-forecasting-model
   
   # View detailed status
   oc describe inferenceservice sales-forecasting-model
   
   # Check pod logs
   oc logs -l serving.kserve.io/inferenceservice=sales-forecasting-model
   ```

**✅ Checkpoint:** Model is deployed and serving predictions via REST API.

---

## 🧪 Step 2.6: Model Testing and Performance Validation

### API Testing

The deployment test notebook performs comprehensive validation:

```python
# Example API test
import requests
import json

# Test data
test_features = {
    "day_of_week": 1,
    "month": 12,
    "price": 99.99,
    "promotion_flag": 1,
    "sales_7d_avg": 150.5
}

# Make prediction request
response = requests.post(
    endpoint_url,
    json={"instances": [test_features]},
    headers={"Content-Type": "application/json"}
)

prediction = response.json()["predictions"][0]
print(f"Predicted sales volume: {prediction}")
```

### Performance Benchmarks

Expected performance metrics:

| Metric | Target | Typical Result |
|--------|--------|----------------|
| **Response Time** | < 100ms | ~50-80ms |
| **Throughput** | > 100 RPS | ~200-500 RPS |
| **CPU Usage** | < 50% | ~30-40% |
| **Memory Usage** | < 2GB | ~1-1.5GB |

---

## 🔧 Troubleshooting Guide

### Common Issues and Solutions

#### Issue 1: ONNX Conversion Errors

**Symptoms:**
```
ValueError: Unable to convert model
```

**Solutions:**
```python
# Check sklearn version compatibility
print(sklearn.__version__)  # Should be >= 1.3.0

# Verify feature types
print(X_train.dtypes)  # All should be numeric

# Simplify model if needed
rf_model = RandomForestRegressor(n_estimators=100, max_depth=10)
```

#### Issue 2: InferenceService Not Ready

**Symptoms:**
```bash
oc get inferenceservice
# STATUS: False
```

**Diagnosis:**
```bash
# Check events
oc get events --sort-by=.metadata.creationTimestamp

# Check pod status
oc get pods -l serving.kserve.io/inferenceservice=sales-forecasting-model

# View logs
oc logs <pod-name>
```

**Solutions:**
- Verify model file exists in storage
- Check resource limits and node capacity
- Validate ONNX model format

#### Issue 3: High Latency

**Symptoms:** Response time > 200ms

**Solutions:**
```yaml
# Optimize resource allocation
resources:
  requests:
    cpu: 2      # Increase CPU
    memory: 4Gi
  limits:
    cpu: 4
    memory: 8Gi

# Add performance annotations
metadata:
  annotations:
    serving.kserve.io/enable-prometheus-scraping: "true"
    autoscaling.knative.dev/target: "10"
```

---

## 📊 Module Summary

### What You've Accomplished

✅ **Data Analysis Complete**
- Explored 10,000+ sales records
- Identified key patterns and trends
- Engineered meaningful features

✅ **Model Development Success**
- Trained Random Forest with >85% accuracy
- Implemented proper validation methodology
- Optimized hyperparameters

✅ **ONNX Export Mastery**
- Successfully converted sklearn to ONNX
- Validated prediction accuracy
- Reduced model deployment complexity

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

Your predictive model is now serving sales forecasts in production! In the next module, you'll:

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
[⬅️ Previous: Environment Setup](01-environment-setup.md) | [🏠 Main Guide](../README.md) | [➡️ Next: Generative Model](03-generative-model.md)

---

*Continue to Module 3 to add powerful text generation capabilities to your AI system!*