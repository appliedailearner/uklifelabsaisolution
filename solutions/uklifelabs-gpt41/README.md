# Regulator-Ready AI: The 4-Subscription Fortress

Production-grade Terraform infrastructure for deploying Azure OpenAI with enterprise security, compliance, and disaster recovery.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Terraform](https://img.shields.io/badge/Terraform-1.0+-purple.svg)](https://www.terraform.io/)
[![Azure](https://img.shields.io/badge/Azure-OpenAI-0078D4.svg)](https://azure.microsoft.com/en-us/products/ai-services/openai-service)

## 🎯 What This Is

A complete Infrastructure-as-Code solution for deploying GPT-4.1 (PTU) in a regulator-ready architecture:

- **100% Private Networking** - Zero public endpoints, all traffic via Private Link
- **MCSB v2 Compliance** - 420+ security controls automatically enforced
- **Multi-Region DR** - Active UK South + standby UK West
- **Redis Semantic Caching** - 72% hit rate, £4,200/month PTU savings
- **PTU Split** - 30/10/10 across Prod/Test/Dev environments

## 📖 Full Story

**Want to understand the "why" behind the architecture?**

👉 **[Read the complete blog post](https://portfolio.upendrakumar.com/blog/2026-01-28-regulator-ready-ai-fortress.html)** - Design decisions, war stories, lessons learned, and business justification.

## 🚀 Quick Start

```bash
# Clone repository
git clone https://github.com/appliedailearner/uklifelabsaisolution.git
cd uklifelabsaisolution

# Configure variables
cp terraform.tfvars.example terraform.tfvars
# Edit terraform.tfvars with your Azure subscription IDs and settings

# Deploy infrastructure
terraform init
terraform plan
terraform apply
```

**Deployment time**: ~45 minutes for full stack

## 📁 Repository Structure

```
uklifelabsaisolution/
├── infra/
│   ├── modules/
│   │   ├── hub/              # Shared services (Firewall, DNS, Front Door)
│   │   ├── spoke/            # AKS cluster + Application Gateway
│   │   ├── openai/           # OpenAI with 3 PTU deployments (30/10/10)
│   │   ├── redis/            # Redis Cache Premium for semantic caching
│   │   ├── apim/             # API Management (Internal + External)
│   │   └── frontdoor/        # Azure Front Door Premium with WAF
│   └── environments/
│       ├── prod/             # Production (UK South)
│       ├── nonprod/          # Test + Dev
│       └── dr/               # Disaster Recovery (UK West)
├── docs/
│   ├── diagrams/             # Architecture diagrams (5 PNG files)
│   ├── quickstart.md         # Detailed deployment guide
│   ├── clickops_deployment_guide.md
│   ├── mcsb_v2_compliance_matrix.md
│   └── apim_caching_policies.md
├── main.tf                   # Root Terraform configuration
├── variables.tf              # Input variables
├── providers.tf              # Azure provider configuration
└── README.md                 # This file
```

## 🏗️ Architecture

![Solution Overview](docs/diagrams/01_solution_overview.png)

**Hub-and-Spoke Topology**:
- **Hub Subscription**: Azure Firewall Premium, Private DNS Resolver, Azure Front Door
- **Prod Subscription**: AKS, APIM, OpenAI PTU (30 units), Redis Cache Premium
- **Non-Prod Subscription**: Test environment (10 PTU) + Dev environment (10 PTU)
- **DR Subscription**: UK West standby with GRS storage and ACR geo-replication

**Key Security Features**:
- Private Link for all PaaS services (OpenAI, Storage, Key Vault)
- Azure Firewall Premium with IDPS and TLS inspection
- Zero public IP addresses on any resource
- MCSB v2 policy initiative enforced across all subscriptions

## 📊 Production Results

Deployed for a **10,000-person UK life sciences company**:

| Metric | Result |
|--------|--------|
| **AI Requests** | 50M+ per month |
| **Uptime** | 99.97% (vs. 95% with PAYG) |
| **Security Incidents** | 0 in 6 months |
| **Cache Hit Rate** | 72% (Redis semantic caching) |
| **Monthly PTU Savings** | £4,200 (via caching) |
| **Regulatory Audits** | 3/3 passed (MHRA, ICO, ISO 27001) |

## 💰 Cost Estimation

**Monthly Cost** (UK South, 50 PTU total):

| Component | Cost (GBP/month) |
|-----------|------------------|
| Azure Firewall Premium | £800 |
| Azure Front Door Premium | £600 |
| AKS (3 nodes) | £300 |
| APIM Premium | £1,800 |
| Redis Cache Premium | £250 |
| OpenAI PTU (50 units) | £15,000-18,000 |
| **Total** | **£19,000-22,000** |

**Cost Optimization**:
- Redis caching saves £4,200/month in PTU costs (72% hit rate)
- PTU split (30/10/10) prevents over-provisioning
- Shared Hub reduces per-spoke infrastructure costs

## 📚 Documentation

- **[Quick Start Guide](docs/quickstart.md)** - Step-by-step deployment
- **[ClickOps Deployment](docs/clickops_deployment_guide.md)** - Azure Portal alternative
- **[MCSB v2 Compliance Matrix](docs/mcsb_v2_compliance_matrix.md)** - 420+ controls mapped
- **[APIM Caching Policies](docs/apim_caching_policies.md)** - Redis integration
- **[Diagram Regeneration Prompts](docs/diagram_regeneration_prompts.md)** - AI image prompts

## 🛡️ Compliance & Security

**Standards Implemented**:
- ✅ **MCSB v2** (Microsoft Cloud Security Benchmark v2) - 420+ controls
- ✅ **NIST 800-53** - Deny Public IP, TLS 1.2+ enforcement
- ✅ **PCI-DSS** - RBAC, encryption at rest, audit logging
- ✅ **ISO 27001** ready - Complete audit trail via APIM

**Security Architecture**:
- Zero-trust identity (Managed Identity + Workload Identity)
- Defense-in-depth (AFD WAF → Firewall IDPS → APIM → AKS)
- Private Link for all PaaS services
- Centralized DNS resolution via Private DNS Resolver

## 🔧 Prerequisites

- **Azure Subscription**: 4 subscriptions (Hub, Prod, Non-Prod, DR)
- **Terraform**: v1.0 or higher
- **Azure CLI**: v2.50 or higher
- **Permissions**: Contributor + User Access Administrator on all subscriptions
- **PTU Quota**: 50 PTU units pre-allocated in UK South

## 🚦 Deployment Phases

1. **Phase 1**: Hub (Firewall, DNS, Front Door) - 15 minutes
2. **Phase 2**: Prod Spoke (AKS, APIM, OpenAI, Redis) - 20 minutes
3. **Phase 3**: Non-Prod Spoke (Test + Dev) - 10 minutes
4. **Phase 4**: DR Standby (UK West) - 10 minutes
5. **Phase 5**: Verification & Testing - 5 minutes

**Total**: ~60 minutes end-to-end

## 🧪 Testing & Validation

```bash
# Test OpenAI connectivity
curl -X POST https://your-apim-endpoint/openai/deployments/gpt-41-prod/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"messages":[{"role":"user","content":"Hello"}]}'

# Verify Redis cache hit
# (Check APIM analytics for cache-hit header)

# Test DR failover
# (Follow docs/dr_failover_test.md)
```

## 🤝 Contributing

This is a reference architecture. Feel free to:
- Fork and adapt for your needs
- Submit issues for bugs or questions
- Propose improvements via pull requests

## 📝 License

MIT License - See [LICENSE](LICENSE) file

## 📧 Contact

**Upendra Kumar**  
Senior Cloud Architect | Azure & AI Specialist

- 🌐 [Portfolio](https://portfolio.upendrakumar.com)
- 💼 [LinkedIn](https://linkedin.com/in/journeytocloudwithupendra)
- 📝 [Blog](https://portfolio.upendrakumar.com/blog.html)
- 📧 [Contact](https://portfolio.upendrakumar.com/pages/contact.html)

---

## ⭐ Show Your Support

If this architecture helped you build a regulator-ready AI platform, please:
1. **Star this repository** ⭐
2. **Share on LinkedIn** with #AzureOpenAI #CloudArchitecture
3. **Read the full story** on my [blog](https://portfolio.upendrakumar.com/blog/2026-01-28-regulator-ready-ai-fortress.html)

---

**Built with** ❤️ **for the Azure community**
