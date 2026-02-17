# Prompt 02 — Criar Módulo Terraform

> **Quando usar:** Sempre que precisar criar um novo módulo Terraform.

## Prompt — Copie daqui ↓

```
Antes de começar, leia:
1. AI-INSTRUCTIONS.md
2. openspec/config.yaml
3. openspec/project.md
4. openspec/specs/iac/spec.md
5. openspec/specs/security/spec.md

Preciso criar um módulo Terraform para: [DESCRIÇÃO DO MÓDULO]

Siga este fluxo:

1. /opsx:new [NOME-DO-MODULO]
2. /opsx:ff
3. Mostre-me o design.md e o tasks.md ANTES de implementar
4. Depois que eu aprovar, execute /opsx:apply
5. Execute /opsx:verify
6. Se tudo OK, execute /opsx:archive

O módulo DEVE seguir a estrutura padrão:
- main.tf (recursos)
- variables.tf (variáveis com type e description)
- outputs.tf (outputs documentados)
- README.md (documentação do módulo)

Lembre-se:
- Remote backend configurado
- Sem credenciais hardcoded
- Variables com type constraints
- terraform fmt + validate + tflint
```

## Exemplos:

```
# VNet Hub-Spoke
[NOME-DO-MODULO] = criar-modulo-vnet-hub-spoke
[DESCRIÇÃO] = "Módulo para criar topologia hub-spoke com VNet Hub 
(firewall, VPN gateway) e VNets Spoke com peering"

# Resource Group com tags
[NOME-DO-MODULO] = criar-modulo-resource-group  
[DESCRIÇÃO] = "Módulo para criar Resource Groups com tags obrigatórias
(environment, owner, project, cost-center)"

# Storage Account para backup
[NOME-DO-MODULO] = criar-modulo-storage-backup
[DESCRIÇÃO] = "Módulo para Storage Account com criptografia, lifecycle
policies e acesso restrito por rede"

# AKS Cluster
[NOME-DO-MODULO] = criar-modulo-aks
[DESCRIÇÃO] = "Módulo para cluster AKS com node pools, RBAC, network
policies e integração com Key Vault"
```
