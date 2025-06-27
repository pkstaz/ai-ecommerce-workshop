# Module 6: Advanced MLOps
## 🚀 Enterprise Production Deployment

---

### 📍 Navigation
[⬅️ Previous: Monitoring](05-monitoring.md) | [🏠 Main Guide](../README.md)

---

**Duration:** 30 minutes  
**Objective:** Implement advanced MLOps practices for enterprise-grade deployments.

---

## 🎯 Module Overview

In this module, you will:
- 🔄 **Implement** model versioning and A/B testing
- 🚀 **Build** CI/CD pipelines for automated deployment
- 🔒 **Secure** AI models with enterprise security practices
- 📋 **Establish** model governance and compliance frameworks
- 🏗️ **Scale** to multi-environment deployments
- 📊 **Measure** business impact and ROI

---

## 🔄 Step 6.1: Model Versioning and A/B Testing

### Model Version Management

**Model versioning** enables:
- **Safe Deployments** - Roll back if issues arise
- **Performance Comparison** - Measure improvement between versions
- **Audit Trail** - Track model evolution and decisions
- **Compliance** - Meet regulatory requirements

### Implement Model Versioning

1. **Setup Model Version Management**
   - Execute: `6-advanced-mlops/notebooks/model_versioning.ipynb`
   
   This notebook implements:
   ```python
   class ModelVersionManager:
       def __init__(self, registry_url):
           self.registry = ModelRegistry(registry_url)
           self.version_format = "v{major}.{minor}.{patch}"
       
       def register_model(self, model_artifacts, metadata):
           """Register new model version"""
           version = self.generate_version_number(metadata)
           
           # Package model artifacts
           model_package = self.package_model(
               model_artifacts,
               version,
               metadata
           )
           
           # Register in model registry
           model_id = self.registry.register_model(
               name=metadata['model_name'],
               version=version,
               artifacts=model_package,
               metadata=metadata,
               tags=metadata.get('tags', [])
           )
           
           return model_id, version
       
       def deploy_model_version(self, model_name, version, environment):
           """Deploy specific model version"""
           # Get model artifacts
           model_package = self.registry.get_model(model_name, version)
           
           # Create deployment configuration
           deployment_config = self.create_deployment_config(
               model_package, 
               environment
           )
           
           # Deploy using KServe
           return self.deploy_to_kserve(deployment_config)
       
       def rollback_model(self, model_name, target_version, environment):
           """Rollback to previous model version"""
           current_deployment = self.get_current_deployment(model_name, environment)
           
           # Deploy target version
           self.deploy_model_version(model_name, target_version, environment)
           
           # Monitor rollback success
           return self.validate_rollback(model_name, target_version)
   ```

### A/B Testing Framework

1. **Implement A/B Testing**
   - Deploy: `6-advanced-mlops/pipelines/ab_testing.yaml`
   
   ```yaml
   apiVersion: argoproj.io/v1alpha1
   kind: Rollout
   metadata:
     name: sales-model-ab-test
   spec:
     replicas: 6
     strategy:
       canary:
         steps:
         - setWeight: 20    # 20% traffic to new version
         - pause: {duration: 10m}
         - setWeight: 40    # 40% traffic to new version
         - pause: {duration: 10m}
         - setWeight: 60    # 60% traffic to new version
         - pause: {duration: 10m}
         - setWeight: 80    # 80% traffic to new version
         - pause: {duration: 10m}
         - setWeight: 100   # 100% traffic to new version
         
         trafficRouting:
           istio:
             virtualService:
               name: sales-model-vs
             destinationRule:
               name: sales-model-dr
         
         analysis:
           templates:
           - templateName: model-accuracy-analysis
           args:
           - name: service-name
             value: sales-forecasting-model
           
   ---
   apiVersion: argoproj.io/v1alpha1
   kind: AnalysisTemplate
   metadata:
     name: model-accuracy-analysis
   spec:
     metrics:
     - name: accuracy-score
       successCondition: result[0] >= 0.85
       failureCondition: result[0] < 0.80
       provider:
         prometheus:
           address: http://prometheus.monitoring.svc.cluster.local:9090
           query: |
             model_prediction_accuracy{model_name="{{args.service-name}}"}
     - name: error-rate
       successCondition: result[0] <= 0.05
       failureCondition: result[0] > 0.10
       provider:
         prometheus:
           address: http://prometheus.monitoring.svc.cluster.local:9090
           query: |
             rate(prediction_errors_total{model="{{args.service-name}}"}[5m])
   ```

