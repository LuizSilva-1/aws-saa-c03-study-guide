# Load Balancers (ALB, NLB, GWLB)

## Comparação

| Tipo | Camada | Protocolos | Quando usar |
|---|---|---|---|
| **ALB** | Layer 7 (aplicação) | HTTP/HTTPS | Apps web, microserviços, roteamento por path/host |
| **NLB** | Layer 4 (transporte) | TCP/UDP/TLS | Latência ultrabaixa, IPs estáticos, gaming |
| **GWLB** | Layer 3 (rede) | IP | Appliances de segurança (firewall, IDS/IPS) |

---

## ALB — Application Load Balancer

**Ideal para:** Aplicações web e microserviços

Features:
- Roteamento por path (`/api/*` → serviço A, `/web/*` → serviço B)
- Roteamento por host (`api.exemplo.com` → serviço A)
- Roteamento por header ou query string
- Integração com WAF (Web Application Firewall)
- Target groups: EC2, IP, Lambda, containers

## NLB — Network Load Balancer

**Ideal para:** Performance extrema e protocolos não-HTTP

Features:
- Latência ultrabaixa (microsegundos vs milissegundos do ALB)
- Milhões de requests por segundo
- IPs estáticos (1 por AZ)
- Preserva IP de origem do cliente
- Suporta TCP, UDP, TLS

## GWLB — Gateway Load Balancer

**Ideal para:** Inspecionar tráfego com appliances third-party

Features:
- Transparente para aplicações (inline inspection)
- Escala appliances de segurança automaticamente
- Usa GENEVE protocol

---

## Decisão rápida na prova

| Palavra-chave | Solução |
|---|---|
| "HTTP", "HTTPS", "roteamento por path", "microserviços" | **ALB** |
| "TCP", "UDP", "IP estático", "latência ultrabaixa", "gaming" | **NLB** |
| "Firewall", "IDS/IPS", "inspecionar tráfego", "appliance" | **GWLB** |
| "WAF" | **ALB** (WAF se associa ao ALB) |

---

## ALB vs NLB — erros comuns

| Mito | Realidade |
|---|---|
| "NLB é melhor que ALB" | São para casos diferentes, não melhor/pior |
| "ALB tem IP estático" | ❌ ALB usa DNS. Para IP estático → NLB |
| "NLB faz roteamento por URL" | ❌ NLB é Layer 4, não entende HTTP |

---

*Domínio 3 — Design de Arquiteturas de Alta Performance (24%)*
