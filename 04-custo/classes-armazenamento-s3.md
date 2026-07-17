# Classes de Armazenamento S3

## Comparação

| Classe | Acesso | Duração mínima | Recuperação | Quando usar |
|---|---|---|---|---|
| **Standard** | Frequente | — | Imediata | Dados ativos, acessados diariamente |
| **Standard-IA** | Infrequente | 30 dias | Imediata | Backups recentes, DR |
| **One Zone-IA** | Infrequente | 30 dias | Imediata | Dados reproduzíveis (thumbnails) |
| **Intelligent-Tiering** | Variável | — | Imediata | Padrão de acesso desconhecido |
| **Glacier Instant** | Raro | 90 dias | Milissegundos | Arquivos médicos, compliance |
| **Glacier Flexible** | Raro | 90 dias | Min a horas | Arquivos de longo prazo |
| **Glacier Deep Archive** | Raríssimo | 180 dias | Até 12 horas | Retenção regulatória (7-10 anos) |

---

## S3 Lifecycle — transições automáticas

Automatiza a movimentação de objetos entre classes baseado na idade:

```
Standard (dia 0)
    ↓ 30 dias
Standard-IA
    ↓ 90 dias
Glacier Flexible Retrieval
    ↓ 180 dias
Glacier Deep Archive
    ↓ 365 dias (opcional)
Delete
```

---

## Palavras-chave na prova

| Cenário | Classe |
|---|---|
| "Acessado frequentemente" | **Standard** |
| "Acessado raramente, mas precisa imediato quando acessa" | **Standard-IA** |
| "Dados podem ser recriados, acesso infrequente" | **One Zone-IA** |
| "Não sei o padrão de acesso" | **Intelligent-Tiering** |
| "Compliance, retenção obrigatória, acesso raro" | **Glacier** (tipo depende da velocidade de recuperação) |
| "Menor custo possível, posso esperar 12h" | **Deep Archive** |

---

## S3 Object Lock

Para dados que **não podem ser deletados** antes de um prazo:

| Modo | Quem pode deletar antes do prazo? |
|---|---|
| **Governance** | Usuários com permissão especial (`s3:BypassGovernanceRetention`) |
| **Compliance** | **Ninguém** — nem o root da conta, nem admin, nem AWS |

### Analogia

- **Governance** = "tranquei a porta mas o gerente tem a chave reserva"
- **Compliance** = "tranquei a porta e joguei a chave no mar — ninguém abre até o prazo"

### Decisão na prova

| Cenário | Modo |
|---|---|
| "Nenhum usuário pode modificar/excluir" | **Compliance** |
| "Regulatório", "HIPAA", "LGPD", "ensaio médico" | **Compliance** |
| "Proteção contra exclusão acidental, admin pode override" | **Governance** |

### Por que IAM Policy / Bucket Policy NÃO substitui Object Lock

Policies podem ser **alteradas por humanos** a qualquer momento. Um admin pode mudar a policy amanhã e liberar exclusão. Object Lock Compliance é uma **trava física** que nenhum humano pode desativar durante o período de retenção.

> **Regra:** "ninguém pode" = Compliance. Sem exceção.

---

## Custos — o que pagar atenção

- **IA/Glacier:** cobram taxa de retrieval (pagar para acessar)
- **Duração mínima:** deletar antes = pagar o período todo
- **One Zone-IA:** mais barato que Standard-IA, mas sem resiliência multi-AZ
- **Intelligent-Tiering:** cobra taxa de monitoramento por objeto

---

*Domínio 4 — Design de Arquiteturas Econômicas (20%)*
