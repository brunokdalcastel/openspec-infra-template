# openspec-infra-template

Template para projetos de **infraestrutura com AI (vibe coding para infra)** usando OpenSpec CLI.
Foco em **IaC, segurança e boas práticas** — feito por e para profissionais de infraestrutura.

> **Diferente do template de desenvolvimento (vibedev):** Este template foi criado especificamente
> para quem trabalha com **Terraform, Ansible, pipelines CI/CD, redes e monitoramento**.
> As specs aqui cobrem o que você já conhece — mas garantem que o AI faça certo.

---

## 📋 Índice

- [O que é isso?](#-o-que-é-isso)
- [Pré-requisitos](#-pré-requisitos)
- [Quick Start — Passo a Passo](#-quick-start--passo-a-passo)
- [Como o OpenSpec funciona](#-como-o-openspec-funciona)
- [Comandos — Referência completa](#-comandos--referência-completa)
- [Fluxos de trabalho](#-fluxos-de-trabalho)
- [Instruções para a IA](#-instruções-para-a-ia)
- [Prompts prontos para usar](#-prompts-prontos-para-usar)
- [O que vem incluso](#-o-que-vem-incluso)
- [Cloud e ferramentas agnósticos](#-cloud-e-ferramentas-agnósticos)
- [Por que segurança é tão enfatizada?](#-por-que-segurança-é-tão-enfatizada)
- [Dicas para economizar sessões do Claude Code](#-dicas-para-economizar-sessões-do-claude-code)
- [Glossário](#-glossário)
- [Estrutura de pastas](#-estrutura-de-pastas)

---

## 🤔 O que é isso?

**OpenSpec** é uma ferramenta CLI que organiza o trabalho entre você e o AI.

### Por que usar para infra?

Você já sabe escrever Terraform, configurar pipelines e gerenciar redes. O problema é que quando pede para o AI gerar código de infra, ele:
- ❌ Cria módulos Terraform sem estrutura padrão
- ❌ Faz state local (em vez de remote backend)
- ❌ Deixa credenciais hardcoded no código
- ❌ Cria regras de firewall genéricas ("any any allow")
- ❌ Ignora pipelines CI/CD e testes
- ❌ Não documenta nada

Com o OpenSpec, o AI **lê suas specs antes de gerar qualquer coisa** e segue as regras de infraestrutura que você definiu.

### Analogia direta:

| O que você já conhece | Equivalente no OpenSpec |
|---|---|
| **Runbook / Playbook** | As **specs** — regras que o AI segue |
| **Change Request / RFC** | O comando **`/opsx:new`** — cria uma "solicitação de mudança" |
| **Plano de implementação** | Os **artifacts** (proposal, design, tasks) |
| **Pre-flight checklist** | O comando **`/opsx:verify`** — valida se tudo foi feito |
| **Post-mortem / documentação** | O comando **`/opsx:archive`** — documenta e finaliza |
| **GitOps / PR review** | O fluxo inteiro — tudo é versionado e revisável |

---

## 📦 Pré-requisitos

### 1. Node.js 20.19+

```bash
# Verificar se já tem instalado:
node --version

# Se não tiver, baixe em: https://nodejs.org
# Escolha a versão LTS (Long Term Support)
```

### 2. OpenSpec CLI

```bash
npm install -g @fission-ai/openspec@latest
```

### 3. Ferramentas de IaC (conforme o que você usa)

```bash
# Terraform
terraform --version

# Ansible
ansible --version

# Azure CLI (se usar Azure)
az --version

# AWS CLI (se usar AWS)
aws --version
```

### 4. Um AI coding assistant

O OpenSpec funciona com:
- **Claude Code** (recomendado)
- **Cursor**
- **Windsurf**
- **GitHub Copilot Chat**
- **Codex CLI**

> **Nota:** Para infra, use modelos de alto raciocínio (Claude Opus, GPT 4o). Eles entendem melhor conceitos de rede, segurança e IaC.

---

## 🚀 Quick Start — Passo a Passo

### Passo 1: Clone o template

```bash
git clone https://github.com/brunokdalcastel/openspec-infra-template.git meu-projeto-infra
cd meu-projeto-infra
```

> **O que isso faz:** Baixa uma cópia do template para a sua máquina.

### Passo 2: Remove o histórico do template e inicia o seu

```bash
# No Linux/Mac:
rm -rf .git

# No Windows (PowerShell):
Remove-Item -Recurse -Force .git

# Depois, em qualquer OS:
git init
```

> **O que isso faz:** Inicia um repositório Git limpo para o SEU projeto.

### Passo 3: Inicializa o OpenSpec

```bash
openspec init --tools claude
```

Opções de tools:
- `claude` — para Claude Code
- `cursor` — para Cursor
- `claude,cursor` — para usar ambos
- Execute `openspec init --help` para ver todas as opções

> **O que isso faz:** Configura o AI para entender os comandos `/opsx:`.

### Passo 4: Personalize o contexto do projeto

Abra o Claude Code (ou seu AI) e digite:

```
Leia o arquivo openspec/project.md e me ajude a preencher com os detalhes deste projeto.
Minha infra é baseada em [Azure/AWS/on-prem/híbrida].
Uso [Terraform/Ansible/ambos].
```

> **Isso ECONOMIZA sessões futuras** porque o AI já vai saber o contexto da sua infra.

### Passo 5: Comece a trabalhar!

No Claude Code, digite:

```
/opsx:new criar-vnet-hub-spoke
```

E siga o fluxo:

```
/opsx:ff        ← Gera todos os documentos de planejamento
/opsx:apply     ← Implementa o código Terraform/Ansible
/opsx:verify    ← Verifica se tudo está correto
/opsx:archive   ← Finaliza e documenta
```

---

## 🔄 Como o OpenSpec funciona

### O ciclo de vida de uma mudança de infra:

```
    ┌─────────────────────────────────────────────────────────┐
    │                    FLUXO DO OPENSPEC                     │
    │                                                          │
    │   1. CRIAR MUDANÇA ─────── /opsx:new nome-da-mudanca    │
    │          │                                                │
    │          ▼                                                │
    │   2. PLANEJAR ──────────── /opsx:ff (gera tudo de vez)  │
    │      │                     ou /opsx:continue (passo a    │
    │      │                        passo)                     │
    │      │                                                   │
    │      │  Gera automaticamente:                            │
    │      │  ✓ proposal.md  → O que + por quê                │
    │      │  ✓ specs/       → Requisitos detalhados           │
    │      │  ✓ design.md    → Arquitetura e decisões          │
    │      │  ✓ tasks.md     → Checklist de tarefas            │
    │      │                                                   │
    │          ▼                                                │
    │   3. IMPLEMENTAR ───────── /opsx:apply                   │
    │      │  O AI escreve Terraform, Ansible, pipelines...    │
    │      │                                                   │
    │          ▼                                                │
    │   4. VERIFICAR ─────────── /opsx:verify (opcional)       │
    │      │  Valida se o código bate com as specs             │
    │      │                                                   │
    │          ▼                                                │
    │   5. FINALIZAR ─────────── /opsx:archive                 │
    │      Documenta e organiza. Pronto para a próxima.        │
    └─────────────────────────────────────────────────────────┘
```

### Onde as coisas ficam:

```
openspec/
├── config.yaml          ← Regras que o AI é OBRIGADO a seguir
├── project.md           ← Contexto do projeto (cloud, ferramentas)
├── specs/               ← Fonte da verdade (como a infra funciona)
│   ├── iac/spec.md           ← Terraform, Ansible, GitOps
│   ├── ci-cd/spec.md         ← Pipelines, deploys, artifacts
│   ├── monitoring/spec.md    ← Métricas, logs, SLA, FinOps
│   ├── networking/spec.md    ← Redes, VPN, DNS, HA
│   └── security/spec.md      ← IAM, secrets, firewall, audit
└── changes/             ← Mudanças em andamento
    ├── criar-vnet/      ← Cada mudança de infra vira uma pasta
    └── archive/         ← Mudanças finalizadas ficam aqui
```

---

## 📖 Comandos — Referência completa

### Tabela resumo

| Comando | O que faz | Quando usar |
|---------|-----------|-------------|
| `/opsx:explore` | Investigar ideias sem compromisso | Quando não sabe por onde começar |
| `/opsx:new` | Criar uma nova mudança | Início de feature/fix/refactor |
| `/opsx:continue` | Criar próximo artefato (um de cada vez) | Quando quer revisar cada passo |
| `/opsx:ff` | Criar TODOS os artefatos de planejamento | Quando sabe o que quer e quer ir rápido |
| `/opsx:apply` | Implementar as tarefas (escrever código) | Depois do planejamento |
| `/opsx:verify` | Validar que implementação bate com o plano | Antes de finalizar |
| `/opsx:sync` | Mesclar specs da mudança no principal | Raramente necessário (archive faz isso) |
| `/opsx:archive` | Finalizar e documentar a mudança | Quando a mudança está pronta |
| `/opsx:bulk-archive` | Finalizar várias mudanças de uma vez | Quando acumulou mudanças prontas |
| `/opsx:onboard` | Tutorial guiado do OpenSpec | Primeira vez usando |

### Detalhamento com exemplos de INFRA

#### `/opsx:explore` — Investigar antes de agir

```
Você: /opsx:explore

AI:   O que você gostaria de explorar?

Você: Preciso redesenhar a rede para hub-spoke. 
      Como está a topologia atual?

AI:   Deixa eu analisar o Terraform atual...
      [Analisa os arquivos .tf]
      
      Encontrei:
      - 1 VNet com 3 subnets (tudo flat, sem segmentação)
      - NSGs com regras "allow all" em 2 subnets
      - Sem VPN configurada
      
      Recomendo migrar para hub-spoke com:
      1. VNet Hub (firewall + VPN)
      2. VNet Spoke-APP (aplicações)
      3. VNet Spoke-DB (banco de dados)
      
      Quer criar uma mudança para isso?

Você: Sim!

AI:   Execute /opsx:new redesign-rede-hub-spoke
```

#### `/opsx:new` — Criar uma mudança

```bash
# Exemplos para infra:
/opsx:new criar-vnet-hub-spoke
/opsx:new adicionar-monitoring-grafana
/opsx:new migrar-terraform-backend-remoto
/opsx:new configurar-pipeline-deploy
/opsx:new hardening-nsg-firewall
/opsx:new setup-vpn-site-to-site
```

> **Boas práticas de nomes:**  
> ✅ `criar-vnet-hub-spoke`, `fix-nsg-regras`, `add-monitoring-alertas`  
> ❌ `update`, `infra`, `changes`, `wip`

#### `/opsx:ff` — Gera tudo de vez

```
Você: /opsx:ff

AI:   Gerando artefatos de planejamento...
      ✓ proposal.md  — migrar para hub-spoke, justificativa
      ✓ specs/       — requisitos de rede, segurança, monitoramento
      ✓ design.md    — diagrama da topologia, peering, NSGs
      ✓ tasks.md     — checklist de implementação
      
      Pronto para implementar! Execute /opsx:apply.
```

#### `/opsx:apply` — Implementar

```
Você: /opsx:apply

AI:   Implementando redesign-rede-hub-spoke...
      
      Lendo tasks.md:
      - [ ] 1.1 Criar módulo VNet Hub
      - [ ] 1.2 Criar módulo VNet Spoke
      - [ ] 1.3 Configurar VNet Peering
      - [ ] 1.4 Criar NSGs com regras segmentadas
      - [ ] 2.1 Configurar Azure Firewall no Hub
      - [ ] 2.2 Criar UDRs (rotas)
      
      Trabalhando no 1.1...
      [Cria modules/vnet-hub/main.tf, variables.tf, outputs.tf]
      ✓ 1.1 Concluído
      ...
```

#### `/opsx:verify` — Verificar

```
Você: /opsx:verify

AI:   Verificando redesign-rede-hub-spoke...
      
      COMPLETUDE
      ✓ Todas as 8 tarefas concluídas
      ✓ Módulos Terraform com estrutura padrão
      ⚠ Falta README.md no módulo vnet-spoke
      
      CORREÇÃO
      ✓ Remote backend configurado
      ✓ Variables com tipos e descrições
      ✓ NSGs sem regras "any any"
      
      COERÊNCIA
      ✓ Segue convenções de naming
      ⚠ Outputs do hub não incluem firewall private IP
      
      RESULTADO: Pronto para archive (com avisos)
```

#### `/opsx:archive` — Finalizar

```
Você: /opsx:archive

AI:   ✓ Specs sincronizadas com openspec/specs/networking/
      ✓ Movido para archive/2025-01-24-redesign-rede-hub-spoke/
      Mudança arquivada com sucesso!
```

---

## 🔀 Fluxos de trabalho

### Fluxo 1: Mudança rápida (o mais comum)

```
/opsx:new nome  →  /opsx:ff  →  /opsx:apply  →  /opsx:verify  →  /opsx:archive
```

**Exemplo:** Adicionar monitoring com alertas

```
/opsx:new add-monitoring-alertas
/opsx:ff
/opsx:apply
/opsx:verify
/opsx:archive
```

### Fluxo 2: Exploratório (investigar antes)

```
/opsx:explore  →  /opsx:new  →  /opsx:continue  →  (revisar)  →  /opsx:apply
```

**Exemplo:** Redesenhar a rede sem saber exatamente como

### Fluxo 3: Mudanças paralelas

```
Você: /opsx:new add-vpn-site-to-site
Você: /opsx:ff
Você: /opsx:apply
[... pausa para resolver um incidente ...]

Você: /opsx:new fix-nsg-regra-exposta
Você: /opsx:ff
Você: /opsx:apply
Você: /opsx:archive

[... volta para a VPN ...]
Você: /opsx:apply add-vpn-site-to-site
```

### Quando usar `/opsx:ff` vs `/opsx:continue`?

| Situação | Use |
|----------|-----|
| Mudança simples, sabe o que quer | `/opsx:ff` |
| Mudança complexa, quer revisar cada passo | `/opsx:continue` |
| Pressão de tempo | `/opsx:ff` |
| Primeira vez usando OpenSpec | `/opsx:continue` |
| Redesign de arquitetura | `/opsx:continue` |

---

## 🤖 Instruções para a IA

Ao iniciar qualquer sessão no Claude Code, **cole isso primeiro:**

```
Antes de qualquer coisa:
1. Leia o arquivo AI-INSTRUCTIONS.md na raiz do projeto
2. Leia o arquivo openspec/config.yaml
3. Leia o arquivo openspec/project.md

Essas são as regras obrigatórias deste projeto de infraestrutura.
Nunca pule regras de segurança, mesmo "para testar".
Sempre explique o que está fazendo em termos que um profissional de infra entende.
```

Veja o arquivo `AI-INSTRUCTIONS.md` para mais detalhes.

---

## 📝 Prompts prontos para usar

A pasta `.prompts/` contém prompts que você pode copiar e colar no AI.

| Arquivo | Quando usar |
|---------|-------------|
| `01-inicio-projeto.md` | Primeira configuração do projeto |
| `02-criar-modulo-terraform.md` | Criar módulo Terraform novo |
| `03-revisar-seguranca-infra.md` | Revisar segurança da infraestrutura |
| `04-configurar-pipeline.md` | Criar/configurar pipeline CI/CD |
| `05-checklist-producao.md` | Antes de aplicar em produção |

### Exemplo de uso:

1. Abra `.prompts/02-criar-modulo-terraform.md`
2. Copie o conteúdo
3. Cole no Claude Code
4. Substitua os `[PLACEHOLDERS]`
5. O AI gera o módulo seguindo todas as specs

---

## 📦 O que vem incluso

Cada spec é um "runbook" para uma área de infraestrutura:

| Spec | Cobre |
|------|-------|
| `iac` | Terraform (módulos, state, import), Ansible, GitOps |
| `ci-cd` | Pipelines, deploy strategies (blue/green, canary), supply chain |
| `monitoring` | Métricas, alertas, logs, SLA tracking, FinOps (custo cloud) |
| `networking` | Segmentação, VPN, DNS, HA/failover, bandwidth |
| `security` | IAM/Zero Trust, secrets, firewall, vulnerabilidades, audit |

> **Esses são os assuntos que você já domina.** A diferença é que agora o AI também é obrigado a seguir essas regras quando gera código.

---

## ☁️ Cloud e ferramentas agnósticos

Os specs NÃO forçam um cloud ou ferramenta específica. Funcionam com:

- **Cloud**: Azure, AWS, GCP, on-premises, híbrido
- **IaC**: Terraform, OpenTofu, Pulumi, CloudFormation, Bicep
- **Config Management**: Ansible, Puppet, Chef, Salt
- **CI/CD**: GitHub Actions, Azure DevOps, GitLab CI, Jenkins
- **Monitoring**: Grafana, Zabbix, Prometheus, Datadog, Azure Monitor
- **Networking**: pfSense, MikroTik, Fortinet, cloud-native (NSG, Security Groups)
- **Secrets**: Azure Key Vault, HashiCorp Vault, AWS Secrets Manager

Edite `openspec/project.md` para definir o que você usa.

---

## 🔒 Por que segurança é tão enfatizada?

Quando o AI gera código de infraestrutura sem guardrails, ele vai:
- ❌ Criar VMs com porta 22 aberta para `0.0.0.0/0`
- ❌ Usar credenciais hardcoded no Terraform
- ❌ Fazer state local sem locking
- ❌ Criar regras de firewall "any any allow"
- ❌ Ignorar MFA e JIT para acesso administrativo
- ❌ Não configurar rotação de secrets
- ❌ Pular logs de auditoria

Este template resolve isso com regras no `config.yaml` que o AI é **obrigado** a seguir e specs que cobrem **Zero Trust, defense-in-depth e least privilege** de forma prática.

---

## 💡 Dicas para economizar sessões do Claude Code

### 1. SEMPRE comece com contexto

```
Leia AI-INSTRUCTIONS.md, openspec/config.yaml e openspec/project.md antes de começar.
```

### 2. Use `/opsx:ff` para mudanças que você já planejou

O `/opsx:ff` faz tudo de uma vez. O `/opsx:continue` gasta mais tokens.

### 3. Seja específico

```
# ❌ Ruim (vago):
"Configura a rede"

# ✅ Bom (específico):
"/opsx:new criar-vnet-hub-spoke"
"/opsx:ff"
"/opsx:apply"
```

### 4. Uma mudança por sessão

```
Sessão 1: /opsx:new criar-vnet → /opsx:ff → /opsx:apply → /opsx:archive
Sessão 2: /opsx:new add-vpn → /opsx:ff → /opsx:apply → /opsx:archive
```

### 5. Use `/opsx:onboard` na primeira vez

```
/opsx:onboard
```

### 6. Limpe o contexto antes de implementar

Limpe a janela de contexto antes de rodar `/opsx:apply`.

---

## 📚 Glossário

| Termo | O que significa |
|-------|-----------------|
| **Spec** | Especificação — documento com regras que o AI deve seguir |
| **Change** | Mudança proposta na infra. Cada mudança vira uma pasta |
| **Artifact** | Documento de uma mudança (proposal, design, tasks, specs) |
| **Delta spec** | Spec que descreve apenas as MUDANÇAS (adicionado/modificado/removido) |
| **Proposal** | Documento que explica O QUE vai ser feito e POR QUÊ |
| **Design** | Documento que explica COMO vai ser feito (diagrama, decisões) |
| **Tasks** | Checklist de tarefas para a implementação |
| **Archive** | Processo de finalizar uma mudança e documentar |
| **Guardrail** | Regra que o AI é obrigado a seguir |
| **IaC** | Infrastructure as Code — infra definida em código (Terraform, etc) |
| **GitOps** | Prática onde o Git é a fonte da verdade para deploys de infra |
| **SDD** | Spec-Driven Development — desenvolvimento guiado por especificações |
| **Source of truth** | Pasta `openspec/specs/` — a verdade sobre como a infra funciona |
| **FinOps** | Práticas para otimizar custos de cloud |

---

## 📁 Estrutura de pastas

```
meu-projeto-infra/
├── AI-INSTRUCTIONS.md       ← Instruções que o AI lê automaticamente
├── README.md                ← Este arquivo
├── .gitignore
├── .prompts/                ← Prompts prontos para copiar e colar
│   ├── 01-inicio-projeto.md
│   ├── 02-criar-modulo-terraform.md
│   ├── 03-revisar-seguranca-infra.md
│   ├── 04-configurar-pipeline.md
│   └── 05-checklist-producao.md
└── openspec/
    ├── config.yaml          ← Regras obrigatórias para o AI
    ├── project.md           ← Contexto do projeto (preencher!)
    └── specs/               ← Specs de infraestrutura
        ├── iac/spec.md           ← Terraform, Ansible, GitOps
        ├── ci-cd/spec.md         ← Pipelines e deploys
        ├── monitoring/spec.md    ← Métricas, logs, FinOps
        ├── networking/spec.md    ← Redes, VPN, DNS, HA
        └── security/spec.md      ← IAM, secrets, firewall, audit
```

---

## 📄 Licença

MIT
