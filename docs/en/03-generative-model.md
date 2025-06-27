# Module 3: Generative Model
## 🤖 Granite 3.1 8B Instruct with vLLM Serving

---

### 📍 Navigation
[⬅️ Previous: Predictive Model](02-predictive-model.md) | [🏠 Main Guide](README.md) | [➡️ Next: LangChain Integration](04-langchain-integration.md)

---

**Duration:** 45 minutes  
**Objective:** Deploy and serve a large language model for generating product descriptions and recommendations.

---

## 🎯 Module Overview

In this module, you will:
- 🔍 **Understand** Granite 3.1 8B capabilities and use cases
- 📊 **Plan** resource requirements for LLM deployment
- 🚀 **Deploy** Granite 3.1 8B using vLLM serving
- ⚡ **Optimize** performance and throughput
- 🧪 **Test** text generation capabilities
- 📝 **Engineer** prompts for e-commerce applications

---

## 🧠 Step 3.1: Granite 3.1 8B Instruct Overview

### Model Capabilities

**Granite 3.1 8B Instruct** is an enterprise-grade language model designed for:

| Capability | Use Case | Business Value |
|------------|----------|----------------|
| **Product Descriptions** | Generate compelling product copy | Increase conversion rates |
| **Personalized Recommendations** | Create targeted suggestions | Improve customer engagement |
| **Customer Insights** | Synthesize behavioral analysis | Data-driven decision making |
| **Conversational Support** | Intelligent customer service | Reduce support costs |
| **Content Generation** | Marketing materials and blogs | Scale content creation |

### Technical Specifications

```yaml
Model Details:
  Parameters: 8 billion
  Architecture: Transformer-based decoder
  Training: Instruction-tuned for enterprise use
  Context Length: 4,096 tokens
  Quantization: Supports INT8/FP16
  Memory Requirements: ~16GB for FP16, ~8GB for INT8
```

### Enterprise Benefits

- ✅ **Red Hat Supported** - Enterprise-grade support and certification
- ✅ **Reasoning Optimized** - Excellent for analytical tasks
- ✅ **Safety Aligned** - Reduced harmful outputs
- ✅ **Commercial License** - Clear licensing for business use
- ✅ **Performance Optimized** - Balanced accuracy and speed

---

## 📊 Step 3.2: Resource Planning and vLLM Configuration

### Understanding vLLM Advantages

**vLLM (Very Large Language Model serving)** provides:

| Feature | Benefit | Impact |
|---------|---------|--------|
| **Continuous Batching** | Process multiple requests efficiently | 2-3x higher throughput |
| **PagedAttention** | Optimized memory management | 2x more concurrent users |
| **OpenAI Compatible API** | Standard interface | Easy integration |
| **GPU Acceleration** | Hardware optimization | 10x faster than CPU |

### Resource Planning

1. **Execute Resource Planning Notebook**
   - Run: `3-generative-model/notebooks/01_resource_planning.ipynb`
   
   This notebook calculates:
   ```python
   # Resource estimation factors
   # 1. Model size and precision (FP16 vs INT8)
   # 2. Batch size and sequence length
   # 3. GPU memory requirements
   # 4. CPU and system memory needs
   # 5. Storage requirements for model files
   ```

2. **Recommended Configurations**

   | Configuration | GPU Memory | CPU | System RAM | Use Case |
   |---------------|------------|-----|------------|----------|
   | **Minimal** | 8GB | 4 cores | 16GB | Development/Testing |
   | **Production** | 16GB | 8 cores | 32GB | Production workloads |
   | **High-Performance** | 24GB+ | 16 cores | 64GB | High-throughput serving |

### GPU Requirements Assessment

```python
# GPU compatibility check
Compatible GPUs:
├── NVIDIA A100 (40GB/80GB) - Optimal
├── NVIDIA V100 (32GB) - Good
├── NVIDIA RTX 4090 (24GB) - Good
├── NVIDIA RTX 3090 (24GB) - Acceptable
└── NVIDIA T4 (16GB) - Minimal (with INT8)
```

**✅ Checkpoint:** Resource requirements are calculated and GPU availability confirmed.

