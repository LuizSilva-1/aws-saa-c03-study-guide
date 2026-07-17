# S3 — Ferramentas de Visibilidade (Storage Lens, Analytics, Inventory)

## O problema

A AWS tem vários serviços que "olham" para o S3, e os nomes parecem fazer a mesma coisa. Não fazem.

---

## Mapa de cada ferramenta

| Serviço | Pergunta que responde | Escopo |
|---|---|---|
| **S3 Storage Lens** | "Como está o uso do meu S3 no geral?" | Toda organização (multi-conta, multi-região) |
| **S3 Analytics** | "Devo mudar a classe de storage desse bucket?" | 1 bucket específico |
| **S3 Inventory** | "Quais objetos existem nesse bucket?" (lista) | 1 bucket específico |
| **AWS Config** | "Meus buckets estão configurados corretamente?" | Conformidade de configuração |

---

## Analogia do escritório

- **Storage Lens** = relatório financeiro da empresa toda ("quanto cada departamento gasta com papel")
- **Analytics** = análise de 1 armário específico ("esse armário é acessado todo dia ou posso mandar pro depósito?")
- **Inventory** = inventário do armário ("lista tudo que tem dentro com data e tamanho")
- **Config** = auditoria ("esse armário está trancado como deveria?")

---

## S3 Storage Lens — detalhes

### O que mostra
- Bytes armazenados por conta/região/bucket
- **Uploads multipart incompletos** (custo oculto)
- Objetos não acessados há X dias
- Tendências de crescimento
- Recomendações de otimização

### Quando usar
| Cenário | Solução |
|---|---|
| "Relatório de uso S3 em múltiplas contas" | **Storage Lens** |
| "Uploads multipart incompletos" | **Storage Lens** |
| "Visibilidade centralizada de armazenamento" | **Storage Lens** |
| "Otimização de custos de S3 na organização" | **Storage Lens** |

### O que NÃO serve
| Opção errada | Por quê |
|---|---|
| S3 Multi-Region Access Point | Acesso unificado a buckets, não gera relatórios |
| AWS Config | Conformidade de configuração, não métricas de uso |
| SCP | Restrição de permissões, não monitoramento |

---

## S3 Analytics — Storage Class Analysis

### O que faz
Analisa o **padrão de acesso** dos objetos de 1 bucket ao longo de 30+ dias e recomenda qual classe de armazenamento seria mais econômica.

### Quando usar
| Cenário | Solução |
|---|---|
| "Qual classe de storage para esse bucket?" | **S3 Analytics** |
| "Padrão de acesso do último mês" | **S3 Analytics** |
| "Devo mover para IA ou Glacier?" | **S3 Analytics** |

---

## S3 Inventory

### O que faz
Gera um relatório (CSV, ORC ou Parquet) listando **todos os objetos** de um bucket com metadados: tamanho, data, classe de storage, criptografia, etc.

### Quando usar
| Cenário | Solução |
|---|---|
| "Listar todos os objetos do bucket" | **S3 Inventory** |
| "Relatório de objetos sem criptografia" | **S3 Inventory** |
| "Exportar lista de objetos para análise" | **S3 Inventory** |

---

## Dica complementar: limpar uploads multipart incompletos

Storage Lens **identifica** o problema. Para **resolver**, use uma Lifecycle Rule:

```json
{
  "Rules": [{
    "ID": "AbortIncompleteMultipartUpload",
    "Status": "Enabled",
    "AbortIncompleteMultipartUpload": {
      "DaysAfterInitiation": 7
    }
  }]
}
```

Storage Lens (identifica) → Lifecycle Rule (limpa). Dois serviços nativos, zero código.

---

## Decisão rápida na prova

```
"Visibilidade/relatório de uso em TODA organização"     → Storage Lens
"Recomendar classe para 1 bucket"                       → S3 Analytics
"Listar objetos de 1 bucket"                            → S3 Inventory
"Bucket está em conformidade com regras?"               → AWS Config
```

---

*Domínio 4 — Design de Arquiteturas Econômicas (20%)*
