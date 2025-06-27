# Module 1: Environment Setup
## 🛠️ Setting Up Your OpenShift AI Workspace

---

### 📍 Navigation
[⬅️ Back to Main Guide](../README.md) | [➡️ Next: Predictive Model](02-predictive-model.md)

---

**Duration:** 30 minutes  
**Objective:** Configure a complete development environment for AI model deployment.

---

## 🎯 Module Overview

In this module, you will:
- ✅ Verify OpenShift AI installation and components
- ✅ Create and configure a Data Science Project
- ✅ Set up Jupyter Workbench with required resources
- ✅ Download datasets and install dependencies
- ✅ Validate the complete environment setup

---

## 🔍 Step 1.1: Verify OpenShift AI Installation

### Access OpenShift AI Dashboard

1. **Check OpenShift AI Operator Installation**
   ```bash
   # Verify if the OpenShift AI operator is installed
   oc get operators | grep openshiftai
   
   # Check operator status
   oc get csv -n openshift-operators | grep rhods
   ```

2. **Navigate to OpenShift AI Dashboard**
   - Open your OpenShift console
   - Look for "Red Hat OpenShift AI" in the application launcher (9-dot menu)
   - Click to access the dashboard

3. **Verify Available Components**
   Confirm these components are accessible:
   - **Data Science Projects** - Project management interface
   - **Model Serving** - KServe model deployment
   - **Jupyter Hub** - Interactive development environment
   - **Model Registry** - Model version management

### Resource Availability Check

```bash
# Check cluster resource availability
oc describe nodes | grep -A 5 "Allocated resources"

# Verify storage classes
oc get storageclass

# Check for GPU nodes (if available)
oc get nodes -l node.openshift.io/instance-type | grep gpu
```

**✅ Checkpoint:** You should see OpenShift AI components listed and cluster resources available.

---

## 🏗️ Step 1.2: Create Data Science Project

### Project Creation Process

1. **Access Data Science Projects**
   - In OpenShift AI Dashboard, click **"Data Science Projects"**
   - Click **"Create data science project"**

2. **Configure Project Details**
   - **Name:** `ecommerce-ai-workshop`
   - **Display Name:** `E-commerce AI Workshop`
   - **Description:** `End-to-end AI workshop for intelligent e-commerce system`

3. **Project Settings Configuration**
   ```yaml
   # Optional: Resource quotas (if cluster admin requires)
   apiVersion: v1
   kind: ResourceQuota
   metadata:
     name: workshop-quota
   spec:
     hard:
       requests.cpu: "8"
       requests.memory: 16Gi
       limits.cpu: "16"
       limits.memory: 32Gi
   ```

4. **Network and Security Setup**
   - Configure network policies (if required by cluster admin)
   - Set up team member access (if workshop has multiple participants)
   - Verify project permissions

**✅ Checkpoint:** Project `ecommerce-ai-workshop` should be created and visible in your dashboard.

---

## 💻 Step 1.3: Setup Jupyter Workbench

### Workbench Configuration

1. **Create New Workbench**
   - Inside your project, click **"Create workbench"**
   - Fill in the configuration:

   | Setting | Value |
   |---------|-------|
   | **Name** | `workshop-notebook` |
   | **Image selection** | Standard Data Science Notebook |
   | **Container size** | Medium (4 CPU, 8GB RAM) |
   | **Cluster storage** | Create new persistent storage |
   | **Storage size** | 20GB |

2. **Environment Variables Setup**
   Add these environment variables in the workbench configuration:
   ```bash
   PYTHONPATH=/opt/app-root/src
   MODEL_ENDPOINT_URL=http://model-server:8080
   WORKSHOP_PATH=/opt/app-root/src/ai-ecommerce-workshop
   ```

3. **Resource Configuration Details**
   ```yaml
   # Workbench resource specification
   resources:
     requests:
       cpu: "2"
       memory: "4Gi"
     limits:
       cpu: "4"
       memory: "8Gi"
   ```

### Launch and Access

1. **Start the Workbench**
   - Click **"Start"** and wait for the status to show **"Running"**
   - This may take 2-3 minutes for the first startup

2. **Access Jupyter Environment**
   - Click **"Open"** to launch Jupyter Lab
   - Verify you can access the Jupyter interface
   - Create a test notebook to confirm functionality

**✅ Checkpoint:** Jupyter workbench is running and accessible with proper resource allocation.

---

## 📊 Step 1.4: Prepare Workshop Materials

### Download Required Datasets

1. **Execute Dataset Download Notebook**
   - Open and run: `1-environment/download_datasets.ipynb`
   - This notebook will download:

   | Dataset | Description | Size | Records |
   |---------|-------------|------|---------|
   | `sales_historical_data.csv` | Sales transactions with temporal patterns | ~50MB | 10,000+ |
   | `product_catalog.csv` | Product metadata and categories | ~5MB | 1,000+ |
   | `customer_behavior.csv` | User interaction and behavioral data | ~20MB | 5,000+ |