---

## 🚀 Step 3.3: vLLM Model Deployment

### Prepare vLLM ServingRuntime

1. **Review ServingRuntime Configuration**
   - Examine: `3-generative-model/deployment/serving-runtime.yaml`
   
   ```yaml
   apiVersion: serving.kserve.io/v1alpha1
   kind: ServingRuntime
   metadata:
     name: vllm-granite-runtime
   spec:
     supportedModelFormats:
       - name: huggingface
         version: "1"
         autoSelect: true
     containers:
       - name: kserve-container
         image: vllm/vllm-openai:latest
         command: ["python", "-m", "vllm.entrypoints.openai.api_server"]
         args:
           - --model=ibm-granite/granite-3.1-8b-instruct
           - --tensor-parallel-size=1
           - --gpu-memory-utilization=0.8
           - --max-model-len=4096
           - --served-model-name=granite-3-1-8b
         env:
           - name: HUGGING_FACE_HUB_TOKEN
             valueFrom:
               secretKeyRef:
                 name: hf-token
                 key: token
         resources:
           requests:
             nvidia.com/gpu: 1
             cpu: 4
             memory: 16Gi
           limits:
             nvidia.com/gpu: 1
             cpu: 8
             memory: 32Gi
   ```

2. **Key Configuration Parameters**

   | Parameter | Value | Purpose |
   |-----------|-------|---------|
   | `tensor-parallel-size` | 1 | Number of GPUs for model sharding |
   | `gpu-memory-utilization` | 0.8 | GPU memory usage (80%) |
   | `max-model-len` | 4096 | Maximum sequence length |
   | `served-model-name` | granite-3-1-8b | API model identifier |

### Deploy InferenceService

1. **Deploy the Model**
   - Apply: `3-generative-model/deployment/inference-service.yaml`
   
   ```yaml
   apiVersion: serving.kserve.io/v1beta1
   kind: InferenceService
   metadata:
     name: granite-3-1-8b-instruct
     annotations:
       serving.kserve.io/enable-prometheus-scraping: "true"
   spec:
     predictor:
       model:
         modelFormat:
           name: huggingface
         runtime: vllm-granite-runtime
         args:
           - --model=ibm-granite/granite-3.1-8b-instruct
           - --trust-remote-code
       minReplicas: 1
       maxReplicas: 3
       resources:
         requests:
           nvidia.com/gpu: 1
           cpu: 4
           memory: 16Gi
         limits:
           nvidia.com/gpu: 1
           cpu: 8
           memory: 32Gi
   ```

2. **Monitor Deployment Progress**
   ```bash
   # Watch deployment status
   oc get inferenceservice granite-3-1-8b-instruct -w
   
   # Check detailed status
   oc describe inferenceservice granite-3-1-8b-instruct
   
   # Monitor pod startup (model loading takes 5-10 minutes)
   oc logs -f deployment/granite-3-1-8b-instruct-predictor
   ```

### Model Loading Verification

1. **Verify Model Loading**
   - Execute: `3-generative-model/deployment/verify_deployment.ipynb`
   
   This notebook checks:
   ```python
   # Deployment verification steps
   # 1. InferenceService status and readiness
   # 2. API endpoint accessibility
   # 3. Model loading completion
   # 4. Basic generation test
   # 5. Response format validation
   ```

**✅ Checkpoint:** Granite 3.1 8B is deployed and ready to serve requests.

---

## 🧪 Step 3.4: API Testing and Performance Benchmarking

### Basic API Testing

1. **Test Generation Capabilities**
   - Run: `3-generative-model/notebooks/02_api_testing.ipynb`
   
   This notebook demonstrates:
   ```python
   # API testing examples
   import openai
   
   # Configure client for vLLM endpoint
   client = openai.OpenAI(
       base_url="http://granite-3-1-8b-instruct.your-namespace.svc.cluster.local:8080/v1",
       api_key="not-used"
   )
   
   # Test basic generation
   response = client.completions.create(
       model="granite-3-1-8b",
       prompt="Generate a product description for a wireless bluetooth headphone:",
       max_tokens=150,
       temperature=0.7
   )
   ```

