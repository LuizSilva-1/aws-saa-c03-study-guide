# WAF e Shield — Proteção contra ataques

## WAF vs Shield — a diferença

| Serviço | Contra o quê | Camada | Analogia |
|---|---|---|---|
| **WAF** | SQL injection, XSS, bots, HTTP flood | Layer 7 (aplicação) | Segurança verificando conteúdo da mala |
| **Shield** | DDoS volumétrico (flood de tráfego bruto) | Layer 3/4 (rede) | Muro bloqueando multidão na porta |

---

## WAF (Web Application Firewall)

**O que faz:** Inspeciona o CONTEÚDO dos requests HTTP e bloqueia os maliciosos.

**Se associa a:** ALB, CloudFront, API Gateway

**Tipos de regras:**
- Bloquear SQL injection
- Bloquear XSS (cross-site scripting)
- Rate-based: bloquear IP que faz mais de X requests por segundo
- Geo-match: bloquear requests de países específicos
- IP reputation: bloquear IPs conhecidos como maliciosos

---

## Shield

| | Standard | Advanced |
|---|---|---|
| **Custo** | ✅ Gratuito (já ativo em toda conta) | $3.000/mês |
| **Protege** | DDoS Layer 3/4 | DDoS Layer 3/4 + Layer 7 |
| **Suporte** | Não | 24/7 DDoS Response Team (DRT) |
| **Proteção de custo** | Não | Sim (reembolsa custo extra do ataque) |

---

## Palavras-chave na prova

| Cenário | Serviço |
|---|---|
| "SQL injection", "XSS", "bots" | **WAF** |
| "Rate limiting", "muitos requests/segundo" | **WAF** (rate-based rules) |
| "DDoS" (sem detalhes extras) | **Shield Standard** (gratuito, já ativo) |
| "DDoS" + "suporte 24/7" ou "proteção de custo" | **Shield Advanced** |
| "Bloquear requests de um país" | **WAF** (geo-match) |
| "Menor custo" + "DDoS" | **Shield Standard** (já incluído, $0) |

---

## Arquitetura de proteção completa

```
Internet
    ↓
CloudFront (+ WAF rules)     ← bloqueia bots, injection, rate limit
    ↓
Shield Standard (automático)  ← absorve DDoS Layer 3/4
    ↓
ALB (+ WAF rules)            ← segunda camada de inspeção
    ↓
EC2 / Fargate (app)
```

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