### Statistical Validation

```python
# A/B test statistical analysis
class ABTestAnalyzer:
    def __init__(self):
        self.significance_level = 0.05
        self.minimum_sample_size = 1000
    
    def analyze_test_results(self, control_metrics, treatment_metrics):
        """Perform statistical analysis of A/B test results"""
        from scipy import stats
        
        results = {}
        
        # Test for statistical significance
        for metric_name in control_metrics.keys():
            control_data = control_metrics[metric_name]
            treatment_data = treatment_metrics[metric_name]
            
            # Perform t-test
            t_stat, p_value = stats.ttest_ind(control_data, treatment_data)
            
            # Calculate effect size (Cohen's d)
            effect_size = self.calculate_cohens_d(control_data, treatment_data)
            
            # Determine significance
            is_significant = p_value < self.significance_level
            
            results[metric_name] = {
                'p_value': p_value,
                'effect_size': effect_size,
                'is_significant': is_significant,
                'control_mean': np.mean(control_data),
                'treatment_mean': np.mean(treatment_data),
                'improvement': (np.mean(treatment_data) - np.mean(control_data)) / np.mean(control_data)
            }
        
        return results
    
    def generate_recommendation(self, analysis_results):
        """Generate deployment recommendation based on A/B test results"""
        key_metrics = ['accuracy', 'latency', 'user_satisfaction']
        
        positive_results = 0
        negative_results = 0
        
        for metric in key_metrics:
            if metric in analysis_results:
                result = analysis_results[metric]
                if result['is_significant']:
                    if result['improvement'] > 0:
                        positive_results += 1
                    else:
                        negative_results += 1
        
        if positive_results > negative_results:
            return "DEPLOY", "New model shows significant improvement"
        elif negative_results > positive_results:
            return "ROLLBACK", "New model shows degraded performance"
        else:
            return "CONTINUE_TESTING", "Results inconclusive, continue testing"
```

**✅ Checkpoint:** Model versioning and A/B testing framework are operational.

---

## 🚀 Step 6.2: CI/CD Pipeline Integration

### GitOps Workflow Implementation

**Continuous Integration/Deployment** for ML models includes:
- **Automated Training** - Trigger retraining on data updates
- **Model Validation** - Automated testing and quality gates
- **Deployment Automation** - Zero-downtime model updates
- **Rollback Mechanisms** - Automatic failure recovery

### GitOps Pipeline Configuration

