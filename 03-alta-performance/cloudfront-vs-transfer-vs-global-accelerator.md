# CloudFront vs Transfer Acceleration vs Global Accelerator

## Comparação

| Serviço | Direção do tráfego | Cache | IPs fixos | Protocolos |
|---|---|---|---|---|
| **CloudFront** | AWS → Usuário (download) | ✅ Sim | ❌ Não | HTTP/HTTPS |
| **Transfer Acceleration** | Usuário → S3 (upload) | ❌ Não | ❌ Não | HTTP/HTTPS |
| **Global Accelerator** | Bidirecional | ❌ Não | ✅ 2 IPs anycast | TCP/UDP |

---

## Palavras-chave na prova

| Se o cenário mencionar... | Resposta |
|---|---|
| "entrega de conteúdo", "vídeo", "latência de download", "cache global" | **CloudFront** |
| "upload rápido de arquivos grandes para S3" | **Transfer Acceleration** |
| "IP estático", "TCP/UDP", "gaming", "failover entre regiões", "app dinâmica" | **Global Accelerator** |

---

## Detalhes de cada um

### CloudFront (CDN)
- Cacheia conteúdo em 400+ edge locations globais
- Ideal para: sites, vídeos, APIs com respostas cacheáveis
- Reduz latência entregando de um ponto próximo ao usuário
- **Não** fornece IPs estáticos (usa DNS/CNAME)

### Transfer Acceleration
- Usa edge locations para acelerar **uploads** para S3
- Dados entram no edge mais próximo e viajam pela backbone AWS até o bucket
- Só funciona com S3 (não serve para outros serviços)
- **Direção: usuário → S3** (não o contrário)

### Global Accelerator
- 2 IPs anycast estáticos globais
- Tráfego entra no edge e viaja pela rede privada AWS
- Suporta TCP e UDP (não só HTTP)
- Health checks + failover automático entre regiões
- Ideal para: gaming, IoT, apps multi-região que precisam de IP fixo

---

## Analogias

- **CloudFront** = bancas de jornal espalhadas com cópias (cache) para não ir até a editora
- **Transfer Acceleration** = estrada expressa para levar seu pacote ATÉ o depósito (S3)
- **Global Accelerator** = atalho pela rede privada AWS direto até sua app

---

## Erro comum

Confundir Transfer Acceleration (upload) com CloudFront (download):
- "Usuários reclamam que vídeo demora para carregar" → **CloudFront** (download)
- "Usuários reclamam que arquivo demora para subir" → **Transfer Acceleration** (upload)

---

## Lambda@Edge — compute no edge

**O que é:** Permite executar código (Lambda) nos edge locations do CloudFront, perto do usuário.

**Quando usar:**
- Resposta **personalizada** por usuário (ex: dashboard, saldo, recomendações)
- Exige latência **< 100ms global**
- **Sem replicar** a aplicação para outras regiões

**Limitações:**
- Execução máxima: 5 segundos (viewer request/response) ou 30 segundos (origin request/response)
- Memória: até 10.240 MB
- Não é para processamento pesado (vídeo, ML)

**A analogia:** Um chef vai até a cidade do cliente e cozinha o prato personalizado lá — não precisa ir até o restaurante central.

---

## ⚠️ LEMBRETE CRÍTICO — cache vs personalização

**ANTES de escolher CloudFront com cache, pergunte-se:**
> "Se dois usuários diferentes acessarem a mesma URL, devem receber a MESMA resposta?"

| Resposta | Solução |
|---|---|
| **SIM** (produto, artigo, vídeo) | CloudFront com cache ✅ |
| **NÃO** (saldo, perfil, extrato, recomendações) | Lambda@Edge ✅ (cache ❌) |

**Palavras que ELIMINAM cache:**
- "personalizado", "por cliente", "saldo", "extrato", "perfil", "recomendações"

**Palavras que INDICAM cache:**
- "catálogo", "produto", "artigo", "vídeo", "imagem", "mesmo para todos"

---

## Resumo: CloudFront vs Global Accelerator vs Lambda@Edge

| Cenário | Solução | Por quê |
|---|---|---|
| Conteúdo **igual** para todos + latência | **CloudFront (cache)** | Cacheia e serve do edge |
| **TCP/UDP** + IPs fixos + failover entre regiões | **Global Accelerator** | Otimiza rede, não é HTTP |
| Conteúdo **personalizado** + latência < 100ms | **Lambda@Edge** | Compute no edge |
| App dinâmica + melhorar latência (sem exigir < 100ms) | **Global Accelerator** | Backbone privada, melhora ~30-40% |

> **Regra:** Precisa de **cache** → CloudFront. Precisa de **código** no edge → Lambda@Edge. Precisa de **rede otimizada** → Global Accelerator.

---

*Domínio 3 — Design de Arquiteturas de Alta Performance (24%)*
