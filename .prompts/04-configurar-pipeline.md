# Prompt 04 — Configurar Pipeline CI/CD

> **Quando usar:** Para criar ou configurar pipelines de CI/CD para infraestrutura.

## Prompt — Copie daqui ↓

```
Leia estes arquivos:
1. AI-INSTRUCTIONS.md
2. openspec/config.yaml
3. openspec/specs/ci-cd/spec.md
4. openspec/specs/iac/spec.md
5. openspec/specs/security/spec.md

Preciso configurar um pipeline CI/CD para: [DESCRIÇÃO]

Plataforma: [GitHub Actions / Azure DevOps / GitLab CI]
Ferramenta IaC: [Terraform / Ansible / ambos]
Cloud: [Azure / AWS / GCP]

O pipeline DEVE incluir os seguintes stages:

1. **Lint & Format**
   - terraform fmt --check
   - tflint

2. **Validate**
   - terraform validate
   - terraform init (com backend)

3. **Security Scan**
   - tfsec ou checkov
   - Bloquear se houver findings críticos

4. **Plan**
   - terraform plan
   - Output do plan como comentário na PR

5. **Apply (staging)**
   - terraform apply (auto-approve em staging)
   - Validação automatizada pós-deploy

6. **Apply (production)**
   - Requer aprovação manual
   - terraform apply com plan salvo
   - Health check pós-deploy
   - Rollback automático se falhar

Siga o fluxo OpenSpec:
1. /opsx:new [NOME-DO-PIPELINE]
2. /opsx:ff
3. Mostre-me antes de implementar
4. /opsx:apply
5. /opsx:verify
6. /opsx:archive

Regras:
- Secrets injetados do vault (nunca no código)
- Versions pinadas para todas as actions/plugins
- Logs sanitizados (sem secrets)
```

## Exemplos:

```
# Pipeline Terraform com GitHub Actions
[NOME-DO-PIPELINE] = setup-pipeline-terraform-gha
[DESCRIÇÃO] = "Pipeline para Terraform com GitHub Actions: lint, validate,
tfsec, plan na PR, apply com aprovação"

# Pipeline Azure DevOps
[NOME-DO-PIPELINE] = setup-pipeline-azdo
[DESCRIÇÃO] = "Pipeline Azure DevOps com stages de validação, 
security scan e deploy multi-ambiente"
```
