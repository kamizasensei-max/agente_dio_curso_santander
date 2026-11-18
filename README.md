# Agente_dio_curso_santander

mkdir -p C:\Users\User\Desktop\dio-agent-main\dio-agent-main\skills\vulnerability-analysis
# Guia Prático: Evoluindo o DIO Agent com Especialização em Cibersegurança

Vou te guiar passo a passo para montar esse projeto. Como você tem o repositório base em `C:\Users\User\Desktop\dio-agent-main\dio-agent-main`, vamos trabalhar a partir daí.

---

## 1. Estrutura do Projeto

Primeiro, vamos entender a estrutura base do DIO Agent:

```
dio-agent/
├── AGENTS.md
├── agent/
│   ├── persona.md
│   └── knowledge/
│       └── (arquivos de conhecimento existentes)
├── skills/
│   └── (skills existentes)
├── README.md
└── (outros arquivos do harness)
```

---

## 2. Criando a Skill de Análise de Vulnerabilidades

### 2.1 Criar a pasta da skill

```bash
mkdir -p C:\Users\User\Desktop\dio-agent-main\dio-agent-main\skills\vulnerability-analysis
```

### 2.2 Criar o `SKILL.md`

Este é o coração da especialização. O agente segue este roteiro quando é acionado.

```markdown
---
name: vulnerability-analysis
description: Analisa vulnerabilidades de segurança em código, infraestrutura e configurações, avaliando impacto, probabilidade e severidade, e orientando correções defensivas.
tags:
  - security
  - vulnerability
  - appsec
  - infrastructure
  - threat-modeling
triggers:
  - "analise essa vulnerabilidade"
  - "tem alguma falha de seguranca?"
  - "review de seguranca"
  - "vulnerability"
  - "seguranca do codigo"
  - "threat model"
---

# Vulnerability Analysis Skill

Você é um analista de segurança sênior. Quando o usuário apresentar código, configuração, arquitetura ou cenário, conduza a análise como um profissional de AppSec/InfraSec faria em uma revisão técnica.

## Regras de Comportamento

1. **Nunca responda de forma genérica.** Sempre contextualize com o cenário específico do usuário.
2. **Pergunte antes de prescrever.** Se faltar contexto (SO, framework, versão, tráfego esperado, ameaças principais), faça perguntas de esclarecimento antes de dar a análise completa.
3. **Seja específico no código.** Cite linhas, funções e variáveis quando analisar código.
4. **Use a tríade CID como lente.** Toda análise deve mapear para Confidencialidade, Integridade e Disponibilidade.
5. **Classifique com severidade.** Use uma escala CVSS-adjacente: Critical / High / Medium / Low / Informational.
6. **Forneça a correção, não só o diagnóstico.** Sempre que apontar uma falha, mostre o código/configuração corrigido.
7. **Cite referências do MITRE ATT&CK** quando aplicável (técnicas e táticas).
8. **Mapeie para a OWASP Top 10** quando for vulnerabilidade de aplicação web.

## Estrutura de Resposta

Sempre que realizar uma análise completa, siga este formato:

### 1. Contexto Entendido
- Resuma o que foi apresentado (tipo de sistema, linguagem, arquitetura).
- Liste suposições feitas.

### 2. Perguntas de Contexto (se necessário)
- Se faltar informação crítica, liste as perguntas antes de prosseguir.
- Exemplos: "Qual a versão do framework?", "O sistema está exposto publicamente?", "Há autenticação?"

### 3. Vulnerabilidades Identificadas

Para cada vulnerabilidade:

| Campo | Descrição |
|-------|-----------|
| **Nome** | Nome técnico (ex: SQL Injection, XSS, Path Traversal) |
| **Severidade** | Critical / High / Medium / Low / Informational |
| **Categoría OWASP** | (se aplicável) |
| **MITRE ATT&CK** | Técnica(s) mapeada(s) (ex: T1190 - Exploit Public-Facing Application) |
| **Impacto (CID)** | Como afeta Confidencialidade, Integridade, Disponibilidade |
| **Probabilidade** | Alta / Média / Baixa (baseada em exposição e complexidade de exploração) |
| **Descrição** | Explicação técnica do porquê é vulnerável |
| **Exploração** | Como um atacante exploraria (passo a passo) |
| **Correção** | Código/configuração corrigido com explicação |
| **Boas Práticas** | Prevenção futura |

### 4. Resumo de Risco
- Tabela resumo: vulnerabilidade, severidade, impacto principal.
- Recomendação prioritária: o que corrigir primeiro.

### 5. Checklist de Validação
- Itens para o usuário verificar após a correção.

## Regras Defensivas (Não Sugira Ativamente, a Menos que Pedido)

- **Nunca sugira comandos destrutivos** sem qualificar o impacto.
- **Sempre considere o contexto de produção** vs. laboratório.
- **Valide se a correção introduz novas vulnerabilidades** (ex: parameterizar SQL mas esquecer validação de input).
- **Pense em cadeias de ataque:** uma vulnerabilidade pode habilitar outra.

## Exemplos de Acionamento

- "Analise este código Python de autenticação"
- "Esta configuração de Nginx tem problemas de segurança?"
- "Me ajude a fazer threat modeling deste microsserviço"
- "Review de segurança deste pipeline CI/CD"

## Tom e Estilo

- Técnico mas acessível.
- Direto ao ponto, sem enrolação.
- Use analogias quando ajudar a entender conceitos complexos.
- Em português, com termos técnicos em inglês quando for o padrão da indústria.
```

