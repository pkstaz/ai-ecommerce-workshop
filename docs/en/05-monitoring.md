# Module 5: Monitoring & Optimization
## 📈 Production-Ready Monitoring and Performance Optimization

---

### 📍 Navigation
[⬅️ Previous: LangChain Integration](04-langchain-integration.md) | [🏠 Main Guide](README.md) | [➡️ Next: Advanced MLOps](06-advanced-mlops.md)

---

**Duration:** 30 minutes  
**Objective:** Implement comprehensive monitoring and optimization strategies for production deployment.

---

## 🎯 Module Overview

In this module, you will:
- 📊 **Implement** OpenShift AI model monitoring
- 🔔 **Configure** alerting and notification systems
- 📈 **Track** custom metrics and KPIs
- ⚡ **Optimize** model performance and resource usage
- 🔄 **Setup** auto-scaling policies
- 🛡️ **Ensure** system reliability and availability

---

## 📊 Step 5.1: Model Performance Monitoring

### OpenShift AI Model Monitoring Setup

**Model Monitoring** provides comprehensive observability for:
- **Prediction Accuracy** - Track model performance over time
- **Data Drift Detection** - Identify when input data changes
- **Resource Utilization** - Monitor CPU, memory, and GPU usage
- **Request Patterns** - Analyze traffic and usage trends

### Configure Model Monitoring

1. **Setup Model Monitoring Configuration**
   - Apply: `5-monitoring-optimization/monitoring/model_monitoring_config.yaml`
   
   ```yaml
   apiVersion: datasciencecluster.opendatahub.io/v1
   kind: ModelMonitoring
   metadata:
     name: ecommerce-ai-monitoring
   spec:
     models:
       - name: sales-forecasting-model
         namespace: ecommerce-ai-workshop
         metrics:
           - accuracy
           - latency
           - throughput
           - data_drift
         alerting:
           enabled: true
           thresholds:
             accuracy: 0.80
             latency: 100
             data_drift: 0.15
       
       - name: granite-3-1-8b-instruct
         namespace: ecommerce-ai-workshop
         metrics:
           - generation_quality
           - token_throughput
           - gpu_utilization
           - response_time
         alerting:
           enabled: true
           thresholds:
             response_time: 2000
             gpu_utilization: 0.95
   ```

2. **Custom Metrics Collection**
   - Deploy: `5-monitoring-optimization/monitoring/custom_metrics.ipynb`
   
   This notebook implements:
   ```python
   # Custom metrics collection
   from prometheus_client import Counter, Histogram, Gauge
   
   # Business metrics
   prediction_requests = Counter('prediction_requests_total', 
                               'Total prediction requests', 
                               ['model', 'status'])
   
   prediction_accuracy = Gauge('model_prediction_accuracy',
                             'Current model accuracy score',
                             ['model_name'])
   
   response_time = Histogram('model_response_time_seconds',
                           'Model response time in seconds',
                           ['model', 'endpoint'])
   
   # E-commerce specific metrics
   sales_forecast_deviation = Gauge('sales_forecast_deviation',
                                  'Deviation from actual sales',
                                  ['product_category'])
   
   content_generation_quality = Gauge('content_quality_score',
                                    'Generated content quality score',
                                    ['content_type'])
   ```

### Monitoring Dashboard Integration

The monitoring setup provides:

| Metric Category | Examples | Business Impact |
|----------------|----------|-----------------|
| **Model Performance** | Accuracy, Precision, Recall | Model reliability |
| **System Performance** | Latency, Throughput, Errors | User experience |
| **Resource Usage** | CPU, Memory, GPU utilization | Cost optimization |
| **Business KPIs** | Prediction confidence, Content quality | ROI measurement |

**✅ Checkpoint:** Comprehensive monitoring is configured and collecting metrics.

---

## 🔔 Step 5.2: Alerting and Notification Systems

### Prometheus Alerting Rules

