# RDS Multi-AZ vs Read Replicas

## Comparação

| Aspecto | Multi-AZ | Read Replica |
|---|---|---|
| **Propósito** | Alta disponibilidade (HA) | Performance de leitura |
| **Replicação** | Síncrona | Assíncrona |
| **Failover** | Automático (1-2 min) | Manual (promoção) |
| **Perda de dados** | Zero | Possível (lag de replicação) |
| **Aceita queries?** | ❌ Standby NÃO aceita conexões | ✅ Sim (endpoint de leitura) |
| **Cross-Region** | ❌ Não (só outra AZ) | ✅ Sim |
| **Custo** | ~2x (standby sempre ligada) | Custo por réplica adicional |

---

## Palavras-chave na prova

| Se o cenário mencionar... | Resposta |
|---|---|
| "alta disponibilidade", "failover automático", "sem perda de dados", "AZ cair" | **Multi-AZ** |
| "relatórios pesados", "isolar carga de leitura", "analytics degradando", "BI" | **Read Replica** |
| "DR cross-region", "recuperação em outra região" | **Read Replica cross-region** (promoção manual) |
| "alta disponibilidade" + "relatórios" (os dois juntos) | **Multi-AZ + Read Replica** (são complementares) |
| "Read Replica na mesma AZ que o primário" | ❌ Não resolve resiliência — perde os dois se a AZ cair |
| "usar standby para leitura/BI" | ❌ Sempre errado — standby não aceita conexões |

---

## Conceitos críticos

### Relatórios = LEITURA

Relatórios, analytics, BI = operações SELECT = **leitura**, mesmo que consumam muita CPU para processar JOINs e agregações. Read Replicas resolvem isso isolando a carga de leitura do banco principal.

### Standby Multi-AZ não aceita conexões

A instância standby do Multi-AZ existe **apenas** para failover. Não aceita queries de leitura nem escrita. Qualquer opção na prova que sugira "direcionar leitura para o standby" está **ERRADA**.

### São complementares, não concorrentes

Você pode (e frequentemente deve) ter **ambos**:
- Multi-AZ → protege contra falha de AZ (HA)
- Read Replica → absorve carga de relatórios/BI (performance)

---

## Analogia

- **Multi-AZ** = copiloto pronto pra assumir se o piloto desmaiar (troca automática, passageiro nem percebe)
- **Read Replica** = guichê extra de atendimento só para consultas (alivia a fila do principal)

---

---

## Read Replica sob demanda (padrão de otimização de custo)

Na AWS, Read Replicas podem ser **criadas e deletadas a qualquer momento**. Isso é um padrão válido para uso esporádico:

```
Precisa do relatório (mensal/semanal):
  1. Cria Read Replica (~15-30 min)
  2. Roda queries pesadas na réplica
  3. Deleta Read Replica
  4. Paga $0 no restante do período
```

**Quando usar:**
- Relatórios mensais/semanais pesados
- Cenário pede "menor custo" + "uso esporádico"
- Não precisa da réplica 24/7

**Mentalidade AWS:** Recursos são descartáveis. Criar/deletar leva minutos. Os dados originais ficam seguros no primário — a réplica é uma cópia descartável.

| Frequência do uso | Manter réplica 24/7? |
|---|---|
| Diário ou contínuo | ✅ Sim (economiza tempo de criação) |
| Semanal ou mensal | ❌ Não (cria sob demanda = menor custo) |

---

## RDS Proxy — o que é e o que NÃO é

**O que faz:** Gerencia connection pooling — reutiliza conexões ao banco para não esgotar o limite.

**O que NÃO faz:**
- ❌ NÃO distribui queries para o standby Multi-AZ
- ❌ NÃO transforma standby em leitor
- ❌ NÃO substitui Read Replica para offload de leitura

**Quando usar:** Aplicações com muitas conexões curtas (Lambda com centenas de invocações simultâneas abrindo conexões ao RDS).

---

*Domínio 2 — Design de Arquiteturas Resilientes (26%)*