1. **Setup GitOps Pipeline**
   - Review: `6-advanced-mlops/pipelines/gitops_pipeline.yaml`
   
   ```yaml
   apiVersion: tekton.dev/v1beta1
   kind: Pipeline
   metadata:
     name: ml-model-cicd
   spec:
     params:
     - name: git-repo-url
       type: string
     - name: model-name
       type: string
     - name: target-environment
       type: string
       default: "staging"
     
     workspaces:
     - name: source-code
     - name: model-artifacts
     
     tasks:
     # Task 1: Source Code Checkout
     - name: git-clone
       taskRef:
         name: git-clone
       params:
       - name: url
         value: $(params.git-repo-url)
       workspaces:
       - name: output
         workspace: source-code
     
     # Task 2: Data Validation
     - name: validate-data
       taskRef:
         name: data-validation
       runAfter: ["git-clone"]
       workspaces:
       - name: source
         workspace: source-code
     
     # Task 3: Model Training
     - name: train-model
       taskRef:
         name: model-training
       runAfter: ["validate-data"]
       params:
       - name: model-name
         value: $(params.model-name)
       workspaces:
       - name: source
         workspace: source-code
       - name: artifacts
         workspace: model-artifacts
     
     # Task 4: Model Validation
     - name: validate-model
       taskRef:
         name: model-validation
       runAfter: ["train-model"]
       workspaces:
       - name: artifacts
         workspace: model-artifacts
     
     # Task 5: ONNX Export
     - name: export-onnx
       taskRef:
         name: onnx-export
       runAfter: ["validate-model"]
       workspaces:
       - name: artifacts
         workspace: model-artifacts
     
     # Task 6: Security Scanning
     - name: security-scan
       taskRef:
         name: security-scanner
       runAfter: ["export-onnx"]
       workspaces:
       - name: artifacts
         workspace: model-artifacts
     
     # Task 7: Deploy to Staging
     - name: deploy-staging
       taskRef:
         name: model-deploy
       runAfter: ["security-scan"]
       params:
       - name: environment
         value: "staging"
       - name: model-name
         value: $(params.model-name)
       workspaces:
       - name: artifacts
         workspace: model-artifacts
     
     # Task 8: Integration Tests
     - name: integration-tests
       taskRef:
         name: integration-testing
       runAfter: ["deploy-staging"]
       params:
       - name: model-endpoint
         value: "http://$(params.model-name)-staging:8080"
     
     # Task 9: Production Deployment (Manual Approval)
     - name: deploy-production
       taskRef:
         name: model-deploy
       runAfter: ["integration-tests"]
       params:
       - name: environment
         value: "production"
       - name: model-name
         value: $(params.model-name)
       workspaces:
       - name: artifacts
         workspace: model-artifacts
   ```

### Quality Gates Implementation

1. **Implement Quality Gates**
   - Execute: `6-advanced-mlops/notebooks/quality_gates.ipynb`
   
   ```python
   class ModelQualityGates:
       def __init__(self):
           self.quality_thresholds = {
               'accuracy_threshold': 0.85,
               'latency_threshold': 100,  # milliseconds
               'memory_threshold': 2048,  # MB
               'security_score_threshold': 8.0,
               'bias_threshold': 0.1
           }
       
       def run_quality_checks(self, model_artifacts, test_data):
           """Run comprehensive quality checks"""
           results = {}
           
           # Performance quality gate
           results['performance'] = self.check_model_performance(
               model_artifacts, test_data
           )
           
           # Latency quality gate
           results['latency'] = self.check_inference_latency(model_artifacts)
           
           # Resource usage quality gate
           results['resources'] = self.check_resource_usage(model_artifacts)
           
           # Security quality gate
           results['security'] = self.check_security_vulnerabilities(model_artifacts)
           
           # Bias and fairness quality gate
           results['fairness'] = self.check_model_bias(model_artifacts, test_data)
           
           # Data quality gate
           results['data_quality'] = self.check_data_quality(test_data)
           
           return results
       
       def evaluate_gate_results(self, quality_results):
           """Evaluate if model passes all quality gates"""
           passed_gates = []
           failed_gates = []
           
           for gate_name, result in quality_results.items():
               if result['passed']:
                   passed_gates.append(gate_name)
               else:
                   failed_gates.append({
                       'gate': gate_name,
                       'reason': result['reason'],
                       'actual_value': result['actual_value'],
                       'threshold': result['threshold']
                   })
           
           overall_result = {
               'passed': len(failed_gates) == 0,
               'passed_gates': passed_gates,
               'failed_gates': failed_gates,
               'recommendation': self.generate_recommendation(failed_gates)
           }
           
           return overall_result
       
       def check_model_performance(self, model_artifacts, test_data):
           """Check model accuracy and performance metrics"""
           # Load model and run evaluation
           model = load_model(model_artifacts)
           predictions = model.predict(test_data['features'])
           accuracy = calculate_accuracy(predictions, test_data['labels'])
           
           return {
               'passed': accuracy >= self.quality_thresholds['accuracy_threshold'],
               'actual_value': accuracy,
               'threshold': self.quality_thresholds['accuracy_threshold'],
               'reason': f"Accuracy {accuracy:.3f} vs threshold {self.quality_thresholds['accuracy_threshold']}"
           }
   ```

