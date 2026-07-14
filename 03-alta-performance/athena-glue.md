# Amazon Athena e AWS Glue

## Amazon Athena

**O que é:** Serviço de consulta SQL **serverless** que roda diretamente sobre dados armazenados no **Amazon S3**. Sem banco de dados para provisionar, sem servidor para gerenciar.

**Como funciona:**
```
Dados no S3 (CSV, JSON, Parquet, ORC...)
        ↓
    Amazon Athena
        ↓
   Resultado da query
```

**Quando usar:**
- "Consultar dados no S3 sem gerenciar banco"
- "Queries ad-hoc em data lake"
- "Menor overhead operacional" + dados em S3
- "Arquitetura serverless" + análise de dados

**Quando NÃO usar:**
- Dados transacionais de alta frequência → **RDS / Aurora / DynamoDB**
- Análise de grandes volumes históricos com queries complexas → **Redshift**

---

## AWS Glue — Job Bookmarks

**Problema sem Bookmarks:**
O job ETL do Glue reprocessa **todos os dados** a cada execução — incluindo dados já processados.

**Solução:**
Habilitar **Job Bookmarks** no AWS Glue.

**O que faz:**
- Rastreia quais dados já foram processados em execuções anteriores
- Na próxima execução, processa **apenas dados novos ou modificados**
- Evita reprocessamento, reduz custo e tempo de execução

```
Execução 1: processa arquivo_jan.xml ✓ (marcado no bookmark)
Execução 2: arquivo_jan.xml já processado → pula
            arquivo_fev.xml novo → processa ✓
```

---

## Glue vs outras opções de ETL

| Opção | Quando usar |
|---|---|
| **AWS Glue** | ETL gerenciado, serverless, integra com S3/Redshift/RDS |
| **AWS Glue Job Bookmarks** | Evitar reprocessamento de dados em runs incrementais |
| **AWS Glue FindMatches** | Identificar registros duplicados — NÃO é para controle de execução |
| **EMR** | Hadoop/Spark personalizado, maior controle |
| **Lambda** | Transformações simples orientadas a evento |

---

## Palavras-chave na prova

| Palavra-chave | Serviço |
|---|---|
| "consultar dados no S3 sem servidor" | **Amazon Athena** |
| "queries ad-hoc no data lake" | **Amazon Athena** |
| "ETL reprocessando dados antigos" | **Glue Job Bookmarks** |
| "processar só dados novos no Glue" | **Glue Job Bookmarks** |
| "encontrar registros duplicados" | **Glue FindMatches** |
| "pipeline ETL serverless" | **AWS Glue** |

---

*Domínio 3 — Design de Arquiteturas de Alta Performance (24%)*