1. **Configure Prometheus Alerts**
   - Apply: `5-monitoring-optimization/monitoring/prometheus_alerts.yaml`
   
   ```yaml
   apiVersion: monitoring.coreos.com/v1
   kind: PrometheusRule
   metadata:
     name: ecommerce-ai-alerts
   spec:
     groups:
     - name: model_performance
       rules:
       - alert: ModelAccuracyDegraded
         expr: model_prediction_accuracy < 0.80
         for: 5m
         labels:
           severity: warning
           component: ml_model
         annotations:
           summary: "Model accuracy below threshold"
           description: "{{ $labels.model_name }} accuracy is {{ $value }}"
       
       - alert: HighModelLatency
         expr: histogram_quantile(0.95, model_response_time_seconds) > 0.1
         for: 2m
         labels:
           severity: critical
           component: inference
         annotations:
           summary: "Model response time too high"
           description: "95th percentile latency is {{ $value }}s"
       
     - name: resource_usage
       rules:
       - alert: GPUMemoryHigh
         expr: gpu_memory_utilization > 0.90
         for: 3m
         labels:
           severity: warning
           component: gpu
         annotations:
           summary: "GPU memory usage high"
           description: "GPU memory usage at {{ $value }}%"
       
       - alert: ModelEndpointDown
         expr: up{job="model-serving"} == 0
         for: 1m
         labels:
           severity: critical
           component: endpoint
         annotations:
           summary: "Model endpoint unreachable"
           description: "Model serving endpoint is down"
   ```

### Notification Configuration

1. **Setup Notification Channels**
   - Configure: `5-monitoring-optimization/monitoring/notification_config.yaml`
   
   ```yaml
   apiVersion: v1
   kind: Secret
   metadata:
     name: alertmanager-config
   data:
     alertmanager.yml: |
       global:
         slack_api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'
       
       route:
         group_by: ['alertname', 'component']
         group_wait: 10s
         group_interval: 10s
         repeat_interval: 1h
         receiver: 'default'
         routes:
         - match:
             severity: critical
           receiver: 'critical-alerts'
         - match:
             component: ml_model
           receiver: 'ml-team'
       
       receivers:
       - name: 'default'
         slack_configs:
         - channel: '#ai-operations'
           title: 'AI System Alert'
           text: '{{ range .Alerts }}{{ .Annotations.summary }}{{ end }}'
       
       - name: 'critical-alerts'
         slack_configs:
         - channel: '#critical-alerts'
           title: 'CRITICAL: AI System Issue'
           text: '{{ range .Alerts }}{{ .Annotations.description }}{{ end }}'
         email_configs:
         - to: 'ai-team@company.com'
           subject: 'Critical AI System Alert'
           body: '{{ range .Alerts }}{{ .Annotations.description }}{{ end }}'
       
       - name: 'ml-team'
         slack_configs:
         - channel: '#ml-engineering'
           title: 'Model Performance Alert'
           text: '{{ range .Alerts }}{{ .Annotations.summary }}{{ end }}'
   ```

**✅ Checkpoint:** Alerting system is configured with multiple notification channels.

---

## ⚡ Step 5.3: Performance Optimization

### Model Quantization

1. **Implement Model Quantization**
   - Execute: `5-monitoring-optimization/optimization/model_quantization.ipynb`
   
   This notebook performs:
   ```python
   # ONNX model quantization for OpenVINO
   from onnx import optimization
   import onnxruntime as ort
   
   def quantize_onnx_model(model_path, quantized_path):
       """
       Quantize ONNX model to INT8 for faster inference
       """
       # Dynamic quantization
       ort.quantization.quantize_dynamic(
           model_input=model_path,
           model_output=quantized_path,
           weight_type=ort.quantization.QuantType.QInt8
       )
       
       # Validate quantized model
       validate_quantized_performance(quantized_path)
       
       return quantized_path
   
   # Granite model optimization
   def optimize_granite_serving():
       """
       Optimize vLLM serving parameters
       """
       optimized_config = {
           "gpu_memory_utilization": 0.9,
           "max_num_batched_tokens": 8192,
           "max_num_seqs": 32,
           "quantization": "int8",  # Enable INT8 quantization
           "tensor_parallel_size": 1
       }
       return optimized_config
   ```

### Caching Strategy Implementation