**✅ Checkpoint:** CI/CD pipeline with quality gates is implemented and tested.

---

## 🔒 Step 6.3: Security and Governance

### Security Best Practices

**AI Model Security** encompasses:
- **Model Protection** - Prevent model theft and adversarial attacks
- **Data Privacy** - Protect sensitive training and inference data
- **Access Control** - Restrict model access to authorized users
- **Audit Logging** - Track all model interactions

### Implement Security Policies

1. **Deploy Security Policies**
   - Apply: `6-advanced-mlops/security/security_policies.yaml`
   
   ```yaml
   # Network Policy for Model Services
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: model-serving-network-policy
   spec:
     podSelector:
       matchLabels:
         component: model-serving
     policyTypes:
     - Ingress
     - Egress
     ingress:
     - from:
       - podSelector:
           matchLabels:
             component: api-gateway
       - namespaceSelector:
           matchLabels:
             name: monitoring
       ports:
       - protocol: TCP
         port: 8080
     egress:
     - to: []
       ports:
       - protocol: TCP
         port: 443  # HTTPS only
       - protocol: TCP
         port: 53   # DNS
   
   ---
   # Pod Security Policy
   apiVersion: policy/v1beta1
   kind: PodSecurityPolicy
   metadata:
     name: model-serving-psp
   spec:
     privileged: false
     allowPrivilegeEscalation: false
     requiredDropCapabilities:
       - ALL
     volumes:
       - 'configMap'
       - 'emptyDir'
       - 'projected'
       - 'secret'
       - 'downwardAPI'
       - 'persistentVolumeClaim'
     runAsUser:
       rule: 'MustRunAsNonRoot'
     seLinux:
       rule: 'RunAsAny'
     fsGroup:
       rule: 'RunAsAny'
   
   ---
   # RBAC Configuration
   apiVersion: rbac.authorization.k8s.io/v1
   kind: Role
   metadata:
     name: model-operator
   rules:
   - apiGroups: ["serving.kserve.io"]
     resources: ["inferenceservices"]
     verbs: ["get", "list", "watch", "create", "update", "patch"]
   - apiGroups: [""]
     resources: ["secrets", "configmaps"]
     verbs: ["get", "list"]
   
   ---
   apiVersion: rbac.authorization.k8s.io/v1
   kind: RoleBinding
   metadata:
     name: model-operator-binding
   subjects:
   - kind: User
     name: ml-engineer
     apiGroup: rbac.authorization.k8s.io
   roleRef:
     kind: Role
     name: model-operator
     apiGroup: rbac.authorization.k8s.io
   ```

### Model Governance Framework

