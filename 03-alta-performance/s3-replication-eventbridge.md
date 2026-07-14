# S3 Replication + EventBridge

## S3 Replication — replicação nativa entre buckets

**O que é:** Recurso nativo do S3 que copia objetos automaticamente de um bucket para outro, sem código.

| Tipo | O que faz |
|---|---|
| **SRR (Same-Region Replication)** | Replica dentro da mesma região |
| **CRR (Cross-Region Replication)** | Replica para outra região |

**Quando preferir sobre Lambda para cópia:**
- Replicação é gerenciada pela AWS — sem código para manter
- Mais eficiente e escalável que Lambda copiando objeto a objeto
- Suporta filtros por prefixo, tag, classe de storage

---

## Padrão: S3 Replication + EventBridge → múltiplos destinos

**Problema:** Upload chega no bucket A → precisa ir para bucket B + acionar Lambda + acionar SageMaker Pipelines.

**Solução errada:** Lambda para copiar + Lambda para processar (dois Lambdas, overhead dobrado)

**Solução correta:**
```
Bucket A (origem)
    ↓ S3 Replication (nativo, sem código)
Bucket B (análise)
    ↓ EventBridge (ObjectCreated)
    ├→ Lambda (processar dados)
    └→ SageMaker Pipelines (ML pipeline)
```

**Por que EventBridge e não S3 Event Notification diretamente?**
- S3 Event Notification → suporta poucos destinos (Lambda, SQS, SNS)
- EventBridge → suporta múltiplos destinos com regras flexíveis, incluindo SageMaker

---

## SSE-KMS com chave multi-região (CRR)

Para replicar objetos criptografados entre regiões usando **a mesma chave**:

- **SSE-S3:** não suporta cross-region com a mesma chave → ❌
- **SSE-KMS com chave single-region:** não funciona na região de destino → ❌
- **SSE-KMS com chave multi-região:** funciona em ambas as regiões → ✅

```
Região A: KMS multi-region key (primária)
Região B: KMS multi-region key (réplica, mesma chave material)
Resultado: mesma chave descriptografa em ambas as regiões
```

---

## Palavras-chave na prova

| Cenário | Solução |
|---|---|
| "copiar objetos entre buckets automaticamente" | **S3 Replication** (não Lambda) |
| "múltiplos destinos após chegada no S3" | **EventBridge** (não S3 Event Notification direto) |
| "mesma chave KMS em múltiplas regiões" | **KMS multi-region key** |
| "criptografar + replicar cross-region + mesma chave" | **SSE-KMS multi-region + CRR** |
| "Lambda + SageMaker + S3" | **S3 Replication + EventBridge** |

---

*Domínio 3 — Design de Arquiteturas de Alta Performance (24%)*
