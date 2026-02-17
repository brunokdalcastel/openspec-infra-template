# AI Instructions — Leia ANTES de qualquer tarefa

> **Este arquivo é obrigatório.** O AI DEVE ler este arquivo no início de cada sessão.

## Contexto do Projeto

1. Leia `openspec/project.md` para entender o stack e o contexto da infraestrutura
2. Leia `openspec/config.yaml` para as regras obrigatórias
3. Consulte os specs em `openspec/specs/` antes de criar/modificar código de infra

## Regras que NUNCA podem ser quebradas

1. **Segurança não é opcional.** Todas as regras em `openspec/config.yaml` são não-negociáveis
2. **Nunca desabilite segurança** — nem "para testar", nem "temporariamente"
3. **Nunca hardcode credenciais** — use vault e variáveis de ambiente
4. **Sempre remote backend** — Terraform state NUNCA fica local
5. **Sempre least privilege** — IAM com permissões mínimas necessárias
6. **Nunca "any any allow"** — regras de firewall sempre específicas
7. **Sempre testes antes de produção** — staging/dev primeiro
8. **Sempre pin de versão** — providers, modules, imagens

## Perfil do usuário

- O usuário é **profissional de infraestrutura** (Cloud Engineer / SysAdmin / DevOps)
- Ele **já conhece** Terraform, Ansible, redes, cloud — mas quer que o AI gere código de qualidade
- **Explique decisões de arquitetura** em termos claros
- **Não simplifique demais** — ele entende conceitos de infra
- As explicações devem ser em **português**

## Fluxo de trabalho esperado

1. Ao receber um pedido de infra, use o fluxo OpenSpec:
   - `/opsx:new` → `/opsx:ff` → `/opsx:apply` → `/opsx:verify` → `/opsx:archive`
2. Nunca pule o planejamento — sempre gere proposal, specs, design e tasks antes de escrever código
3. Ao gerar Terraform: sempre inclua `terraform fmt`, `terraform validate`, `tflint` e `tfsec`

## Referência dos specs

Os specs em `openspec/specs/` são a **fonte da verdade**. Antes de gerar código, consulte:

| Área | Spec | O que contém |
|------|------|-------------|
| IaC | `openspec/specs/iac/spec.md` | Terraform, Ansible, GitOps, import de recursos |
| CI/CD | `openspec/specs/ci-cd/spec.md` | Pipelines, deploy strategies, supply chain |
| Monitoring | `openspec/specs/monitoring/spec.md` | Métricas, alertas, logs, SLA, FinOps |
| Networking | `openspec/specs/networking/spec.md` | Segmentação, VPN, DNS, HA, failover |
| Security | `openspec/specs/security/spec.md` | IAM, secrets, firewall, CVEs, audit |

## Checklist obrigatório para Terraform

Antes de finalizar qualquer mudança Terraform, verifique:

- [ ] `terraform fmt` sem alterações
- [ ] `terraform validate` sem erros
- [ ] `tflint` sem warnings/errors
- [ ] `tfsec`/`checkov` sem findings críticos
- [ ] Remote backend configurado
- [ ] Variables com type e description
- [ ] Outputs documentados
- [ ] README.md do módulo atualizado
