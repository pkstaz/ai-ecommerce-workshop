# OpenShift AI End-to-End Workshop - English Guide
## Intelligent E-commerce Recommendation and Analysis System

---

## 🎯 Workshop Overview

This comprehensive workshop guides you through building an intelligent e-commerce system using OpenShift AI. You'll deploy both predictive and generative AI models, integrate them using LangChain, and create an interactive dashboard for real-time analysis.

**Duration:** 4 hours  
**Format:** Hands-on workshop with step-by-step instructions  
**Target Audience:** ML Engineers, Data Scientists, DevOps Engineers

---

## 📋 Prerequisites Verification

Before starting, ensure you have:

### ✅ Platform Access
- [ ] OpenShift cluster with admin/developer access
- [ ] OpenShift AI operator installed and configured
- [ ] Access to OpenShift AI Dashboard
- [ ] Jupyter Hub access configured

### ✅ Technical Requirements
- [ ] Basic Python programming knowledge
- [ ] Understanding of machine learning concepts
- [ ] Familiarity with REST APIs
- [ ] Basic containerization knowledge

### ✅ Hardware Requirements
- [ ] Minimum 4 CPU cores available
- [ ] 8GB RAM for notebook instances
- [ ] 20GB persistent storage
- [ ] GPU access (recommended for generative model)

---

## 🗺️ Workshop Roadmap

