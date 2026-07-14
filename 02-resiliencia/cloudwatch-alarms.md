# CloudWatch Alarms — Composite Alarms

## Tipos de alarme no CloudWatch

| Tipo | O que faz | Quando usar |
|---|---|---|
| **Metric Alarm** | Monitora UMA métrica | Condição simples (ex: CPU > 80%) |
| **Composite Alarm** | Combina MÚLTIPLOS alarmes com AND/OR | Múltiplas condições, reduzir falsos alarmes |

---

## Composite Alarm — por que importa

**Problema:** Alarme de CPU dispara em picos curtos e irrelevantes → falso alarme → equipe responde desnecessariamente.

**Solução:** Composite Alarm só dispara quando **todas as condições** são verdadeiras ao mesmo tempo.

```
Composite Alarm = Alarm_CPU AND Alarm_IOPS

Só dispara quando:
  CPU > 50%    ← true
  AND
  IOPS > X     ← true
```

---

## Pegadinha do exame

**Dashboard CloudWatch** ≠ alarme. Dashboard mostra métricas visualmente mas **não gera alerta automático**.

Quando o cenário disser "responder rapidamente" + "alertar automaticamente" → precisa de **alarme**, não dashboard.

---

## Palavras-chave na prova

| Palavra-chave | Solução |
|---|---|
| "múltiplas condições para disparar" | **Composite Alarm** |
| "reduzir falsos alarmes" | **Composite Alarm** |
| "CPU AND disco ao mesmo tempo" | **Composite Alarm** |
| "visualizar métricas" | CloudWatch Dashboard |
| "alertar automaticamente" | CloudWatch Alarm (metric ou composite) |
| "monitorar endpoint / fluxo de usuário" | CloudWatch Synthetics (canário) |

---

## CloudWatch Synthetics (canário) — o que NÃO é

Canários do CloudWatch Synthetics monitoram **endpoints e fluxos de usuário simulados** (ex: "consegue fazer login?", "a API responde?"). **Não monitora métricas de infraestrutura** como CPU ou IOPS.

> Regra: se o cenário falar em **métricas** (CPU, IOPS, memória) → Alarm. Se falar em **disponibilidade de endpoint** → Synthetics.

---

*Domínio 2 — Design de Arquiteturas Resilientes (26%)*
