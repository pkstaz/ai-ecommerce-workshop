# OpenShift AI End-to-End Workshop
## Intelligent E-commerce Recommendation and Analysis System

<div align="center">

![OpenShift AI](https://img.shields.io/badge/OpenShift-AI-red?style=for-the-badge&logo=redhat)
![Workshop](https://img.shields.io/badge/Workshop-Hands--on-blue?style=for-the-badge)
![Duration](https://img.shields.io/badge/Duration-4%20hours-green?style=for-the-badge)
![English](https://img.shields.io/badge/Language-English-blue?style=for-the-badge)

**[🇪🇸 Versión en Español](../es/README.md)** | **[🏠 Main Repository](../../README.md)**

</div>

---

## 🎯 Welcome to the OpenShift AI Workshop

This comprehensive workshop guides you step-by-step through building an **intelligent e-commerce system** using the most advanced enterprise AI technologies. By the end, you'll have constructed a complete solution that combines predictive and generative models to create real business value.

**Instructor:** Carlos Estay | **Email:** cestay@redhat.com | **GitHub:** [pkstaz](https://github.com/pkstaz)

---

## 🚀 What You'll Build

### 🏗️ **Complete AI System for E-commerce**

```mermaid
graph LR
    A[📊 Sales Data] --> B[🤖 Predictive Model]
    B --> C[📈 Forecasts]
    
    D[💬 Content Requirements] --> E[🧠 Generative Model]
    E --> F[✍️ Automated Descriptions]
    
    C --> G[🔗 LangChain Orchestrator]
    F --> G
    
    G --> H[🖥️ Interactive Dashboard]
    H --> I[💼 Business Decisions]
    
    style A fill:#e3f2fd
    style D fill:#e3f2fd
    style B fill:#e8f5e8
    style E fill:#fce4ec
    style G fill:#fff3e0
    style H fill:#f1f8e9
    style I fill:#e1f5fe
```

### 💡 **Final System Capabilities**
- **📊 Sales Forecasting** with 87% accuracy in <50ms
- **🤖 Automated Content Generation** with Granite 3.1 8B
- **🔗 Intelligent Orchestration** using LangChain
- **📈 Real-time Monitoring** with automated alerts
- **🚀 Auto-scaling** based on demand
- **🛡️ Enterprise Security** and model governance

---

## 📚 Your Learning Journey

### 🗺️ **Workshop Roadmap**

| 🎯 | Module | Duration | What You'll Learn | What You'll Build |
|---|---------|----------|-------------------|-------------------|
| **1️⃣** | **[Environment Setup](01-environment-setup.md)** | 30 min | OpenShift AI, Jupyter, Datasets | Complete development environment |
| **2️⃣** | **[Predictive Model](02-predictive-model.md)** | 45 min | Random Forest, ONNX, OpenVINO | Sales forecasting system |
| **3️⃣** | **[Generative Model](03-generative-model.md)** | 45 min | Granite 3.1 8B, vLLM, Prompting | Automated content generator |
| **4️⃣** | **[LangChain Integration](04-langchain-integration.md)** | 45 min | Orchestration, Chains, Dashboard | Unified AI system |
| **5️⃣** | **[Monitoring & Optimization](05-monitoring.md)** | 30 min | Prometheus, Alerts, Performance | Production observability |
| **6️⃣** | **[Advanced MLOps](06-advanced-mlops.md)** | 30 min | CI/CD, A/B Testing, Governance | Enterprise practices |

### 🎓 **Skill Progression**

```
Beginner → Intermediate → Advanced → Expert
    ↓         ↓          ↓         ↓
 Module 1  Modules 2-3  Modules 4-5  Module 6
```

---

## 🎯 Start Your Journey

### ✅ **Quick Checklist**

Before starting, confirm you have:
- [ ] **OpenShift AI** installed and accessible
- [ ] **Jupyter Hub** running
- [ ] **GPU** available (recommended)
- [ ] **Basic knowledge** of Python and ML

### 🚀 **Get Started Now!**

<div align="center">

### **[👉 START WITH MODULE 1 👈](01-environment-setup.md)**

*Everything begins with a solid foundation*

</div>

---

## 📖 Detailed Modules

### 🛠️ **[Module 1: Environment Setup](01-environment-setup.md)**
**⏱️ Duration:** 30 minutes | **🎯 Level:** Beginner

**What you'll learn:**
- Verify OpenShift AI installation
- Create data science projects
- Configure Jupyter Workbench
- Download and validate datasets

**Outcome:** Complete environment ready for AI development

---

### 📊 **[Module 2: Predictive Model](02-predictive-model.md)**
**⏱️ Duration:** 45 minutes | **🎯 Level:** Intermediate

**What you'll learn:**
- Exploratory data analysis of sales data
- Advanced feature engineering
- Optimized Random Forest training
- ONNX export and OpenVINO deployment

**Outcome:** Sales forecasting model with 87% accuracy serving in production

**🎯 Target metrics:**
- ✅ Accuracy > 85%
- ✅ Latency < 100ms
- ✅ Availability 99.9%

---

### 🤖 **[Module 3: Generative Model](03-generative-model.md)**
**⏱️ Duration:** 45 minutes | **🎯 Level:** Intermediate-Advanced

**What you'll learn:**
- Granite 3.1 8B architecture and capabilities
- vLLM configuration and optimization
- Advanced prompt engineering techniques
- LLM performance benchmarking

**Outcome:** High-performance text generation service with 75 TPS

**🎯 Target metrics:**
- ✅ Throughput > 50 TPS
- ✅ First token < 500ms
- ✅ High content quality

---

### 🔗 **[Module 4: LangChain Integration](04-langchain-integration.md)**
**⏱️ Duration:** 45 minutes | **🎯 Level:** Advanced

**What you'll learn:**
- Multi-model AI orchestration
- Specialized chain development
- Interactive dashboard construction
- End-to-end testing

**Outcome:** Unified AI system with functional web interface

**🎯 Key capabilities:**
- ✅ Complete product analysis
- ✅ Personalized recommendations
- ✅ Real-time dashboard
- ✅ Unified APIs

---

### 📈 **[Module 5: Monitoring & Optimization](05-monitoring.md)**
**⏱️ Duration:** 30 minutes | **🎯 Level:** Advanced

**What you'll learn:**
- OpenShift AI monitoring configuration
- Intelligent alerting implementation
- Performance optimization (quantization, caching)
- Auto-scaling configuration

**Outcome:** Optimized and monitored system ready for production

**🎯 Optimizations achieved:**
- ✅ 44% latency improvement
- ✅ 70% cache hit rate
- ✅ 30% cost reduction
- ✅ Automatic scaling

---

### 🚀 **[Module 6: Advanced MLOps](06-advanced-mlops.md)**
**⏱️ Duration:** 30 minutes | **🎯 Level:** Expert

**What you'll learn:**
- Model versioning and A/B testing
- CI/CD pipelines for ML
- Enterprise security and governance
- ROI measurement and business value

**Outcome:** Enterprise-grade MLOps implementation

**🎯 Practices implemented:**
- ✅ Automated CI/CD
- ✅ Statistical A/B testing
- ✅ Security policies
- ✅ ROI tracking (400% first year)

---

## 🎯 Guaranteed Learning Outcomes

### 💼 **Upon Completing This Workshop, You'll Be Able To:**

#### **🔧 Technical Skills**
- [x] **Deploy ML models** on OpenShift AI with confidence
- [x] **Optimize inference** using ONNX and OpenVINO for maximum performance
- [x] **Serve LLMs** with vLLM for high-throughput applications
- [x] **Orchestrate complex AI systems** with LangChain
- [x] **Implement comprehensive monitoring** for production
- [x] **Apply enterprise MLOps** with CI/CD and governance

#### **💡 Business Capabilities**
- [x] **Reduce time-to-market** for AI products by 60%
- [x] **Optimize operational costs** through automation (30% reduction)
- [x] **Scale AI applications** without manual intervention
- [x] **Measure ROI** of AI implementations systematically
- [x] **Implement governance** for enterprise compliance
- [x] **Create sustainable value** with production AI

---

## 🛠️ Complete Technology Stack

### 🏗️ **Platform and Orchestration**
| Technology | Purpose | Module |
|-----------|----------|--------|
| **OpenShift AI** | Enterprise MLOps platform | 1-6 |
| **KServe** | Kubernetes-native model serving | 2-3 |
| **LangChain** | LLM application orchestration | 4 |

### 🤖 **Models and Frameworks**
| Technology | Purpose | Module |
|-----------|----------|--------|
| **Random Forest** | Sales prediction model | 2 |
| **Granite 3.1 8B** | Text generation model | 3 |
| **ONNX** | Standard model format | 2 |
| **OpenVINO** | Inference optimization | 2 |
| **vLLM** | Efficient LLM serving | 3 |

### 📊 **Monitoring and Operations**
| Technology | Purpose | Module |
|-----------|----------|--------|
| **Prometheus** | Metrics and monitoring | 5 |
| **Grafana** | Data visualization | 5 |
| **Tekton** | CI/CD pipelines | 6 |
| **Argo CD** | GitOps deployment | 6 |

---

## 📈 ROI and Business Value

### 💰 **Projected Return on Investment**

| Metric | Baseline | After Workshop | Improvement |
|---------|----------|----------------|-------------|
| **Development Time** | 6 months | 2 months | **66% reduction** |
| **Operational Costs** | $100k/year | $70k/year | **30% savings** |
| **Prediction Accuracy** | 70% | 87% | **24% improvement** |
| **Model Throughput** | 10 RPS | 100+ RPS | **10x increase** |

### 🎯 **Quantified Business Value**
- **Total ROI:** 400% in first year
- **Payback Period:** 6 months
- **Annual Value:** $800k in combined benefits
- **Efficiency:** 60% reduction in analysis time

---

## 🤝 Support During and After the Workshop

### 👨‍🏫 **Your Instructor**
**Carlos Estay**
- **📧 Email:** cestay@redhat.com
- **🐱 GitHub:** [pkstaz](https://github.com/pkstaz)
- **💼 LinkedIn:** [Carlos Estay](https://www.linkedin.com/in/cestayg/)

### 🆘 **Get Immediate Help**
- **💬 During workshop:** Ask the instructor directly
- **🐛 Technical issues:** [Create GitHub issue](../../issues)
- **📚 Troubleshooting:** Each module includes problem-solving sections
- **🤝 Community:** [OpenShift AI Forums](https://www.redhat.com/en/technologies/cloud-computing/openshift/openshift-ai)

### 📞 **Post-Workshop Support**
- **📧 Email consultations** - 24-48 hour response
- **📅 Monthly office hours** - Group Q&A sessions
- **📚 Updated resources** - Access to new versions and content
- **🎓 Certification paths** - Routes to official Red Hat certifications

---

## 📚 Extended Learning Resources

### 📖 **Essential Documentation**
- **[OpenShift AI Docs](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed)** - Complete official documentation
- **[KServe Guide](https://kserve.github.io/website/)** - Model serving on Kubernetes
- **[LangChain Cookbook](https://python.langchain.com/)** - Recipes and patterns for LLM apps
- **[MLOps Best Practices](https://ml-ops.org/)** - Industry standards and practices

### 🎓 **Certification Paths**
- **Red Hat Certified Specialist in OpenShift AI** - Official certification
- **Kubernetes Application Developer (CKAD)** - Kubernetes fundamentals
- **MLOps Engineer Certification** - ML operations specialization

### 🌐 **Community and Events**
- **[Red Hat Summit](https://www.redhat.com/en/summit)** - Annual flagship conference
- **[OpenShift Commons](https://commons.openshift.org/)** - User community
- **[MLOps Community](https://mlops.community/)** - Networking and best practices
- **[AI/ML Meetups](https://www.meetup.com/topics/machine-learning/)** - Local events

### 📄 **Whitepapers and Case Studies**
- **MLOps Best Practices** - Enterprise implementation guides
- **AI/ML Security** - Security best practices
- **AI Implementation ROI** - Case studies and metrics

---

## 🚀 Your AI Journey Starts Here!

### 🎯 **Ready to Transform Your Career?**

This workshop isn't just about learning technologies - it's about **transforming how your organization implements AI**. The skills you develop here will position you as a leader in the enterprise MLOps space.

### 🏆 **By the End of the Workshop You'll Have:**
- ✅ **Built** a complete AI system from scratch
- ✅ **Mastered** cutting-edge MLOps tools
- ✅ **Implemented** industry best practices
- ✅ **Created** measurable and quantifiable business value
- ✅ **Prepared** your organization for the future of AI

### 🌟 **Your Impact After the Workshop:**
- **🚀 Accelerate** AI project time-to-market
- **💰 Generate** measurable ROI on implementations
- **📈 Scale** ML operations without limits
- **🛡️ Ensure** compliance and governance
- **👥 Lead** your team's digital transformation

---

<div align="center">

## 🎬 **Time to Get Started!**

### **[🚀 BEGIN MODULE 1: ENVIRONMENT SETUP 🚀](01-environment-setup.md)**

*A journey of a thousand miles begins with a single step*

---

### 📞 **Questions Before Starting?**

**📧 Contact Carlos:** cestay@redhat.com  
**🐱 Follow on GitHub:** [pkstaz](https://github.com/pkstaz)  
**💼 Connect on LinkedIn:** [Carlos Estay](https://www.linkedin.com/in/cestayg/)

---

</div>

### 📍 **Workshop Navigation**

| **Module** | **Title** | **Duration** | **Level** |
|------------|------------|--------------|-----------|
| **[1️⃣](01-environment-setup.md)** | Environment Setup | 30 min | 🟢 Beginner |
| **[2️⃣](02-predictive-model.md)** | Predictive Model | 45 min | 🟡 Intermediate |
| **[3️⃣](03-generative-model.md)** | Generative Model | 45 min | 🟡 Intermediate |
| **[4️⃣](04-langchain-integration.md)** | LangChain Integration | 45 min | 🟠 Advanced |
| **[5️⃣](05-monitoring.md)** | Monitoring & Optimization | 30 min | 🟠 Advanced |
| **[6️⃣](06-advanced-mlops.md)** | Advanced MLOps | 30 min | 🔴 Expert |

---

**🎉 Welcome to the future of enterprise AI! Let's build something incredible together! 🎉**

---

**© 2025 Red Hat, Inc. - Workshop Material | [🏠 Home](../../README.md) | [🇪🇸 Español](../es/README.md)**