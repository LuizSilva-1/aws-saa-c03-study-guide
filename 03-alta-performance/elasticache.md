# ElastiCache — quando usar e quando NÃO usar

## O que é

Serviço de cache in-memory (Redis ou Memcached) que armazena dados frequentemente acessados para reduzir latência e carga no banco.

---

## Quando USAR ✅

| Cenário | Por quê |
|---|---|
| Mesma query repetida milhares de vezes | Cache do resultado evita hit no banco |
| Session store para aplicação web | Sessões acessadas a cada request |
| Leaderboard de gaming (Redis Sorted Sets) | Ranking lido constantemente |
| Dados de referência que mudam raramente | Cachear tabela de preços, catálogo |

## Quando NÃO usar ❌

| Cenário | Usar em vez disso |
|---|---|
| Relatórios de analytics com parâmetros variáveis | **Read Replica** |
| Queries com filtros diferentes a cada execução | **Read Replica** |
| Dados que mudam a cada segundo | Não faz sentido cachear |
| Primeira execução de query pesada (cache miss) | Não resolve o problema inicial |

---

## Regra para a prova

```
Queries REPETITIVAS e IDÊNTICAS acessadas milhares de vezes?
  → ElastiCache ✅

Queries de ANALYTICS com parâmetros variáveis (filtros, datas)?
  → Read Replica ✅ (não cache)
```

---

## Redis vs Memcached

| | Redis | Memcached |
|---|---|---|
| Persistência | ✅ Sim | ❌ Não |
| Replicação | ✅ Sim (Multi-AZ) | ❌ Não |
| Estruturas de dados | Sorted sets, lists, hashes | Apenas key-value simples |
| Pub/Sub | ✅ Sim | ❌ Não |
| Na prova | Resposta padrão (mais features) | Só se pedir "cache simples multi-thread" |

---

*Domínio 3 — Design de Arquiteturas de Alta Performance (24%)*