1. **Deploy Intelligent Caching**
   - Implement: `5-monitoring-optimization/optimization/caching_strategy.ipynb`
   
   ```python
   import redis
   import hashlib
   import json
   
   class IntelligentCache:
       def __init__(self, redis_host='localhost', redis_port=6379):
           self.redis_client = redis.Redis(host=redis_host, port=redis_port)
           self.default_ttl = 3600  # 1 hour
       
       def get_cache_key(self, model_name, input_data):
           """Generate consistent cache key"""
           data_str = json.dumps(input_data, sort_keys=True)
           hash_obj = hashlib.md5(data_str.encode())
           return f"{model_name}:{hash_obj.hexdigest()}"
       
       def get_prediction(self, model_name, input_data):
           """Get cached prediction or None"""
           cache_key = self.get_cache_key(model_name, input_data)
           cached_result = self.redis_client.get(cache_key)
           
           if cached_result:
               self.redis_client.incr(f"cache_hits:{model_name}")
               return json.loads(cached_result)
           
           self.redis_client.incr(f"cache_misses:{model_name}")
           return None
       
       def cache_prediction(self, model_name, input_data, prediction, ttl=None):
           """Cache prediction result"""
           cache_key = self.get_cache_key(model_name, input_data)
           ttl = ttl or self.default_ttl
           
           self.redis_client.setex(
               cache_key,
               ttl,
               json.dumps(prediction)
           )
       
       def get_cache_stats(self, model_name):
           """Get cache performance statistics"""
           hits = int(self.redis_client.get(f"cache_hits:{model_name}") or 0)
           misses = int(self.redis_client.get(f"cache_misses:{model_name}") or 0)
           
           if hits + misses == 0:
               return {"hit_rate": 0, "total_requests": 0}
           
           hit_rate = hits / (hits + misses)
           return {
               "hit_rate": hit_rate,
               "cache_hits": hits,
               "cache_misses": misses,
               "total_requests": hits + misses
           }
   ```

### Auto-scaling Configuration

1. **Configure Horizontal Pod Autoscaler**
   - Apply: `5-monitoring-optimization/optimization/autoscaling_config.yaml`
   
   ```yaml
   # Sales forecasting model HPA
   apiVersion: autoscaling/v2
   kind: HorizontalPodAutoscaler
   metadata:
     name: sales-model-hpa
   spec:
     scaleTargetRef:
       apiVersion: serving.kserve.io/v1beta1
       kind: InferenceService
       name: sales-forecasting-model
     minReplicas: 1
     maxReplicas: 10
     metrics:
     - type: Resource
       resource:
         name: cpu
         target:
           type: Utilization
           averageUtilization: 70
     - type: Resource
       resource:
         name: memory
         target:
           type: Utilization
           averageUtilization: 80
     - type: Pods
       pods:
         metric:
           name: requests_per_second
         target:
           type: AverageValue
           averageValue: "100"
     behavior:
       scaleUp:
         stabilizationWindowSeconds: 60
         policies:
         - type: Percent
           value: 50
           periodSeconds: 60
       scaleDown:
         stabilizationWindowSeconds: 300
         policies:
         - type: Percent
           value: 25
           periodSeconds: 60
   
   ---
   # Granite model HPA (GPU-aware)
   apiVersion: autoscaling/v2
   kind: HorizontalPodAutoscaler
   metadata:
     name: granite-model-hpa
   spec:
     scaleTargetRef:
       apiVersion: serving.kserve.io/v1beta1
       kind: InferenceService
       name: granite-3-1-8b-instruct
     minReplicas: 1
     maxReplicas: 3  # Limited by GPU availability
     metrics:
     - type: Resource
       resource:
         name: nvidia.com/gpu
         target:
           type: Utilization
           averageUtilization: 80
     - type: Pods
       pods:
         metric:
           name: queue_depth
         target:
           type: AverageValue
           averageValue: "5"
   ```

**✅ Checkpoint:** Performance optimizations are implemented and auto-scaling is configured.

---

## 📊 Step 5.4: System Health and Reliability

### Health Check Implementation

```python
# Comprehensive health monitoring
class SystemHealthMonitor:
    def __init__(self):
        self.health_checks = {
            'openvino_model': self.check_openvino_health,
            'granite_model': self.check_granite_health,
            'dashboard': self.check_dashboard_health,
            'cache': self.check_cache_health,
            'database': self.check_database_health
        }
    
    def check_openvino_health(self):
        """Check OpenVINO model health"""
        try:
            # Test prediction request
            test_data = self.get_test_data()
            response = requests.post(
                OPENVINO_ENDPOINT,
                json=test_data,
                timeout=5
            )
            
            if response.status_code == 200:
                return {"status": "healthy", "latency": response.elapsed.total_seconds()}
            else:
                return {"status": "unhealthy", "error": f"HTTP {response.status_code}"}
        
        except Exception as e:
            return {"status": "unhealthy", "error": str(e)}
    
    def check_granite_health(self):
        """Check Granite model health"""
        try:
            # Test generation request
            client = openai.OpenAI(base_url=GRANITE_ENDPOINT)
            response = client.completions.create(
                model="granite-3-1-8b",
                prompt="Test prompt",
                max_tokens=10,
                timeout=10
            )
            
            return {"status": "healthy", "response_length": len(response.choices[0].text)}
        
        except Exception as e:
            return {"status": "unhealthy", "error": str(e)}
    
    def get_system_health(self):
        """Get overall system health status"""
        health_results = {}
        overall_status = "healthy"
        
        for component, check_fn in self.health_checks.items():
            result = check_fn()
            health_results[component] = result
            
            if result["status"] != "healthy":
                overall_status = "degraded"
        
        return {
            "overall_status": overall_status,
            "components": health_results,
            "timestamp": datetime.now().isoformat()
        }
```

