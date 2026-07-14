# RDS Custom — Quando acesso ao SO é necessário

## O problema

O **Amazon RDS** gerencia tudo automaticamente — patches, backups, failover. Mas em troca, você **não tem acesso ao sistema operacional** nem ao banco de dados em nível baixo.

Para bancos que precisam de **personalizações avançadas** (Oracle, SQL Server com extensões específicas), isso é um problema.

---

## A solução: RDS Custom

**Amazon RDS Custom** é o meio-termo entre EC2 (controle total, overhead total) e RDS (gerenciado, sem acesso ao SO).

| | EC2 com Oracle | RDS for Oracle | RDS Custom for Oracle |
|---|---|---|---|
| **Acesso ao SO** | ✅ Total | ❌ Nenhum | ✅ Sim |
| **Gerenciamento AWS** | ❌ Você faz tudo | ✅ AWS faz tudo | ✅ AWS faz o básico |
| **Overhead operacional** | 🔴 Alto | 🟢 Baixo | 🟡 Médio |
| **Personalizações avançadas** | ✅ Sim | ❌ Não | ✅ Sim |
| **Patching, backups** | Você | AWS | AWS |

---

## Recuperação de desastres com RDS Custom

Para DR cross-region com RDS Custom → **Read Replica em outra região**

```
Região primária:          Região DR:
RDS Custom (primário) --> Read Replica
                          (pode ser promovida em caso de desastre)
```

---

## Palavras-chave na prova

| Palavra-chave | Solução |
|---|---|
| "acesso ao SO" + "banco gerenciado" | **RDS Custom** |
| "personalização avançada" + "Oracle/SQL Server" | **RDS Custom** |
| "menor overhead" + "sem acesso ao SO" | **RDS for Oracle** |
| "controle total" + "alto overhead" | EC2 com Oracle |
| "DR cross-region" + RDS | Read Replica em outra região |

---

*Domínio 2 — Design de Arquiteturas Resilientes (26%)*
