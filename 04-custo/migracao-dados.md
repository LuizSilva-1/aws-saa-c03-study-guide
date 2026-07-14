# Migração de Dados — Snowball, DataSync e Direct Connect

## O problema: como mover grandes volumes de dados para a AWS?

A resposta depende de dois fatores:
1. **Volume de dados**
2. **Largura de banda disponível**

---

## Calculando o tempo de transferência

Fórmula rápida para a prova:

```
Tempo = Volume ÷ Banda

Exemplo: 700 TB ÷ 500 Mbps
= 700 × 1.000.000 MB ÷ 500 Mbps
≈ 128 dias ← inviável em 1 mês
```

Quando o cálculo mostrar semanas ou meses → **Snowball**

---

## Comparação de serviços de migração

| Serviço | Quando usar | Limitação |
|---|---|---|
| **AWS Snowball** | Centenas de TB, prazo curto, banda limitada | Físico — leva alguns dias para envio |
| **AWS DataSync** | Dezenas de TB, automatização, transferência contínua | Limitado pela banda disponível |
| **Direct Connect** | Conexão privada dedicada, uso recorrente | Caro para migração única |
| **VPN + CLI** | Poucos GB, baixa frequência | Lento e inseguro para grandes volumes |
| **S3 Transfer Acceleration** | Upload de objetos via edge locations | Não resolve grandes migrações únicas |

---

## Snowball — detalhes importantes

- Dispositivo físico enviado pela AWS, você carrega os dados e devolve
- Capacidade: **80 TB** por dispositivo (pode pedir múltiplos)
- Dados criptografados automaticamente
- Após receber os dados, AWS faz upload para S3
- Depois disso, aplicar **Lifecycle Policy** para mover para Glacier Deep Archive

```
Data center → Snowball → AWS → S3 → Lifecycle → Glacier Deep Archive
```

---

## Glacier Deep Archive — para retenção longa

| Classe S3 | Acesso | Custo | Quando usar |
|---|---|---|---|
| **Glacier Deep Archive** | Até 12h | Mais barato | Dados raramente acessados, retenção regulatória longa |
| **Glacier Flexible** | 1-5 min a horas | Barato | Acesso esporádico |
| **Glacier Instant** | Milissegundos | Moderado | Acesso raro mas imediato |

**Lifecycle Policy:** regra automática que move objetos entre classes após X dias.

---

## Palavras-chave na prova

| Cenário | Solução |
|---|---|
| "centenas de TB + prazo curto + banda limitada" | **AWS Snowball** |
| "transferência automatizada contínua" | **AWS DataSync** |
| "conexão privada recorrente" | **Direct Connect** |
| "armazenar por 7 anos + acesso raramente" | **S3 Glacier Deep Archive** |
| "mover dados automaticamente entre classes" | **S3 Lifecycle Policy** |

---

*Domínio 4 — Design de Arquiteturas Econômicas (20%)*