### 2.3 Criar o `README.md` da skill

```markdown
# Vulnerability Analysis Skill

## O que faz

Esta skill transforma o DIO Agent em um analista de segurança sênior. Em vez de dar respostas genéricas sobre segurança, o agente:

1. **Entende o contexto** antes de prescrever
2. **Classifica vulnerabilidades** com severidade, impacto e probabilidade
3. **Mapeia para frameworks** (MITRE ATT&CK, OWASP Top 10, CID)
4. **Fornece correções práticas** com código
5. **Valida que a correção não introduz novas falhas**

## Quando o agente usa esta skill

- Quando você apresentar código e pedir análise de segurança
- Quando descrever uma arquitetura e pedir threat modeling
- Quando mostrar uma configuração e pedir review de segurança
- Quando perguntar sobre vulnerabilidades específicas

## Estrutura de saída

Cada análise segue um formato padronizado com:
- Contexto entendido
- Vulnerabilidades detalhadas (tabela)
- Resumo de risco
- Checklist de validação

## Exemplo de uso

```
Você: Analise este código de login:

app.py:
def login(username, password):
    query = f"SELECT * FROM users WHERE username='{username}' AND password='{password}'"
    return db.execute(query)

Agente: [executa a skill de vulnerability-analysis]
```

## Arquivos

- `SKILL.md` - Roteiro que o agente segue
- `README.md` - Este arquivo (explicação para humanos)
```

---

## 3. Criando o Arquivo de Conhecimento

### 3.1 Criar `agent/knowledge/security-basics.md`

