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

*Domínio 3 — Design de Arquiteturas de Alta Performance (24%)*