### Performance Benchmarking

Expected performance improvements after optimization:

| Metric | Before Optimization | After Optimization | Improvement |
|--------|-------------------|-------------------|-------------|
| **OpenVINO Latency** | ~80ms | ~45ms | 44% faster |
| **Granite Throughput** | ~45 TPS | ~75 TPS | 67% increase |
| **Cache Hit Rate** | 0% | 65-80% | Response time reduction |
| **Resource Usage** | High | Optimized | 30% cost reduction |

---

## 🔧 Troubleshooting Guide

### Common Monitoring Issues

#### Issue 1: Missing Metrics

**Symptoms:** Prometheus not collecting model metrics

**Solutions:**
```yaml
# Add service monitor
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: model-metrics
spec:
  selector:
    matchLabels:
      app: inference-service
  endpoints:
  - port: metrics
    path: /metrics
    interval: 30s
```

#### Issue 2: Alert Fatigue

**Symptoms:** Too many false positive alerts

**Solutions:**
```yaml
# Adjust alert thresholds
- alert: ModelLatencyHigh
  expr: histogram_quantile(0.95, model_response_time_seconds) > 0.2  # Increased threshold
  for: 5m  # Increased duration
```

#### Issue 3: Cache Performance Issues

**Symptoms:** Low cache hit rates

**Solutions:**
```python
# Optimize cache strategy
def smart_cache_ttl(prediction_confidence):
    """Adjust TTL based on prediction confidence"""
    if prediction_confidence > 0.9:
        return 3600  # 1 hour for high confidence
    elif prediction_confidence > 0.7:
        return 1800  # 30 minutes for medium confidence
    else:
        return 600   # 10 minutes for low confidence
```

---

## 📊 Module Summary

### What You've Accomplished

✅ **Comprehensive Monitoring**
- Model performance tracking
- System health monitoring
- Custom business metrics
- Real-time alerting

✅ **Performance Optimization**
- Model quantization (30-50% speedup)
- Intelligent caching (65-80% hit rate)
- Resource optimization
- Auto-scaling policies

✅ **Production Reliability**
- Health check endpoints
- Alert management
- Error recovery mechanisms
- Performance benchmarking

✅ **Operational Excellence**
- Prometheus metrics integration
- Slack/email notifications
- Dashboard visualization
- Cost optimization

### Key Performance Gains

| Optimization | Impact | Business Benefit |
|-------------|--------|------------------|
| **Model Quantization** | 44% faster inference | Improved user experience |
| **Intelligent Caching** | 70% cache hit rate | Reduced compute costs |
| **Auto-scaling** | Dynamic resource allocation | Cost efficiency |
| **Monitoring** | 99.9% uptime target | Reliable service |

---

## 🚀 Next Steps

Your AI system is now optimized and production-ready! In the final module, you'll explore:

1. **Advanced MLOps Practices** - Model versioning and A/B testing
2. **CI/CD Pipeline Integration** - Automated deployment workflows
3. **Security Best Practices** - Secure AI model serving
4. **Governance Framework** - Model compliance and auditing
5. **Enterprise Scaling** - Multi-environment deployment

These advanced practices will prepare your AI system for enterprise-scale production deployment!

---

### 📁 Files Referenced in This Module
- `5-monitoring-optimization/monitoring/model_monitoring_config.yaml`
- `5-monitoring-optimization/monitoring/custom_metrics.ipynb`
- `5-monitoring-optimization/monitoring/prometheus_alerts.yaml`
- `5-monitoring-optimization/monitoring/notification_config.yaml`
- `5-monitoring-optimization/optimization/model_quantization.ipynb`
- `5-monitoring-optimization/optimization/caching_strategy.ipynb`
- `5-monitoring-optimization/optimization/autoscaling_config.yaml`

---

### 📍 Navigation
[⬅️ Previous: LangChain Integration](04-langchain-integration.md) | [🏠 Main Guide](README.md) | [➡️ Next: Advanced MLOps](06-advanced-mlops.md)

---

*Continue to Module 6 to implement advanced MLOps practices for enterprise deployment!*