2. **API Parameter Optimization**

   | Parameter | Range | Effect | Recommendation |
   |-----------|-------|--------|----------------|
   | `temperature` | 0.0-1.0 | Creativity vs consistency | 0.7 for descriptions |
   | `top_p` | 0.1-1.0 | Token selection diversity | 0.9 for quality |
   | `max_tokens` | 50-500 | Response length | 150-200 for products |
   | `frequency_penalty` | 0.0-2.0 | Repetition reduction | 0.1-0.3 |

### Performance Benchmarking

1. **Measure Performance Metrics**
   - Execute: `3-generative-model/notebooks/03_performance_benchmark.ipynb`
   
   This notebook measures:

   | Metric | Description | Target |
   |--------|-------------|--------|
   | **First Token Latency** | Time to first response token | < 500ms |
   | **Tokens per Second** | Generation throughput | > 50 TPS |
   | **Concurrent Requests** | Parallel processing capacity | > 10 concurrent |
   | **GPU Utilization** | Hardware efficiency | 70-90% |

2. **Benchmark Results Example**
   ```python
   # Expected performance ranges
   Performance Metrics:
   ├── First Token Latency: 200-400ms
   ├── Generation Speed: 50-80 tokens/sec
   ├── Concurrent Users: 10-20 (depending on GPU)
   └── GPU Memory Usage: 12-16GB
   ```

**✅ Checkpoint:** Performance benchmarks meet production requirements.

---

## 📝 Step 3.5: Prompt Engineering for E-commerce

### E-commerce Prompt Templates

1. **Develop Specialized Prompts**
   - Use: `3-generative-model/notebooks/04_prompt_optimization.ipynb`
   
   This notebook creates templates for:

   **Product Description Generation:**
   ```python
   PRODUCT_DESCRIPTION_PROMPT = """
   Create a compelling product description for the following item:
   
   Product: {product_name}
   Category: {category}
   Key Features: {features}
   Price Range: {price_range}
   
   Write a 2-3 paragraph description that:
   - Highlights key benefits and features
   - Appeals to the target customer
   - Includes relevant keywords for SEO
   - Maintains a professional yet engaging tone
   
   Description:
   """
   ```

   **Personalized Recommendations:**
   ```python
   RECOMMENDATION_PROMPT = """
   Based on the customer profile below, recommend 3 products that would be perfect for them:
   
   Customer Profile:
   - Purchase History: {purchase_history}
   - Preferences: {preferences}
   - Budget Range: {budget}
   - Demographics: {demographics}
   
   For each recommendation, provide:
   1. Product name and brief description
   2. Why it fits their profile
   3. Key selling points
   
   Recommendations:
   """
   ```

### Advanced Prompt Techniques

2. **Implement Few-Shot Learning**
   ```python
   # Few-shot example for consistent formatting
   FEW_SHOT_PROMPT = """
   Generate product descriptions following these examples:
   
   Example 1:
   Product: Wireless Earbuds
   Description: Experience freedom with these premium wireless earbuds featuring noise cancellation and 24-hour battery life. Perfect for commuting, workouts, or relaxing at home.
   
   Example 2:
   Product: Smart Watch
   Description: Stay connected and healthy with this advanced smartwatch that tracks fitness, monitors heart rate, and delivers notifications right to your wrist.
   
   Now generate for:
   Product: {product_name}
   Description:
   """
   ```

3. **Chain-of-Thought Prompting**
   ```python
   COT_ANALYSIS_PROMPT = """
   Analyze this product's market potential step by step:
   
   Product: {product_name}
   Sales Data: {sales_metrics}
   
   Step 1: Market Position Analysis
   Step 2: Competitive Advantages
   Step 3: Target Customer Identification
   Step 4: Growth Potential Assessment
   Step 5: Strategic Recommendations
   
   Analysis:
   """
   ```

**✅ Checkpoint:** Prompt templates are optimized for e-commerce use cases.

---

## 🔧 Troubleshooting Guide

### Common Issues and Solutions

