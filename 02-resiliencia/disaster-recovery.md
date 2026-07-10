# Disaster Recovery (DR) — Estratégias

## As 4 estratégias (do mais barato ao mais caro)

| Estratégia | RTO | RPO | Custo | O que tem na região DR |
|---|---|---|---|---|
| **Backup & Restore** | Horas | Horas | $ | Nada rodando (só backups no S3) |
| **Pilot Light** | 15-30 min | Minutos | $$ | Dados replicados + AMIs prontas (sem compute) |
| **Warm Standby** | Minutos | Minutos | $$$ | Infra MÍNIMA rodando (EC2 pequeno + Read Replica) |
| **Multi-Site Active-Active** | ~0 (instantâneo) | ~0 | $$$$ | Infra COMPLETA rodando (2x custo) |

---

## Definições

| Termo | Significado |
|---|---|
| **RTO** (Recovery Time Objective) | Quanto tempo pode ficar fora? (tempo máximo de indisponibilidade) |
| **RPO** (Recovery Point Objective) | Quantos dados pode perder? (tempo desde o último backup/replicação) |

---

## Detalhes de cada estratégia

### Backup & Restore ($)
- Backups automáticos do RDS + snapshots de EBS replicados para outra região
- Em caso de desastre: restaura do backup, lança instâncias do zero
- **RTO:** Horas (tempo de restaurar + configurar + testar)
- **RPO:** Depende da frequência do backup (1h, 6h, 24h)
- **Quando usar:** Dados não-críticos, pode ficar fora por horas

### Pilot Light ($$)
- Dados replicados continuamente (Read Replica cross-region, S3 CRR)
- AMIs/templates prontos mas NENHUMA instância rodando
- Em caso de desastre: lança EC2 das AMIs, promove Read Replica
- **RTO:** 15-30 min (tempo de lançar compute)
- **RPO:** Minutos (replicação contínua)
- **Quando usar:** RTO de ~30 min aceitável, quer economizar

### Warm Standby ($$$)
- Infra mínima JÁ RODANDO na região DR (EC2 pequeno, Read Replica ativa)
- Em caso de desastre: escala o EC2 (resize ou ASG), promove replica
- **RTO:** Minutos (só escalar, não lançar do zero)
- **RPO:** Minutos (Read Replica com lag baixo)
- **Quando usar:** RTO < 30 min necessário, aceita pagar por infra mínima

### Multi-Site Active-Active ($$$$)
- Infraestrutura COMPLETA em 2+ regiões, ambas recebendo tráfego
- Route 53 distribui entre regiões. Se uma cai, outra absorve tudo.
- **RTO:** ~0 (failover DNS automático)
- **RPO:** ~0 (escritas em ambas regiões)
- **Quando usar:** Zero downtime obrigatório, custo não é prioridade

---

## Palavras-chave na prova

| Cenário | Estratégia |
|---|---|
| "Menor custo" + "pode ficar fora por horas" | **Backup & Restore** |
| "RTO 30 min" + "menor custo possível" | **Pilot Light** |
| "RTO < 30 min" + "RPO minutos" + "custo moderado" | **Warm Standby** |
| "Zero downtime" + "failover instantâneo" | **Active-Active** |

---

## Componentes comuns de DR

| Componente | Papel no DR |
|---|---|
| **RDS Read Replica cross-region** | Dados replicados (RPO minutos) — promover em caso de desastre |
| **S3 Cross-Region Replication** | Backups/arquivos disponíveis na outra região |
| **AMIs copiadas para outra região** | Lançar EC2 rapidamente na região DR |
| **Route 53 failover routing** | DNS detecta região down e redireciona |
| **CloudFormation / Terraform** | Recriar infra rapidamente na região DR |

---

## ⚠️ LEMBRETE para a prova

- Multi-AZ ≠ DR. Multi-AZ protege contra falha de **AZ**. DR protege contra falha de **REGIÃO**.
- Read Replica cross-region = replicação assíncrona (RPO de minutos, não zero)
- Promover Read Replica = ela vira o novo banco primário (processo irreversível)

---

*Domínio 2 — Design de Arquiteturas Resilientes (26%)*
