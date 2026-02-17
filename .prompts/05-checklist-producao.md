# Prompt 05 — Checklist Pré-Produção

> **Quando usar:** ANTES de rodar `terraform apply` em produção.
> Funciona como um "Pre-flight Check" antes de aplicar mudanças.

## Prompt — Copie daqui ↓

```
Leia estes arquivos:
1. AI-INSTRUCTIONS.md
2. openspec/config.yaml
3. openspec/project.md
4. Todos os specs em openspec/specs/

Analise TODAS as mudanças pendentes neste projeto e gere um checklist 
de pré-produção COMPLETO.

## IaC (Terraform)
- [ ] `terraform fmt` sem alterações
- [ ] `terraform validate` sem erros
- [ ] `tflint` sem warnings/errors
- [ ] `tfsec`/`checkov` sem findings críticos
- [ ] `terraform plan` revisado — nenhuma destruição inesperada
- [ ] Remote backend configurado e acessível
- [ ] State lock funcionando
- [ ] .tfstate NÃO está no Git
- [ ] Providers com versão pinada
- [ ] Mudanças testadas em staging/dev primeiro
- [ ] Plan de staging vs production comparados

## Segurança
- [ ] Sem credenciais hardcoded em nenhum arquivo
- [ ] Secrets no vault (Key Vault / HashiCorp Vault / etc)
- [ ] IAM roles com least privilege
- [ ] MFA habilitado para acesso admin
- [ ] Network Security Groups / firewall rules revisados
  - [ ] Sem regras "any any allow"
  - [ ] Sem portas desnecessárias abertas para 0.0.0.0/0
- [ ] TLS 1.2+ configurado
- [ ] Certificates válidos (não auto-signed em prod)

## Rede
- [ ] DNS configurado corretamente
- [ ] VPN health check ativo (se aplicável)
- [ ] Failover testado (se HA configurado)
- [ ] Network diagram atualizado
- [ ] IPAM atualizado

## Monitoring
- [ ] Métricas configuradas (CPU, memória, disco, rede)
- [ ] Alertas configurados com thresholds
- [ ] Dashboard atualizado
- [ ] Log forwarding funcionando
- [ ] Budget alerts configurados (FinOps)

## CI/CD
- [ ] Pipeline testado em staging
- [ ] Production deploy com aprovação manual
- [ ] Rollback strategy definida e testada
- [ ] Health checks pós-deploy configurados

## Backup e DR
- [ ] Backup automatizado configurado
- [ ] Backup criptografado
- [ ] Recovery testado (último teste: ______)
- [ ] RTO/RPO documentados

## Documentação
- [ ] README dos módulos atualizado
- [ ] Network diagram atualizado
- [ ] Runbook de operação atualizado
- [ ] Change request / RFC documentado
- [ ] Audit trail completo (quem, o quê, quando, por quê)

Para cada item verificado, informe o status:
✅ Conforme | ❌ Não conforme (com ação corretiva) | ⚠️ Parcial | N/A
```