#### Issue 1: Model Loading Timeout

**Symptoms:**
```bash
oc get inferenceservice
# STATUS: False, READY: Unknown
```

**Diagnosis:**
```bash
# Check pod logs
oc logs -f deployment/granite-3-1-8b-instruct-predictor

# Common error messages:
# "CUDA out of memory" - Need more GPU memory
# "Model loading timeout" - Need more time or resources
```

**Solutions:**
```yaml
# Increase resources
resources:
  requests:
    nvidia.com/gpu: 1
    memory: 32Gi  # Increase memory
  limits:
    memory: 64Gi

# Add deployment annotations
metadata:
  annotations:
    serving.kserve.io/deployment-timeout: "600"  # 10 minutes
```

#### Issue 2: High Latency Issues

**Symptoms:** Response time > 2 seconds

**Solutions:**
```yaml
# Optimize vLLM parameters
args:
  - --gpu-memory-utilization=0.9  # Use more GPU memory
  - --max-num-batched-tokens=8192  # Increase batch size
  - --max-num-seqs=32  # More concurrent sequences
```

#### Issue 3: Out of Memory Errors

**Symptoms:**
```
RuntimeError: CUDA out of memory
```

**Solutions:**
```yaml
# Reduce model precision
args:
  - --quantization=int8  # Enable INT8 quantization
  - --max-model-len=2048  # Reduce max sequence length
  - --gpu-memory-utilization=0.7  # Conservative memory usage
```

#### Issue 4: Slow Generation Speed

**Symptoms:** < 20 tokens per second

**Optimization strategies:**
```python
# Optimal generation parameters
generation_params = {
    "max_tokens": 150,      # Don't generate more than needed
    "temperature": 0.7,     # Balance creativity and speed
    "top_p": 0.9,          # Efficient token selection
    "stop": ["\n\n", "---"] # Early stopping tokens
}
```

---

## 📊 Module Summary

### What You've Accomplished

✅ **Model Deployment Success**
- Deployed Granite 3.1 8B with vLLM serving
- Configured optimal resource allocation
- Achieved production-ready performance

✅ **Performance Optimization**
- Measured key performance metrics
- Optimized generation parameters
- Implemented concurrent request handling

✅ **E-commerce Integration Ready**
- Developed specialized prompt templates
- Created product description generators
- Built recommendation systems

✅ **Production Readiness**
- Implemented monitoring and health checks
- Configured auto-scaling policies
- Validated API compatibility

### Key Performance Results

| Metric | Achievement |
|--------|-------------|
| **Deployment Time** | ~8 minutes (including model download) |
| **First Token Latency** | ~300ms average |
| **Generation Speed** | ~65 tokens/second |
| **Concurrent Capacity** | 15+ parallel requests |
| **GPU Utilization** | 75-85% efficient |

---

## 🚀 Next Steps

Your generative AI model is now serving high-quality text generation! In the next module, you'll:

1. **Integrate with LangChain** - Unified framework for AI orchestration
2. **Connect both models** - Combine predictive and generative capabilities
3. **Build AI chains** - Create sophisticated workflows
4. **Develop dashboard** - Interactive user interface
5. **Test end-to-end** - Complete system validation

The integration will enable powerful use cases like predicting sales AND generating explanations, or creating product recommendations with compelling descriptions!

---

### 📁 Files Referenced in This Module
- `3-generative-model/notebooks/01_resource_planning.ipynb`
- `3-generative-model/notebooks/02_api_testing.ipynb`
- `3-generative-model/notebooks/03_performance_benchmark.ipynb`
- `3-generative-model/notebooks/04_prompt_optimization.ipynb`
- `3-generative-model/deployment/serving-runtime.yaml`
- `3-generative-model/deployment/inference-service.yaml`
- `3-generative-model/deployment/verify_deployment.ipynb`

---

### 📍 Navigation
[⬅️ Previous: Predictive Model](02-predictive-model.md) | [🏠 Main Guide](README.md) | [➡️ Next: LangChain Integration](04-langchain-integration.md)

---

*Continue to Module 4 to orchestrate both models into a unified AI system!*