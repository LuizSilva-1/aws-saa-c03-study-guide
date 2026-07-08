# Auto Scaling e Availability Zones

## Regra fundamental

Use pelo menos **3 Availability Zones** para garantir capacidade se uma cair.

| Config | Se uma AZ cai... |
|---|---|
| 2 AZs | Toda capacidade concentra em 1 AZ (risco de sobrecarga) |
| 3 AZs | Capacidade distribuída entre 2 AZs restantes (mais seguro) |

---

## Auto Scaling Group (ASG)

- Distribui instâncias entre as AZs configuradas automaticamente
- Mantém número mínimo/desejado de instâncias saudáveis
- Se uma instância falha ou AZ cai → lança novas nas AZs saudáveis

---

## Arquitetura de alta disponibilidade padrão

```
Internet
    ↓
ALB (distribui entre AZs)
    ↓
Auto Scaling Group
    ├→ AZ-a: EC2, EC2
    ├→ AZ-b: EC2, EC2
    └→ AZ-c: EC2, EC2
    ↓
RDS Multi-AZ (failover automático entre AZs)
```

---

## Para a prova

| Cenário | Solução |
|---|---|
| "Continuar funcionando se uma AZ cair" | ASG com **múltiplas AZs** + ALB |
| "Menor esforço operacional para HA" | ASG + ALB + RDS Multi-AZ |
| "Escalar automaticamente com a demanda" | ASG com scaling policy (target tracking) |

---

*Domínio 2 — Design de Arquiteturas Resilientes (26%)*
