# Modelos de Compra EC2

## Modelos de compra EC2

## ⚠️ LEMBRETE — Savings Plans na prova

| Tipo de Savings Plan | Abrange | Flexibilidade |
|---|---|---|
| **EC2 Instance SP** | Só EC2 (família específica, região específica) | Baixa |
| **Compute SP** | EC2 + Fargate + Lambda | Total (tipo, tamanho, região, SO) |

Na prova, se pedir "flexibilidade" + "desconto" + "um único compromisso" → **Compute Savings Plans**.
Convertible RI é quase sempre distrator — Compute SP faz o mesmo com mais flexibilidade e mais desconto.

---

## Comparação

| Modelo | Desconto | Compromisso | Interrupção | Quando usar |
|---|---|---|---|---|
| **On-Demand** | 0% | Nenhum | ❌ Não | Imprevisível, testes, curto prazo |
| **Reserved (RI)** | 30-72% | 1 ou 3 anos | ❌ Não | Uso 24/7, tipo definido |
| **Savings Plans** | 30-72% | 1 ou 3 anos ($/hora) | ❌ Não | Uso 24/7, flexível em tipo/região |
| **Spot** | Até 90% | Nenhum | ✅ Sim (2 min aviso) | Tolerante a interrupção |
| **Dedicated Hosts** | Varia | Por host físico | ❌ Não | Licenciamento, compliance |

---

## Spot Instances — sinais na prova

Quando o cenário mencionar QUALQUER combinação destes → **Spot**:

- "pode ser interrompido" / "tolerante a falhas"
- "checkpoints" / "retoma de onde parou"
- "maior redução de custo" / "mais econômico possível"
- "batch" / "rendering" / "ML training"
- "processamento não-crítico" / "flexível no tempo"

**Spot Fleet:** Diversifica entre múltiplos tipos de instância para aumentar disponibilidade.

---

## Reserved vs Savings Plans

| | Reserved Instances | Savings Plans |
|---|---|---|
| Flexibilidade de tipo | ❌ Fixo (ex: m5.large) | ✅ Qualquer tipo (Compute SP) |
| Flexibilidade de região | ❌ Fixo (ou zonal ou regional) | ✅ Qualquer região (Compute SP) |
| Desconto | Ligeiramente maior | Ligeiramente menor |
| Na prova | "tipo específico + longo prazo" | "flexível + longo prazo" |

---

## Armadilhas na prova

| Cenário | Resposta ERRADA | Resposta CERTA |
|---|---|---|
| Workload roda só 3h/dia + tolerante a interrupção | Reserved (paga 24h) | **Spot** |
| Uso 24/7 previsível por 3 anos | Spot (pode ser interrompido) | **Reserved/Savings Plans** |
| Workload crítico + imprevisível | Spot (risco de interrupção) | **On-Demand** |

> **⚠️** Reserved/Savings Plans cobram por hora **contínua**. Se o workload não roda 24/7, pode ser mais caro que Spot ou On-Demand.

---

## Dica final

Na prova real, confie nos sinais. Se vir "pode interromper" + "menor custo" → **Spot**. Não mude de resposta sem motivo.

---

*Domínio 4 — Design de Arquiteturas Econômicas (20%)*