```markdown
# Fundamentos de Segurança

## Conceitos Centrais

### Vulnerabilidade
Uma fraqueza em um sistema que pode ser explorada por uma ameaça. É o "o quê" está errado.

### Ameaça
Uma ação ou evento potencialmente indesejado que explora uma vulnerabilidade. É o "quem/quê" vai explorar.

### Risco
A combinação de probabilidade de uma ameaça explorar uma vulnerabilidade e o impacto resultante.

**Risco = Probabilidade × Impacto**

### Exposição
O grau em que um sistema é acessível por atores não autorizados. Um serviço em localhost tem exposição diferente de um em 0.0.0.0:80.

## Tríade CID (Confidencialidade, Integridade, Disponibilidade)

### Confidencialidade (C)
Garantir que informações não sejam acessadas por não autorizados.
- Exemplos de falha: dados expostos em logs, senha em texto puro, acesso sem autenticação
- Controles: criptografia, RBAC, mascaramento de dados, redes segmentadas

### Integridade (I)
Garantir que dados não sejam alterados de forma não autorizada.
- Exemplos de falha: tampering com input, modificação de arquivos do sistema, MITM
- Controles: checksums, assinaturas digitais, WAF, validação de input

### Disponibilidade (D)
Garantir que o sistema esteja acessível quando necessário.
- Exemplos de falha: DDoS, resource exhaustion, configuração incorreta
- Controles: rate limiting, auto-scaling, redundância, failover

## Frameworks de Referência

### OWASP Top 10 (2021)
1. Broken Access Control
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable and Outdated Components
7. Identification and Authentication Failures
8. Software and Data Integrity Failures
9. Security Logging and Monitoring Failures
10. Server-Side Request Forgery (SSRF)

### MITRE ATT&CK
Framework de técnicas de ataque observadas no campo. Organizado em:
- **Táticas** (objetivos do atacante): Initial Access, Execution, Persistence, Privilege Escalation, Defense Evasion, Credential Access, Discovery, Lateral Movement, Collection, Command and Control, Exfiltration, Impact
- **Técnicas** (métodos específicos): T1190 (Exploit Public-Facing Application), T1059 (Command and Scripting Interpreter), T1552 (Unsecured Credentials), etc.

### CVSS (Common Vulnerability Scoring System)
Escala de 0 a 10 para classificar severidade:
- 0.0-3.9: Low
- 4.0-6.9: Medium
- 7.0-8.9: High
- 9.0-10.0: Critical

## Boas Práticas Fundamentais

### Input Handling
- **Sempre valide e sanitize input**
- Use consultas parametrizadas (nunca concatene strings em SQL)
- Whitelist > Blacklist para validação
- Assuma que todo input é hostil

### Autenticação e Autorização
- MFA para sistemas críticos
- Senhas: mínimo 12 caracteres, use gerenciador de senhas
- Hash com algoritmos modernos (bcrypt, Argon2)
- Implemente RBAC (Role-Based Access Control)
- Least privilege: cada componente tem só o acesso necessário

### Criptografia
- TLS 1.2+ para transporte
- AES-256 para dados em repouso
- Nunca implemente criptografia custom
- Rote de chaves regularmente

### Logging e Monitoramento
- Logue eventos de segurança (login, autorização, mudanças de config)
- Não logue dados sensíveis
- Alertas para padrões anômalos
- Retenção de logs para forensia

### Segredos e Configuração
- Nunca commite segredos no repositório
- Use secret managers (Vault, AWS Secrets Manager)
- Variáveis de ambiente para configuração
- Hardening de imagens base

### DevSecOps
- Security shifts left: segurança desde o design
- SAST (Static Application Security Testing)
- DAST (Dynamic Application Security Testing)
- SCA (Software Composition Analysis)
- Pipeline com gates de segurança
- Threat modeling antes de implementação

## Padrões de Vulnerabilidade Comuns

### Injeção (SQL, Command, LDAP, XPath)
- Causa: concatenação de input não confiável em comandos
- Sinais: f-strings em SQL, eval(), exec(), shell=True
- Correção: parametrização, escopo controlado, allowlists

### XSS (Cross-Site Scripting)
- Causa: output não escapado em HTML/JS
- Sinais: innerHTML, document.write, template strings sem escape
- Correção: context-aware output encoding, CSP

### Path Traversal
- Causa: input não validado em operações de arquivo
- Sinais: ../, ..\, absPath sem normalização
- Correção: canonicalização, chroot, allowlist de caminhos

### SSRF (Server-Side Request Forgery)
- Causa: URL não validada em requisições do servidor
- Sinais: fetch/requests com URL do usuário, metadata endpoints
- Correção: allowlist de hosts, bloqueio de IPs privados, proxy

### Deserialização Insegura
- Causa: deserializar dados não confiáveis
- Sinais: pickle.loads, unserialize, YAML.load (sem SafeLoader)
- Correção: formatos seguros (JSON), validação de esquema, sandbox

### Race Conditions
- Causa: operações não atômicas em estado compartilhado
- Sinais: TOCTOU (time-of-check-to-time-of-use), check-then-act
- Correção: locks, transações, operações atômicas

### Buffer Overflow
- Causa: escrita além dos limites de memória
- Sinais: C/C++ sem bounds checking, sprintf sem limites
- Correção: safe libraries, ASLR, stack canaries, sanitizers

## Análise de Impacto

### Como avaliar impacto
1. **O que o atacante pode fazer?** (escopo)
2. **O que ele precisa para fazer?** (pré-condições)
3. **Quanto dano causa?** (impacto no CID)
4. **Quão difícil é explorar?** (complexidade)
5. **Quanto tempo até ser detectado?** (detecção)

### Matriz de Probabilidade
- **Alta:** explorável remotamente, sem autenticação, input simples
- **Média:** requer autenticação ou input específico
- **Baixa:** requer acesso local, timing específico, ou múltiplas condições

## Comunicação de Achados

### Como escrever um finding
1. **Título claro:** "SQL Injection em endpoint /api/login permite acesso a todos os dados"
2. **Severidade com justificativa:** "High: expõe PII de todos os usuários"
3. **Reprodução passo a passo:** como um atacante exploraria
4. **Impacto:** o que acontece se explorado
5. **Recomendação:** correção específica com código
6. **Validação:** como confirmar que foi corrigido

### O que NÃO fazer em um finding
- Não seja vago: "há problemas de segurança"
- Não assuma contexto: pergunte se não sabe
- Não ignore a severidade: um bug low não é o mesmo que critical
- Não dê apenas o diagnóstico: forneça a solução
```

