# Security Group vs NACL

## Comparação

| | Security Group | NACL |
|---|---|---|
| **Nível** | Instância (EC2, RDS) | Subnet |
| **Estado** | **Stateful** — resposta automática | **Stateless** — liberar entrada E saída |
| **Regras de negação** | ❌ Não (só permite) | ✅ Sim (permite e nega) |
| **Ordem das regras** | Avalia todas | Avalia em ordem numérica, para na primeira |
| **Padrão** | Nega tudo de entrada, permite tudo de saída | NACL padrão permite tudo |

---

## Fluxo de tráfego

```
Tráfego entrando na instância:  Internet → NACL (subnet) → Security Group (instância)
Tráfego saindo da instância:    Instância → Security Group → NACL → Internet
```

Não é "prioridade" de um sobre o outro — são **filtros em série**. O tráfego precisa passar pelos DOIS.

---

## Stateful vs Stateless na prática

**Security Group (Stateful):**
- Se você libera entrada na porta 80 → a resposta sai automaticamente
- Não precisa criar regra de saída para o tráfego de retorno

**NACL (Stateless):**
- Se você libera entrada na porta 80 → PRECISA criar regra de saída nas portas efêmeras (1024-65535)
- Sem regra de saída = resposta não volta para o cliente

---

## Quando usar cada um na prova

| Cenário | Solução |
|---|---|
| Restringir tráfego por porta/protocolo | **Security Group** (99% dos casos) |
| Bloquear um IP específico | **NACL** (único que tem DENY) |
| "Negar acesso de um range de IPs" | **NACL** |
| "Permitir apenas porta 443 para a instância" | **Security Group** |

---

## Regra de ouro

A NACL padrão da VPC já permite tudo (entrada e saída). Na prova, só mexe na NACL quando precisa **bloquear** algo explicitamente. Para todo o resto, **Security Group** resolve.

---

*Domínio 1 — Design de Arquiteturas Seguras (30%)*
