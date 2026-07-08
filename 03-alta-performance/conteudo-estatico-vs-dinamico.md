# Conteúdo Estático vs Dinâmico

## Conceito-chave

"Estático" na AWS **não significa que nunca muda**. Significa que cada request recebe a **mesma resposta por um período** (TTL).

---

## Quando tratar como estático (usar S3 + CloudFront)

| Cenário | TTL sugerido | Por quê |
|---|---|---|
| Site de notícias atualizado a cada 30 min | 30 min | Todos leem o mesmo artigo |
| Vídeos de curso atualizados semanalmente | 7 dias | Conteúdo não muda por semana |
| Imagens de produto | Longo (dias) | Raramente mudam |
| Landing page de marketing | Horas | Atualizada poucas vezes ao dia |

## Quando NÃO tratar como estático (usar compute)

| Cenário | Por quê |
|---|---|
| Dashboard com dados em tempo real por usuário | Cada usuário vê dados diferentes |
| API que retorna resultado de cálculo personalizado | Resposta diferente a cada request |
| Chat em tempo real | Dados mudam a cada segundo |

---

## Regra para a prova

```
Conteúdo lido por muitos + atualizado com pouca frequência?
  → S3 + CloudFront (TTL = intervalo de atualização)
  → Menor custo, maior escalabilidade, zero infra

Cada usuário recebe resposta diferente em tempo real?
  → Compute (Lambda, Fargate, EC2)
```

---

## Por que S3 + CloudFront ganha em custo

| Aspecto | S3 + CloudFront | EC2/Fargate |
|---|---|---|
| Custo base | Centavos por GB | Instâncias rodando 24/7 |
| Escala | Automática (milhões de requests) | Precisa de Auto Scaling |
| Manutenção | Zero | Patches, deploys, monitoring |
| Picos de tráfego | CloudFront absorve | Precisa provisionar capacidade |

---

*Domínio 3 — Design de Arquiteturas de Alta Performance (24%)*
