# Prompt 03 — Revisar Segurança da Infraestrutura

> **Quando usar:** Para checar se o código de infra segue as regras de segurança.
> Use periodicamente e SEMPRE antes de terraform apply em produção.

## Prompt — Copie daqui ↓

```
Leia estes arquivos obrigatórios:
1. AI-INSTRUCTIONS.md
2. openspec/config.yaml
3. openspec/specs/security/spec.md
4. openspec/specs/iac/spec.md
5. openspec/specs/networking/spec.md
6. openspec/specs/ci-cd/spec.md

Faça uma revisão de segurança COMPLETA da infraestrutura deste projeto.

Verifique CADA item abaixo:

## Terraform / IaC
- [ ] Remote backend configurado (não state local)
- [ ] State locking habilitado
- [ ] .tfstate e .tfvars no .gitignore
- [ ] Providers com versão pinada
- [ ] Modules com versão pinada
- [ ] Sem credenciais hardcoded
- [ ] Variables com type constraints

## Identidade e Acesso (IAM)
- [ ] MFA obrigatório para admins
- [ ] Least privilege em todos os roles
- [ ] Service principals com escopo mínimo
- [ ] Credenciais com data de expiração
- [ ] Rotação de secrets configurada

## Rede
- [ ] Sem firewall rules "any any allow"
- [ ] Segmentação de rede implementada
- [ ] TLS 1.2+ em toda comunicação
- [ ] VPN com criptografia forte (IKEv2/WireGuard)
- [ ] NSGs/Security Groups restritivos

## Secrets
- [ ] Todos os secrets no vault (Key Vault, etc.)
- [ ] Secrets com expiração definida
- [ ] Pipeline injeta secrets em runtime (não hardcoded)
- [ ] Logs sanitizados (sem secrets impressos)

## Pipelines
- [ ] Stages incluem lint, validate, scan
- [ ] Production deploy requer aprovação manual
- [ ] Dependencies com versão pinada
- [ ] Access control configurado

Para cada problema, informe:
1. O que está errado
2. Qual o risco (o que pode acontecer)  
3. Gravidade (CRÍTICO / ALTO / MÉDIO / BAIXO)
4. Como corrigir (com exemplo)
```