1. **Establish Model Governance**
   - Setup: `6-advanced-mlops/notebooks/model_governance.ipynb`
   
   ```python
   class ModelGovernanceFramework:
       def __init__(self):
           self.governance_policies = {
               'data_retention': 365,  # days
               'model_approval_required': True,
               'audit_trail_required': True,
               'bias_monitoring_required': True,
               'explainability_required': True
           }
       
       def create_model_card(self, model_metadata):
           """Create comprehensive model documentation"""
           model_card = {
               'model_details': {
                   'name': model_metadata['name'],
                   'version': model_metadata['version'],
                   'date': model_metadata['creation_date'],
                   'type': model_metadata['model_type'],
                   'architecture': model_metadata['architecture']
               },
               'intended_use': {
                   'primary_use': model_metadata['primary_use'],
                   'primary_users': model_metadata['primary_users'],
                   'out_of_scope_uses': model_metadata['limitations']
               },
               'training_data': {
                   'dataset': model_metadata['training_dataset'],
                   'size': model_metadata['dataset_size'],
                   'preprocessing': model_metadata['preprocessing_steps']
               },
               'evaluation_data': {
                   'dataset': model_metadata['test_dataset'],
                   'metrics': model_metadata['evaluation_metrics'],
                   'performance': model_metadata['performance_results']
               },
               'ethical_considerations': {
                   'bias_analysis': model_metadata['bias_analysis'],
                   'fairness_metrics': model_metadata['fairness_metrics'],
                   'potential_harms': model_metadata['potential_harms']
               },
               'caveats_and_recommendations': {
                   'limitations': model_metadata['known_limitations'],
                   'recommendations': model_metadata['usage_recommendations']
               }
           }
           
           return model_card
       
       def track_model_lineage(self, model_id):
           """Track complete model lineage"""
           lineage = {
               'data_sources': self.get_data_sources(model_id),
               'training_jobs': self.get_training_history(model_id),
               'model_versions': self.get_version_history(model_id),
               'deployments': self.get_deployment_history(model_id),
               'performance_history': self.get_performance_history(model_id)
           }
           
           return lineage
       
       def audit_model_usage(self, model_id, time_range):
           """Generate audit report for model usage"""
           audit_report = {
               'model_id': model_id,
               'audit_period': time_range,
               'prediction_requests': self.count_predictions(model_id, time_range),
               'users': self.get_model_users(model_id, time_range),
               'performance_metrics': self.get_performance_metrics(model_id, time_range),
               'compliance_status': self.check_compliance(model_id),
               'security_events': self.get_security_events(model_id, time_range)
           }
           
           return audit_report
   ```

**✅ Checkpoint:** Security policies and governance framework are implemented.

---

## 📊 Step 6.4: Business Impact Measurement

### ROI and KPI Tracking

```python
class BusinessImpactTracker:
    def __init__(self):
        self.kpi_definitions = {
            'prediction_accuracy_improvement': {
                'description': 'Improvement in forecast accuracy vs baseline',
                'measurement': 'percentage',
                'target': 15  # 15% improvement
            },
            'response_time_improvement': {
                'description': 'Reduction in decision-making time',
                'measurement': 'percentage',
                'target': 50  # 50% faster decisions
            },
            'cost_reduction': {
                'description': 'Operational cost savings',
                'measurement': 'currency',
                'target': 100000  # $100k annual savings
            },
            'revenue_impact': {
                'description': 'Additional revenue from better predictions',
                'measurement': 'currency',
                'target': 500000  # $500k annual increase
            }
        }
    
    def calculate_roi(self, implementation_cost, annual_benefits):
        """Calculate return on investment"""
        if implementation_cost == 0:
            return float('inf')
        
        roi = ((annual_benefits - implementation_cost) / implementation_cost) * 100
        payback_period = implementation_cost / annual_benefits
        
        return {
            'roi_percentage': roi,
            'payback_period_years': payback_period,
            'net_benefit': annual_benefits - implementation_cost
        }
    
    def measure_business_impact(self, baseline_metrics, current_metrics):
        """Measure overall business impact"""
        impact_analysis = {}
        
        for kpi_name, kpi_def in self.kpi_definitions.items():
            baseline_value = baseline_metrics.get(kpi_name, 0)
            current_value = current_metrics.get(kpi_name, 0)
            
            if kpi_def['measurement'] == 'percentage':
                improvement = current_value - baseline_value
            else:  # currency or absolute values
                improvement = ((current_value - baseline_value) / baseline_value) * 100 if baseline_value != 0 else 0
            
            impact_analysis[kpi_name] = {
                'baseline': baseline_value,
                'current': current_value,
                'improvement': improvement,
                'target_met': improvement >= kpi_def['target'],
                'description': kpi_def['description']
            }
        
        return impact_analysis
```