| Phase | Module | Duration | Outcome |
|-------|--------|----------|---------|
| **Setup** | [Environment Setup](#module-1-environment-setup) | 30 min | Ready development environment |
| **Predictive** | [Sales Forecasting Model](#module-2-predictive-model) | 45 min | ONNX model deployed with OpenVINO |
| **Generative** | [Granite 3.1 8B Deployment](#module-3-generative-model) | 45 min | LLM serving with vLLM |
| **Integration** | [LangChain Orchestration](#module-4-langchain-integration) | 45 min | Unified AI system |
| **Operations** | [Monitoring & Optimization](#module-5-monitoring) | 30 min | Production-ready monitoring |
| **Advanced** | [MLOps Best Practices](#module-6-advanced-mlops) | 30 min | Enterprise deployment strategies |

---

## Module 1: Environment Setup
### 🛠️ Setting Up Your OpenShift AI Workspace

**Objective:** Configure a complete development environment for AI model deployment.

#### Step 1.1: Verify OpenShift AI Installation

1. **Access OpenShift AI Dashboard**
   ```bash
   # Check if OpenShift AI operator is installed
   oc get operators | grep openshiftai
   ```
   
2. **Verify Components**
   - Navigate to OpenShift AI Dashboard
   - Confirm these components are available:
     - Data Science Projects
     - Model Serving
     - Jupyter Hub
     - Model Registry

3. **Check Resource Availability**
   ```bash
   # Verify cluster resources
   oc describe nodes | grep -A 5 "Allocated resources"
   ```

#### Step 1.2: Create Data Science Project

1. **Project Creation**
   - Open OpenShift AI Dashboard
   - Click "Data Science Projects"
   - Create new project: `ecommerce-ai-workshop`
   - Add description: "End-to-end AI workshop for e-commerce"

2. **Configure Project Settings**
   - Set resource quotas (if required)
   - Configure network policies
   - Add team members (if applicable)

#### Step 1.3: Setup Jupyter Workbench

1. **Create Workbench**
   - In your project, click "Create workbench"
   - **Name:** `workshop-notebook`
   - **Image:** Standard Data Science Notebook
   - **Container size:** Medium (4 CPU, 8GB RAM)
   - **Storage:** 20GB PVC

2. **Configure Environment**
   - Add environment variables:
     ```
     PYTHONPATH=/opt/app-root/src
     MODEL_ENDPOINT_URL=http://model-server:8080
     ```

3. **Start and Access Workbench**
   - Click "Start" and wait for ready status
   - Click "Open" to access Jupyter

#### Step 1.4: Prepare Workshop Materials

1. **Download Datasets**
   - Execute `1-environment/download_datasets.ipynb`
   - This notebook downloads:
     - `sales_historical_data.csv` (10,000+ sales records)
     - `product_catalog.csv` (product metadata)
     - `customer_behavior.csv` (user interaction data)

2. **Install Required Libraries**
   - Run `1-environment/install_requirements.ipynb`
   - Installs: scikit-learn, onnx, langchain, gradio, and others

3. **Verify Installation**
   - Execute `1-environment/verify_environment.ipynb`
   - Confirms all dependencies are correctly installed

**📁 Code References:**
- `1-environment/download_datasets.ipynb`
- `1-environment/install_requirements.ipynb`
- `1-environment/verify_environment.ipynb`

---

## Module 2: Predictive Model
### 📊 Sales Forecasting with Random Forest, ONNX & OpenVINO

**Objective:** Build, export, and deploy a high-performance predictive model for sales forecasting.

#### Step 2.1: Understand the Business Case

**Problem Statement:**
Predict daily sales volume for e-commerce products based on:
- Historical sales patterns
- Product characteristics
- Seasonal trends
- Marketing campaigns

**Success Metrics:**
- Model accuracy > 85%
- Inference time < 100ms
- 99.9% endpoint availability

#### Step 2.2: Model Development

1. **Data Exploration and Preparation**
   - Execute `2-predictive-model/notebooks/01_data_exploration.ipynb`
   - This notebook:
     - Loads and examines the sales dataset
     - Performs feature engineering
     - Creates time-based features (day of week, month, seasonality)
     - Handles missing values and outliers

2. **Feature Engineering**
   - Run `2-predictive-model/notebooks/02_feature_engineering.ipynb`
   - Creates features:
     ```python
     # Key features created:
     # - temporal_features: day_of_week, month, quarter
     # - product_features: price_category, promotion_flag
     # - lag_features: sales_7d_avg, sales_30d_avg
     # - seasonal_features: holiday_flag, weekend_flag
     ```

3. **Model Training**
   - Execute `2-predictive-model/notebooks/03_train_model.ipynb`
   - Training process:
     - Split data (70% train, 15% validation, 15% test)
     - Train Random Forest with hyperparameter tuning
     - Evaluate model performance
     - Generate feature importance analysis

#### Step 2.3: ONNX Export Process

1. **Model Conversion**
   - Run `2-predictive-model/notebooks/04_export_onnx.ipynb`
   - Conversion steps:
     ```python
     # Key conversion process:
     # 1. Load trained scikit-learn model
     # 2. Define input schema
     # 3. Convert using skl2onnx
     # 4. Validate ONNX predictions match sklearn
     # 5. Save optimized ONNX model
     ```

2. **ONNX Validation**
   - Execute `2-predictive-model/notebooks/05_validate_onnx.ipynb`
   - Validation includes:
     - Prediction accuracy comparison
     - Performance benchmarking
     - Input/output schema verification

#### Step 2.4: OpenVINO Deployment

1. **Create Custom ServingRuntime**
   - Apply `2-predictive-model/deployment/serving-runtime.yaml`
   - This creates an OpenVINO-optimized runtime for ONNX models

2. **Deploy InferenceService**
   - Apply `2-predictive-model/deployment/inference-service.yaml`
   - Configuration includes:
     ```yaml
     # Key configuration elements:
     # - Model storage location
     # - Resource requests/limits
     # - Scaling configuration
     # - Health check endpoints
     ```

3. **Verify Deployment**
   - Run `2-predictive-model/deployment/test_deployment.ipynb`
   - Tests:
     - Endpoint connectivity
     - Prediction accuracy
     - Response time measurement

**📁 Code References:**
- `2-predictive-model/notebooks/01_data_exploration.ipynb`
- `2-predictive-model/notebooks/02_feature_engineering.ipynb`
- `2-predictive-model/notebooks/03_train_model.ipynb`
- `2-predictive-model/notebooks/04_export_onnx.ipynb`
- `2-predictive-model/notebooks/05_validate_onnx.ipynb`
- `2-predictive-model/deployment/serving-runtime.yaml`
- `2-predictive-model/deployment/inference-service.yaml`
- `2-predictive-model/deployment/test_deployment.ipynb`

---

## Module 3: Generative Model
### 🤖 Granite 3.1 8B Instruct with vLLM Serving

**Objective:** Deploy and serve a large language model for generating product descriptions and recommendations.

#### Step 3.1: Granite 3.1 8B Overview

**Model Capabilities:**
- Generate compelling product descriptions
- Create personalized recommendations
- Synthesize customer insights
- Provide conversational customer support

**Technical Specifications:**
- 8 billion parameters
- Instruction-tuned for enterprise use
- Optimized for reasoning tasks
- Red Hat supported and certified

#### Step 3.2: vLLM Configuration

1. **Understand vLLM Benefits**
   - High-throughput inference serving
   - Continuous batching optimization
   - GPU memory management
   - OpenAI-compatible API interface

2. **Resource Planning**
   - Execute `3-generative-model/notebooks/01_resource_planning.ipynb`
   - Calculates required:
     - GPU memory allocation
     - CPU resources
     - Storage requirements

#### Step 3.3: Model Deployment

1. **Prepare vLLM ServingRuntime**
   - Review `3-generative-model/deployment/serving-runtime.yaml`
   - Key configurations:
     ```yaml
     # Critical settings:
     # - GPU resource allocation
     # - Model loading parameters
     # - API endpoint configuration
     # - Health check definitions
     ```

2. **Deploy InferenceService**
   - Apply `3-generative-model/deployment/inference-service.yaml`
   - Monitor deployment:
     ```bash
     # Watch deployment status
     oc get inferenceservice granite-3-1-8b -w
     ```

3. **Verify Model Loading**
   - Execute `3-generative-model/deployment/verify_deployment.ipynb`
   - Checks:
     - Model loading completion
     - API endpoint responsiveness
     - Generation quality assessment

#### Step 3.4: API Testing and Optimization

1. **Basic API Testing**
   - Run `3-generative-model/notebooks/02_api_testing.ipynb`
   - Tests include:
     - Simple text generation
     - Parameter optimization
     - Response formatting

2. **Performance Benchmarking**
   - Execute `3-generative-model/notebooks/03_performance_benchmark.ipynb`
   - Measures:
     - Tokens per second
     - First token latency
     - Concurrent request handling

3. **Prompt Optimization**
   - Use `3-generative-model/notebooks/04_prompt_optimization.ipynb`
   - Develops:
     - Product description templates
     - Recommendation prompts
     - Customer support scenarios

**📁 Code References:**
- `3-generative-model/notebooks/01_resource_planning.ipynb`
- `3-generative-model/notebooks/02_api_testing.ipynb`
- `3-generative-model/notebooks/03_performance_benchmark.ipynb`
- `3-generative-model/notebooks/04_prompt_optimization.ipynb`
- `3-generative-model/deployment/serving-runtime.yaml`
- `3-generative-model/deployment/inference-service.yaml`
- `3-generative-model/deployment/verify_deployment.ipynb`

---

## Module 4: LangChain Integration
### 🔗 Orchestrating Multiple AI Models

**Objective:** Create a unified interface that combines predictive and generative models using LangChain.

#### Step 4.1: LangChain Architecture

**Integration Strategy:**
- Custom LLM wrapper for vLLM endpoint
- HTTP client for OpenVINO predictions
- Chain composition for complex workflows
- Memory management for conversations

#### Step 4.2: Setup LangChain Components

1. **Install and Configure LangChain**
   - Execute `4-langchain-integration/notebooks/01_setup_langchain.ipynb`
   - Configures:
     - Custom LLM wrapper
     - HTTP clients for model endpoints
     - Prompt templates
     - Memory components

2. **Create Model Wrappers**
   - Review `4-langchain-integration/notebooks/02_model_wrappers.ipynb`
   - Implements:
     ```python
     # Key wrapper classes:
     # - OpenVINOPredictor: Sales forecasting
     # - GraniteLLM: Text generation
     # - ModelOrchestrator: Combined workflows
     ```

#### Step 4.3: Build E-commerce AI Chains

1. **Product Analysis Chain**
   - Implement `4-langchain-integration/notebooks/03_product_analysis_chain.ipynb`
   - Workflow:
     ```
     Input Product → Sales Prediction → Description Generation → Recommendations
     ```

2. **Customer Insight Chain**
   - Create `4-langchain-integration/notebooks/04_customer_insight_chain.ipynb`
   - Capabilities:
     - Analyze customer behavior
     - Generate personalized recommendations
     - Predict purchase likelihood

3. **Market Analysis Chain**
   - Build `4-langchain-integration/notebooks/05_market_analysis_chain.ipynb`
   - Features:
     - Trend analysis
     - Competitive insights
     - Market opportunity identification

#### Step 4.4: Interactive Dashboard Development

1. **Create Dashboard Interface**
   - Execute `4-langchain-integration/notebooks/06_dashboard.ipynb`
   - Features:
     - Real-time model predictions
     - Interactive visualizations
     - Parameter adjustment controls
     - Results comparison tools

2. **Deploy Dashboard**
   - Run `4-langchain-integration/deployment/deploy_dashboard.ipynb`
   - Creates:
     - Gradio-based web interface
     - OpenShift route for external access
     - Authentication integration

3. **Test End-to-End Workflow**
   - Use `4-langchain-integration/notebooks/07_e2e_testing.ipynb`
   - Validates:
     - Complete user journey
     - Error handling
     - Performance under load

**📁 Code References:**
- `4-langchain-integration/notebooks/01_setup_langchain.ipynb`
- `4-langchain-integration/notebooks/02_model_wrappers.ipynb`
- `4-langchain-integration/notebooks/03_product_analysis_chain.ipynb`
- `4-langchain-integration/notebooks/04_customer_insight_chain.ipynb`
- `4-langchain-integration/notebooks/05_market_analysis_chain.ipynb`
- `4-langchain-integration/notebooks/06_dashboard.ipynb`
- `4-langchain-integration/notebooks/07_e2e_testing.ipynb`
- `4-langchain-integration/deployment/deploy_dashboard.ipynb`

---

## Module 5: Monitoring & Optimization
### 📈 Production-Ready Monitoring and Performance Optimization

**Objective:** Implement comprehensive monitoring and optimization strategies for production deployment.

#### Step 5.1: Model Performance Monitoring

1. **Setup OpenShift AI Model Monitoring**
   - Apply `5-monitoring-optimization/monitoring/model_monitoring_config.yaml`
   - Configures:
     - Model performance metrics
     - Data drift detection
     - Prediction accuracy tracking
     - Resource utilization monitoring

2. **Custom Metrics Collection**
   - Deploy `5-monitoring-optimization/monitoring/custom_metrics.ipynb`
   - Tracks:
     - Request/response patterns
     - Model prediction confidence
     - Business KPIs
     - User satisfaction metrics

#### Step 5.2: Alerting Configuration

1. **Setup Prometheus Alerts**
   - Apply `5-monitoring-optimization/monitoring/prometheus_alerts.yaml`
   - Alert conditions:
     - Model endpoint downtime
     - Prediction accuracy degradation
     - High response latency
     - Resource threshold breaches

2. **Configure Notification Channels**
   - Setup `5-monitoring-optimization/monitoring/notification_config.yaml`
   - Integrates with:
     - Slack notifications
     - Email alerts
     - PagerDuty integration

#### Step 5.3: Performance Optimization

1. **Model Quantization**
   - Execute `5-monitoring-optimization/optimization/model_quantization.ipynb`
   - Optimizations:
     - INT8 quantization for ONNX model
     - Memory usage reduction
     - Inference speed improvement

2. **Caching Strategy**
   - Implement `5-monitoring-optimization/optimization/caching_strategy.ipynb`
   - Features:
     - Redis-based response caching
     - Smart cache invalidation
     - Cache hit rate optimization

3. **Auto-scaling Configuration**
   - Apply `5-monitoring-optimization/optimization/autoscaling_config.yaml`
   - Scaling policies:
     - CPU and memory based scaling
     - Request rate based scaling
     - Custom metrics scaling

**📁 Code References:**
- `5-monitoring-optimization/monitoring/model_monitoring_config.yaml`
- `5-monitoring-optimization/monitoring/custom_metrics.ipynb`
- `5-monitoring-optimization/monitoring/prometheus_alerts.yaml`
- `5-monitoring-optimization/monitoring/notification_config.yaml`
- `5-monitoring-optimization/optimization/model_quantization.ipynb`
- `5-monitoring-optimization/optimization/caching_strategy.ipynb`
- `5-monitoring-optimization/optimization/autoscaling_config.yaml`

---

## Module 6: Advanced MLOps
### 🚀 Enterprise Production Deployment

**Objective:** Implement advanced MLOps practices for enterprise-grade deployments.

#### Step 6.1: Model Versioning and A/B Testing

1. **Model Version Management**
   - Setup `6-advanced-mlops/notebooks/model_versioning.ipynb`
   - Features:
     - Semantic versioning
     - Model registry integration
     - Rollback capabilities

2. **A/B Testing Framework**
   - Deploy `6-advanced-mlops/pipelines/ab_testing.yaml`
   - Enables:
     - Traffic splitting between model versions
     - Statistical significance testing
     - Automated performance comparison

#### Step 6.2: CI/CD Pipeline Integration

1. **GitOps Workflow**
   - Review `6-advanced-mlops/pipelines/gitops_pipeline.yaml`
   - Pipeline stages:
     - Automated model training
     - ONNX conversion and validation
     - Model deployment
     - Performance testing

2. **Quality Gates**
   - Implement `6-advanced-mlops/notebooks/quality_gates.ipynb`
   - Checks:
     - Model accuracy thresholds
     - Performance benchmarks
     - Security scanning
     - Compliance validation

#### Step 6.3: Security and Governance

1. **Security Best Practices**
   - Apply `6-advanced-mlops/security/security_policies.yaml`
   - Implements:
     - Network policies
     - RBAC configuration
     - Secret management
     - Audit logging

2. **Model Governance**
   - Setup `6-advanced-mlops/notebooks/model_governance.ipynb`
   - Features:
     - Model lineage tracking
     - Compliance reporting
     - Risk assessment
     - Documentation automation

**📁 Code References:**
- `6-advanced-mlops/notebooks/model_versioning.ipynb`
- `6-advanced-mlops/pipelines/ab_testing.yaml`
- `6-advanced-mlops/pipelines/gitops_pipeline.yaml`
- `6-advanced-mlops/notebooks/quality_gates.ipynb`
- `6-advanced-mlops/security/security_policies.yaml`
- `6-advanced-mlops/notebooks/model_governance.ipynb`

---

## 🔧 Troubleshooting Guide

### Common Issues and Solutions

#### 🚫 Model Deployment Failures

**Issue:** InferenceService not ready
```bash
# Diagnose the issue
oc describe inferenceservice <service-name>
oc logs deployment/<service-name>-predictor-default
```

**Common Solutions:**
- Verify resource requirements match cluster capacity
- Check image pull policies and registry access
- Validate model storage location and permissions

#### 🐛 ONNX Conversion Errors

**Issue:** Model conversion fails
```python
# Debug conversion process
python troubleshooting/debug_onnx_conversion.ipynb
```

**Common Solutions:**
- Verify scikit-learn version compatibility
- Check feature types and data preprocessing
- Validate model serialization format

#### 💾 vLLM Memory Issues

**Issue:** GPU out of memory
```bash
# Monitor GPU usage
nvidia-smi
oc describe pod <vllm-pod-name>
```

**Common Solutions:**
- Adjust model parameters (max_model_len, tensor_parallel_size)
- Increase GPU resources or use model sharding
- Optimize batch size and sequence length

#### 🔌 LangChain Connection Problems

**Issue:** API endpoints unreachable
```python
# Test connectivity
python troubleshooting/test_endpoints.ipynb
```

**Common Solutions:**
- Verify service discovery configuration
- Check network policies and firewall rules
- Validate authentication and authorization

---

## 📚 Additional Resources

### Documentation
- [OpenShift AI Documentation](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed)
- [KServe Documentation](https://kserve.github.io/website/)
- [OpenVINO Documentation](https://docs.openvino.ai/)
- [vLLM Documentation](https://docs.vllm.ai/)
- [LangChain Documentation](https://python.langchain.com/)

### Community
- [OpenShift AI Community](https://www.redhat.com/en/technologies/cloud-computing/openshift/openshift-ai)
- [CNCF Model Serving WG](https://github.com/cncf/tag-runtime/tree/main/wg-model-serving)

### Training
- [Red Hat Training - OpenShift AI](https://www.redhat.com/en/services/training)
- [Machine Learning Engineering Courses](https://www.redhat.com/en/services/training/all-courses-exams)

---

## 🎉 Workshop Completion

**Congratulations!** You have successfully:

- ✅ Deployed production-ready predictive models using ONNX and OpenVINO
- ✅ Configured high-performance generative AI serving with vLLM
- ✅ Integrated multiple models using LangChain framework
- ✅ Built interactive analytics dashboards
- ✅ Implemented monitoring and optimization strategies
- ✅ Explored advanced MLOps practices

### Next Steps

1. **Experiment with your own datasets** using the frameworks you've learned
2. **Explore additional OpenShift AI features** like distributed training
3. **Join the community** to share your experiences and learn from others
4. **Consider certification** in Red Hat OpenShift AI

---

**Contact Information:**
- **Instructor:** Carlos Estay
- **Email:** cestay@redhat.com
- **GitHub:** [pkstaz](https://github.com/pkstaz)

---

*Thank you for participating in this workshop! Your feedback is valuable for improving future sessions.*