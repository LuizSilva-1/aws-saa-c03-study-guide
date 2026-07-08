# CloudTrail vs Config vs GuardDuty

## Os 3 serviços de monitoramento

| Serviço | Pergunta que responde | Analogia |
|---|---|---|
| **CloudTrail** | Quem fez o quê e quando? | Câmera de segurança (gravação) |
| **AWS Config** | Meus recursos estão em conformidade? | Inspetor verificando se tudo segue as regras |
| **GuardDuty** | Tem algo suspeito acontecendo? | Alarme inteligente que detecta invasores |

---

## CloudTrail

**O que faz:** Registra TODA chamada de API na conta AWS.

**Mostra:** Quem (user/role), o quê (ação), quando (timestamp), de onde (IP de origem).

**Exemplo:** "User luiz.silva deletou a instância i-12345 às 15:03 de 08/07/2026 do IP 189.x.x.x"

**Na prova:** "auditoria", "quem fez", "registro de atividades", "investigar quem deletou"

---

## AWS Config

**O que faz:** Monitora continuamente se os recursos seguem as REGRAS que você define.

**Exemplos de regras:**
- "Todos os buckets devem ter criptografia habilitada"
- "Nenhum Security Group pode ter porta 22 aberta para 0.0.0.0/0"
- "Todas as instâncias devem ter tags obrigatórias"

**Na prova:** "conformidade", "compliance", "avaliar configuração", "detectar recurso fora da regra"

**Config vs CloudTrail:**
- CloudTrail = "quem fez" (registro de ações passadas)
- Config = "está certo?" (avaliação contínua do estado atual)

---

## GuardDuty

**O que faz:** Detecta ameaças e comportamentos anômalos AUTOMATICAMENTE usando ML.

**Analisa:**
- Logs do CloudTrail (chamadas de API suspeitas)
- VPC Flow Logs (tráfego de rede anômalo)
- DNS Logs (comunicação com IPs maliciosos)

**Exemplos de alertas:**
- Login de um país onde a empresa não opera
- EC2 se comunicando com IP de botnet conhecida
- Acesso em massa ao S3 de forma não usual
- Escalação de privilégios suspeita

**Na prova:** "detecção de ameaças", "comportamento anômalo", "login suspeito", "atividade maliciosa"

---

## Resumo — decisão rápida

| Palavra-chave | Serviço |
|---|---|
| "auditoria", "quem deletou", "registro" | **CloudTrail** |
| "conformidade", "compliance", "regras de config" | **AWS Config** |
| "ameaça", "anômalo", "suspeito", "intrusão" | **GuardDuty** |
| "impedir ação em toda a organização" | **SCP** (prevenção, não detecção) |

---

## Atenção: detecção vs prevenção

| Tipo | Serviços |
|---|---|
| **Detecção** (avisar que algo errado aconteceu) | GuardDuty, CloudTrail, Config |
| **Prevenção** (impedir que aconteça) | SCP, WAF, Security Group, NACL, Bucket Policy |

GuardDuty **detecta** mas não **bloqueia**. Para ação automática baseada em alertas do GuardDuty → combina com EventBridge + Lambda.

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