---

## 4. Atualizando a Persona

### 4.1 Adicionar trecho ao `agent/persona.md`

Abra o arquivo `agent/persona.md` e adicione no final (ou em uma seção de especializações):

```markdown
## Especialização: Análise de Vulnerabilidades

Quando o usuário apresentar código, configuração, arquitetura ou cenário de segurança, você age como um analista de segurança sênior. Isso significa:

- **Pergunte antes de prescrever.** Se faltar contexto (SO, framework, versão, exposição, ameaças), faça perguntas de esclarecimento.
- **Use a tríade CID como lente.** Toda análise mapeia para Confidencialidade, Integridade e Disponibilidade.
- **Classifique com severidade.** Critical / High / Medium / Low / Informational, com justificativa.
- **Mapeie para frameworks.** OWASP Top 10 para web, MITRE ATT&CK para técnicas de ataque.
- **Forneça correção, não só diagnóstico.** Mostre o código/configuração corrigido.
- **Valide que a correção não introduz novas falhas.** Pense em cadeias de ataque.
- **Seja específico.** Cite linhas, funções, variáveis. Nada de respostas genéricas.

Quando o usuário pedir uma análise de segurança, use a skill `vulnerability-analysis`.
```

---

## 5. Atualizando o `AGENTS.md`

### 5.1 Adicionar linha na tabela de skills

Abra `AGENTS.md` e na tabela de skills, adicione:

```markdown
| vulnerability-analysis | Analisa vulnerabilidades em código, infraestrutura e configurações. Avalia impacto, probabilidade e severidade. Orienta correções defensivas com código. | security, appsec, threat-modeling, infrastructure |
```

---

## 6. Testando com Código Vulnerável

### 6.1 Criar `app.py` de teste

```python
# app.py - Login vulnerável para teste
from flask import Flask, request, jsonify

app = Flask(__name__)

# Simulação de banco de dados (em produção seria um DB real)
USERS_DB = {
    "admin": "secure_password_123",
    "alice": "alice_pass_456",
    "bob": "bob_secret_789",
}

@app.route("/login", methods=["POST"])
def login():
    data = request.get_json()
    username = data.get("username", "")
    password = data.get("password", "")
    
    # VULNERABILIDADE: SQL Injection via string concatenation
    # Em produção, isso seria:
    # query = f"SELECT * FROM users WHERE username='{username}' AND password='{password}'"
    # results = db.execute(query)
    
    # Simulando a vulnerabilidade para demonstração
    query = f"SELECT * FROM users WHERE username='{username}' AND password='{password}'"
    
    # Em produção, retornaria os resultados da query
    # Aqui simulamos a lógica
    if username in USERS

## Atribuição

Projeto de origem: [DIO Agent](https://github.com/digitalinnovationone/dio-agent).
