# Project Context — Infrastructure

## Overview
<!-- PREENCHA: Descreva o projeto de infraestrutura -->
<!-- Ex: "Infraestrutura cloud Azure para aplicação SaaS com alta disponibilidade" -->
<!-- Ex: "Ambiente híbrido (on-prem + Azure) para empresa de médio porte" -->

## Infrastructure Stack
<!-- PREENCHA: Descomente e ajuste as linhas do seu stack -->

### Cloud Provider
<!-- - Azure (principal) -->
<!-- - AWS -->
<!-- - GCP -->
<!-- - On-premises -->
<!-- - Híbrido (on-prem + cloud) -->

### IaC (Infrastructure as Code)
<!-- - Terraform (principal) -->
<!-- - OpenTofu -->
<!-- - Pulumi -->
<!-- - Bicep / ARM Templates -->
<!-- - CloudFormation -->

### Configuration Management
<!-- - Ansible -->
<!-- - Puppet -->
<!-- - Chef -->
<!-- - Salt -->

### CI/CD
<!-- - GitHub Actions -->
<!-- - Azure DevOps Pipelines -->
<!-- - GitLab CI -->
<!-- - Jenkins -->

### Monitoring
<!-- - Grafana + Prometheus -->
<!-- - Zabbix -->
<!-- - Datadog -->
<!-- - Azure Monitor / Application Insights -->
<!-- - AWS CloudWatch -->

### Networking
<!-- - pfSense -->
<!-- - MikroTik -->
<!-- - Fortinet -->
<!-- - Cloud-native (NSG / Security Groups) -->

### Secret Management
<!-- - Azure Key Vault -->
<!-- - HashiCorp Vault -->
<!-- - AWS Secrets Manager -->

### Container Runtime (se aplicável)
<!-- - Docker -->
<!-- - Kubernetes (AKS / EKS / GKE) -->
<!-- - Docker Compose -->

## Architecture
<!-- PREENCHA: Ajuste ao seu padrão -->
- Infrastructure as Code (tudo versionado)
- Remote state com locking
- Ambientes separados (dev/staging/prod)
- GitOps para deploys
- Defense-in-depth (segmentação de rede)

## Conventions

### Naming
- Recursos cloud: `{empresa}-{ambiente}-{tipo}-{nome}` (ex: `contoso-prod-vnet-hub`)
- Módulos Terraform: kebab-case (`vnet-hub-spoke`)
- Variáveis: snake_case (`resource_group_name`)
- Tags obrigatórias: `environment`, `owner`, `project`, `cost-center`
- Branches: `feat/`, `fix/`, `infra/`

### File Structure
- Módulos Terraform: `main.tf`, `variables.tf`, `outputs.tf`, `README.md`
- Ambientes: separados por workspace ou diretórios (`environments/dev/`, `environments/prod/`)
- Ansible: roles em `roles/`, inventários separados por ambiente

### SECURITY RULES (NON-NEGOTIABLE)
1. NUNCA credenciais no código — .env/.tfvars no .gitignore, secrets no vault
2. SEMPRE remote backend para Terraform state com locking
3. SEMPRE least privilege para IAM (nunca Owner/Admin sem justificativa)
4. SEMPRE MFA para acesso administrativo à produção
5. SEMPRE TLS 1.2+ para comunicação entre serviços
6. NUNCA firewall "any any allow" — regras específicas com documentação
7. SEMPRE scan de segurança (tfsec/checkov) antes de apply
8. SEMPRE testes em staging/dev antes de produção
9. SEMPRE pin de versão em providers, modules e imagens base
10. SEMPRE audit trail — quem, o quê, quando, por quê
11. SEMPRE rotação de secrets com política definida
12. SEMPRE backup automatizado e criptografado

### AI-Assisted Infrastructure Rules
- O AI DEVE seguir as security rules acima mesmo sem ser pedido
- O AI DEVE usar remote backend em TODO projeto Terraform
- O AI NÃO DEVE gerar código que desabilite segurança "para testar"
- O AI DEVE incluir validação (fmt, validate, lint, scan) em todo workflow
- O AI DEVE documentar decisões e mudanças de arquitetura
- O AI DEVE explicar em termos que um profissional de infra entende

## Team
<!-- PREENCHA: -->
<!-- - Profissional de infraestrutura / Cloud Engineer / SysAdmin -->
<!-- - Uso intensivo de AI para geração de IaC -->
<!-- - Foco: automação com segurança e boas práticas -->