2. **Dataset Validation**
   The notebook will perform these checks:
   ```python
   # Example validation checks performed
   # - File integrity verification
   # - Schema validation
   # - Data quality assessment
   # - Missing value analysis
   ```

### Install Required Dependencies

1. **Execute Requirements Installation**
   - Run: `1-environment/install_requirements.ipynb`
   - This installs essential libraries:

   | Library | Purpose | Version |
   |---------|---------|---------|
   | `scikit-learn` | Machine learning algorithms | >=1.3.0 |
   | `onnx` | Model export and conversion | >=1.14.0 |
   | `skl2onnx` | Scikit-learn to ONNX converter | >=1.15.0 |
   | `langchain` | LLM orchestration framework | >=0.1.0 |
   | `gradio` | Interactive dashboard creation | >=4.0.0 |
   | `requests` | HTTP client for API calls | >=2.31.0 |
   | `pandas` | Data manipulation and analysis | >=2.0.0 |
   | `numpy` | Numerical computing | >=1.24.0 |

2. **Additional ML Libraries**
   ```python
   # Additional packages for advanced features
   pip install prometheus-client  # Metrics collection
   pip install redis             # Caching strategies
   pip install plotly           # Advanced visualizations
   ```

### Environment Verification

1. **Run Verification Notebook**
   - Execute: `1-environment/verify_environment.ipynb`
   - This notebook validates:

   ✅ **Library Imports**
   ```python
   # Verification of key imports
   import sklearn
   import onnx
   import langchain
   import gradio as gr
   ```

   ✅ **Dataset Accessibility**
   ```python
   # Dataset loading verification
   sales_data = pd.read_csv('datasets/sales_historical_data.csv')
   product_data = pd.read_csv('datasets/product_catalog.csv')
   customer_data = pd.read_csv('datasets/customer_behavior.csv')
   ```

   ✅ **Resource Availability**
   ```python
   # System resource check
   import psutil
   print(f"Available RAM: {psutil.virtual_memory().available / 1024**3:.2f} GB")
   print(f"CPU cores: {psutil.cpu_count()}")
   ```

**✅ Checkpoint:** All libraries installed successfully and datasets are accessible.

---

## 🔧 Troubleshooting Common Issues

### Issue 1: Workbench Won't Start

**Symptoms:** Workbench status stuck in "Starting" or "Error"

**Diagnosis:**
```bash
# Check pod status
oc get pods -n <your-project-namespace>

# View pod logs
oc logs <workbench-pod-name> -n <your-project-namespace>

# Check events
oc get events -n <your-project-namespace> --sort-by=.metadata.creationTimestamp
```

**Solutions:**
- Verify sufficient cluster resources
- Check image pull policies and registry access
- Validate persistent volume claims
- Restart the workbench if needed

### Issue 2: Dataset Download Failures

**Symptoms:** Network errors or incomplete downloads

**Solutions:**
```python
# Manual download verification
import urllib.request
import hashlib

def verify_download(url, expected_hash=None):
    try:
        response = urllib.request.urlopen(url)
        print(f"✅ URL accessible: {url}")
        if expected_hash:
            # Implement hash verification
            pass
    except Exception as e:
        print(f"❌ Download failed: {e}")
```

### Issue 3: Package Installation Conflicts

**Symptoms:** Dependency resolution errors or import failures

**Solutions:**
```bash
# Clean pip cache
pip cache purge

# Force reinstall problematic packages
pip install --force-reinstall --no-cache-dir package_name

# Use conda if pip fails
conda install -c conda-forge package_name
```

---

## 📝 Environment Configuration Summary

After completing this module, your environment should have:

### ✅ **Infrastructure Ready**
- OpenShift AI project: `ecommerce-ai-workshop`
- Jupyter workbench: `workshop-notebook` (4 CPU, 8GB RAM)
- Persistent storage: 20GB mounted and accessible

### ✅ **Data Assets Available**
- Sales historical data: 10,000+ transactions
- Product catalog: 1,000+ products with metadata
- Customer behavior: 5,000+ interaction records

### ✅ **Development Environment**
- All required Python libraries installed
- Environment variables configured
- Datasets validated and accessible

### ✅ **Verification Complete**
- System resources confirmed adequate
- Library imports successful
- Ready for model development

---

## 🚀 Next Steps

Your environment is now ready for AI model development! In the next module, you'll:

1. **Explore the sales dataset** and understand business patterns
2. **Engineer features** for predictive modeling
3. **Train a Random Forest model** for sales forecasting
4. **Export the model to ONNX format** for optimized serving
5. **Deploy with OpenVINO** for high-performance inference

---

### 📁 Files Referenced in This Module
- `1-environment/download_datasets.ipynb`
- `1-environment/install_requirements.ipynb`
- `1-environment/verify_environment.ipynb`

---

### 📍 Navigation
[⬅️ Back to Main Guide](../README.md) | [➡️ Next: Predictive Model](02-predictive-model.md)

---

*Continue to Module 2 to start building your first AI model for sales prediction!*