### Success Metrics Dashboard

Expected business outcomes:

| Metric | Baseline | Target | Achieved | Status |
|--------|----------|--------|----------|--------|
| **Prediction Accuracy** | 70% | 85% | 87% | ✅ Exceeded |
| **Response Time** | 2 hours | 30 minutes | 15 minutes | ✅ Exceeded |
| **Cost Reduction** | $0 | $100k/year | $150k/year | ✅ Exceeded |
| **Revenue Impact** | $0 | $500k/year | $650k/year | ✅ Exceeded |

---

## 🎉 Workshop Completion

### 🏆 Congratulations! You Have Successfully:

✅ **Built End-to-End AI System**
- Deployed predictive models with OpenVINO optimization
- Implemented generative AI with vLLM serving
- Created unified orchestration with LangChain
- Built interactive dashboard interface

✅ **Achieved Production Readiness**
- Comprehensive monitoring and alerting
- Performance optimization and auto-scaling
- Security policies and governance framework
- CI/CD pipelines with quality gates

✅ **Implemented Enterprise MLOps**
- Model versioning and A/B testing
- Automated deployment workflows
- Compliance and audit capabilities
- Business impact measurement

### 📈 Key Achievements

| Component | Achievement |
|-----------|-------------|
| **Predictive Model** | 87% accuracy, 45ms latency |
| **Generative Model** | 75 TPS, 300ms first token |
| **System Integration** | 99.9% uptime, automated scaling |
| **Business Impact** | $800k annual value, 6-month ROI |

### 🚀 Your AI System is Now:

- **🏗️ Production-Ready** - Monitoring, scaling, security
- **🔄 Continuously Deployed** - Automated CI/CD pipelines
- **📊 Business-Aligned** - ROI tracking and KPI measurement
- **🛡️ Enterprise-Secure** - Governance and compliance
- **📈 Performance-Optimized** - 50% cost reduction achieved

---

## 🎓 Next Steps and Continued Learning

### Immediate Actions
1. **Deploy to Production** - Apply learnings to your real use cases
2. **Extend Capabilities** - Add more models and use cases
3. **Scale Team Knowledge** - Share expertise with colleagues
4. **Measure Business Impact** - Track ROI and success metrics

### Advanced Learning Paths
- **Distributed Training** - Scale to larger models and datasets
- **Federated Learning** - Multi-organization AI collaboration
- **Edge Deployment** - Deploy models to edge devices
- **Multi-Cloud MLOps** - Cross-platform AI deployment

### Community and Support
- **OpenShift AI Community** - Join discussions and share experiences
- **Red Hat Training** - Pursue advanced certifications
- **Conference Presentations** - Share your success story
- **Open Source Contributions** - Contribute to AI/ML projects

---

### 📁 Files Referenced in This Module
- `6-advanced-mlops/notebooks/model_versioning.ipynb`
- `6-advanced-mlops/pipelines/ab_testing.yaml`
- `6-advanced-mlops/pipelines/gitops_pipeline.yaml`
- `6-advanced-mlops/notebooks/quality_gates.ipynb`
- `6-advanced-mlops/security/security_policies.yaml`
- `6-advanced-mlops/notebooks/model_governance.ipynb`

---

### 📍 Navigation
[⬅️ Previous: Monitoring](05-monitoring.md) | [🏠 Main Guide](../README.md)

---

## 🙏 Thank You!

**You have completed the OpenShift AI End-to-End Workshop!**

Your feedback is valuable for improving future workshops. Please share:
- What you found most valuable
- Areas for improvement
- Real-world applications you plan to implement
- Additional topics you'd like to explore

**Contact Information:**
- **Instructor:** Carlos Estay
- **Email:** cestay@redhat.com
- **GitHub:** [pkstaz](https://github.com/pkstaz)

---

*🎉 Congratulations on mastering enterprise-grade AI deployment with OpenShift AI! Go build amazing AI solutions! 🚀*