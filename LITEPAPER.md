# PLEGMA DAG — Litepaper v1.0
### *Parallel Ledger Economy Graph · Directed Acyclic Graph*

> **"Código que não pede permissão. Consenso que não tem dono.
> Valor que não depende de ninguém acreditar nele — apenas da matemática ser verdadeira."**

---

## Sumário

1. [O Problema](#1-o-problema)
2. [A Solução](#2-a-solução)
3. [Arquitetura DAG](#3-arquitetura-dag)
4. [Criptografia Pós-Quântica](#4-criptografia-pós-quântica)
5. [Segurança — Sistema Sentinela](#5-segurança--sistema-sentinela)
6. [Tokenomics](#6-tokenomics)
7. [Genesis Reserve — $PLG-G](#7-genesis-reserve--plg-g)
8. [Governança](#8-governança)
9. [Mineração e Participação de Rede](#9-mineração-e-participação-de-rede)
10. [Roadmap](#10-roadmap)
11. [Princípios do Protocolo](#11-princípios-do-protocolo)

---

## 1. O Problema

As redes blockchain de primeira e segunda geração apresentam limitações estruturais que impedem a soberania digital real:

- **Centralização progressiva:** Mineração concentrada em pools industriais contradiz o princípio de descentralização.
- **Vulnerabilidade quântica:** A criptografia ECDSA/Ed25519 utilizada pela maioria das redes será quebrada por computadores quânticos — não é questão de *se*, mas de *quando*.
- **Exclusão por hardware:** Participar da rede exige equipamentos caros, excluindo a maioria da população global.
- **Governança capturada:** Sistemas de votação baseados em simples proof-of-stake favorecem baleias e perpetuam oligarquias.
- **Custódia centralizada:** Exchanges e custodians controlam efetivamente os ativos dos usuários.

**A Plegma DAG foi construída para resolver todos esses problemas simultaneamente.**

---

## 2. A Solução

PLEGMA DAG é um protocolo de registro distribuído de nova geração baseado em estrutura DAG (*Directed Acyclic Graph*), com:

- Criptografia **pós-quântica nativa** (Crystals-Dilithium3, padrão NIST FIPS 204)
- Consenso **sem mineração em blocos lineares** — qualquer dispositivo participa
- Governança **matemática e transparente** via sistema de tiers com pesos de voto verificáveis
- Segurança em **três camadas independentes** com slashing automático
- **Soberania de hardware** — a carteira é vinculada ao dispositivo, não ao provedor

---

## 3. Arquitetura DAG

Ao contrário de blockchains lineares, a Plegma utiliza um **Grafo Acíclico Dirigido (DAG)** como estrutura de dados.

### Como funciona

Cada transação é um **vértice** do grafo. Para ser aceita, deve referenciar dois ou mais vértices anteriores válidos (*pais*). A validação é distribuída — não existe um único minerador selecionado por sorteio ou poder de hash.

```
[Vértice A] ──────────┐
                      ▼
[Vértice C] ──────────────► [Novo Vértice]
                      ▲
[Vértice B] ──────────┘
```

### Vantagens sobre blockchain linear

| Característica | Blockchain Linear | Plegma DAG |
|---|---|---|
| Throughput | Limitado por bloco | Paralelo e escalável |
| Finalidade | Probabilística (confirmações) | Estrutural (referência direta) |
| Hardware necessário | Alto (mineração) | Qualquer dispositivo |
| Participação | Exclusiva (pools) | Universal |

---

## 4. Criptografia Pós-Quântica

A Plegma é uma das primeiras redes públicas a implementar **Crystals-Dilithium3** como algoritmo nativo de assinatura de transações e autenticação.

### Por que Dilithium3?

| Parâmetro | ECDSA (Ethereum) | Dilithium3 (Plegma) |
|---|---|---|
| Padrão | Pré-quântico | NIST FIPS 204 (2024) |
| Segurança | Vulnerável a Shor | Resistente a computadores quânticos |
| Chave pública | 33 bytes | 1.952 bytes |
| Assinatura | 64 bytes | 3.293 bytes |
| Nível de segurança | 128-bit clássico | Level 3 (equivalente AES-192) |

### ZK-SNARK BN128

Além de Dilithium3, a Plegma utiliza **provas de conhecimento zero** (ZK-SNARK sobre a curva BN128) para validação de transações sem revelar dados sensíveis do remetente.

> Cada prova ZK ocupa aproximadamente **543 bytes** — compacta e eficiente para uso em dispositivos móveis.

---

## 5. Segurança — Sistema Sentinela

O **Sentinela** é o módulo de segurança da Plegma, operando em três camadas independentes e sequenciais.

### Camada 1 — Vigia (Borda da Rede)

Primeira linha de defesa. Atua antes que qualquer transação entre na mempool.

- Geofencing: bloqueia jurisdições com restrições legais severas
- Anti-Smurfing: limita nós móveis por endereço IP
- PHR (Higiene de Protocolo): filtra conteúdo ilícito no payload

### Camada 2 — Crivo (Mempool)

Auditoria matemática de integridade.

- Detecção de overflow e underflow de valores
- Proteção contra ataques de reentrância
- Validação de limites de supply total

### Camada 3 — Escudo (Reputação e Slashing)

Sistema de reputação por identidade de nó com penalidade automática.

- Score de reputação por UIDG (identificador único de carteira)
- **Slashing:** confisco total de stake + banimento permanente para gasto duplo confirmado
- Ativado automaticamente por tentativas de ataque severo

### Anti-Dump

Transações acima de 500.000 PLG têm atraso automático de 24 horas — proteção estrutural contra manipulação de mercado.

---

## 6. Tokenomics

### $PLG — Token Nativo da Rede

| Parâmetro | Valor |
|---|---|
| Supply total | 21.000.000.000 PLG |
| Emissão | Por mineração (Validator e Prover Pool) |
| Taxa de transação | 0,1% do valor (pós-Genesis, Dia 31+) |
| Vesting de recompensas | 30 dias a partir do lançamento oficial |

### Fórmula de Emissão

A recompensa por bloco é calculada de forma dinâmica:

```
R = G / N
R = Recompensa do período
G = Pool disponível (Validator ou Prover)
N = Número de nós ativos do mesmo tipo
```

Esta fórmula garante que a diluição seja **proporcional à participação** — não existe vantagem injusta para primeiros entrantes além da proporção natural.

### Alocação dos Pools

| Pool | % do Tesouro | Perfil de Hardware |
|---|---|---|
| Validator Pool | 60% | Mobile, tablet, notebook, PC básico |
| Prover Pool | 40% | GPU dedicada, ASIC, servidor |

A distribuição 60/40 favorece deliberadamente a participação de dispositivos comuns — smartphones e notebooks — garantindo descentralização real.

### Justice Cap

Nenhum Prover individual pode executar mais de **10% do trabalho total da rede** (Cláusula 14 do protocolo). Esta regra é aplicada em código, não apenas por política.

### Distribuição de Taxas de Rede

As taxas coletadas no DAG são distribuídas em tempo real entre os participantes:

**Modo Padrão:**

| Destinatário | % das Taxas |
|---|---|
| Aerarium | 10% |
| Provers | 40% |
| Validadores | 50% |

**Regra de Transbordo** — Após o Aerarium atingir o teto programado de $1.000, a distribuição se reconfigura automaticamente:

| Destinatário | % das Taxas |
|---|---|
| Aerarium | 0% |
| Provers | 40% |
| Validadores | 60% |

> O excedente bonifica integralmente a base de suporte (validadores) — sem acúmulo além do necessário para manutenção.

---

## 7. Genesis Reserve — $PLG-G

O **Genesis Reserve** é o mecanismo de bootstrap e governança inicial da rede. Através dele, os primeiros participantes adquirem direitos de governança proporcionais ao seu aporte.

Não existe ICO, pré-mineração ou fatia silenciosa de fundador. Existem exatamente **10.500.000 PLG-G** disponíveis — 0,05% do supply total de 21 bilhões de $PLG — vendidos a preço fixo de **$0,10 USDC** por unidade, sem negociação, sem desconto para "investidores estratégicos", sem janela VIP.

### $PLG-G — Token de Governança Genesis

| Parâmetro | Valor |
|---|---|
| Supply total | 10.500.000 PLG-G |
| Preço Genesis | $0,10 USDC (fixo — sem desconto, sem VIP) |
| Aporte mínimo | $100 USDC (= 1.000 PLG-G) |
| Lock-up | 30 dias a partir do lançamento oficial (liberado no Dia 31) |
| Pagamento | USDC nativo na rede Polygon |
| Carteira de pagamento | 0xD8422d6936bE77179DC33C7C2ffceEF4c34FB183 |
| Tokens não vendidos | Queimados automaticamente no Dia 31 |

### Tiers Genesis

> **Atualizado — Blog Post "Genesis Reserve: distribuição, destino dos fundos e governança" (22/03/2026)**

| Tier | Saldo PLG-G | Aporte | Boost de Mineração | Mineradores (máx) |
|---|---|---|---|---|
| **MASTER** | 6.001–10.000 | $601–$1.000 | 2,0× | até 10 |
| **SENTINELA** | 3.001–6.000 | $301–$600 | 1,5× | até 6 |
| **APOIADOR** | 1.000–3.000 | $100–$300 | 1,0× | até 3 |
| Validador comum | < 1.000 | — | — | — |

> Saldo acima de 10.000 PLG-G: peso de voto e boost permanecem no máximo (5,0 e 2,0×). O excedente acumula para negociação P2P futura.

### Destinação dos Fundos (Automático no Dia 31)

| Destino | % USDC Arrecadado | Finalidade |
|---|---|---|
| Pool de Liquidez | 90% | Pool inicial $PLG/USDC — liquidez desde o primeiro segundo no mercado aberto |
| Aerarium | 10% | Desenvolvimento contínuo e custeio de infraestrutura (Nós Âncoras Continentais) |

### Timeline — Dia 30 e Dia 31

| Momento | Evento |
|---|---|
| Dias 0–30 | Carteira Genesis ativa, compras aceitas |
| Dia 31 | **Três eventos simultâneos e automáticos:** |
| └─ (1) | Lockup dos PLG-G dos Sócios liberado — tokens disponíveis para movimentação P2P |
| └─ (2) | Lockup das recompensas mineradas do período inicial liberado |
| └─ (3) | Carteira Genesis encerrada definitivamente — nenhuma nova compra aceita |
| Dia 31+ | USDC enviado após encerramento: devolvido automaticamente ao remetente |
| Dia 31+ | PLG-G não vendidos: queimados automaticamente (público, verificável, definitivo) |

### Negociação P2P de PLG-G

PLG-G **não pode** ser vendido em exchanges ou pools de liquidez — o contrato rejeita operações AMM. A negociação é exclusivamente P2P via três canais:

- **Mercado de Sócios** — página exclusiva na plataforma PLEGMA, sem intermediário e sem taxa de protocolo
- **Rede PLEGMA Social** — oferta publicada na rede social nativa, visível para toda a comunidade
- **Canal externo** — qualquer meio fora da plataforma; a transferência on-chain é de carteira para carteira

Após o lock-up de 30 dias, os tokens PLG-G são transferíveis P2P. No Dia 31, a governança plena é ativada.

---

## 8. Governança

A governança da Plegma é **matemática e on-chain**. O peso de voto de cada participante é calculado por fórmula verificável, não por decisão de equipe ou multisig.

### Fórmula de Peso de Voto

```
peso_voto = 1,01 + (saldo_plgg - 1.000) × (3,99 / 9.000)
Range: 1,01 (com 1.000 PLG-G) → 5,0 (com ≥ 10.000 PLG-G)
Participantes sem PLG-G: peso = 1,0 (voto igual)
```

Nenhum participante, independentemente de seu saldo, tem peso maior que **5,0** — impedindo capturas oligárquicas.

### PMR — Protocol Maturity Rating

A Plegma possui um mecanismo único: a **Chave DEUS** — uma chave administrativa de emergência mantida pelos fundadores, que será **queimada publicamente e de forma irreversível** quando o protocolo atingir maturidade real.

Condições para queima (todas simultâneas):

- Média de 30 dias com ≥ 100.000 nós ativos
- Implementação de IA autônoma de consenso
- Auditoria de segurança independente aprovada
- PlegmaVM (smart contracts completos) operacional
- **≥ 67% dos membros Genesis aprovam via votação de 30 dias**

Enquanto a Chave DEUS existe, ela é uma salvaguarda documentada e transparente — não um poder oculto.

---

## 9. Mineração e Participação de Rede

### Dois Tipos de Nó

**Validator (Dispositivo Comum)**
- Smartphone, tablet, notebook, PC com CPU básica
- Participa do Validator Pool (60% da emissão)
- Acessível para qualquer pessoa no mundo

**Prover (Hardware de Alta Performance)**
- Gaming PC, GPU dedicada, ASIC, servidor
- Participa do Prover Pool (40% da emissão)
- Sujeito ao Justice Cap de 10%

### Binding de Hardware

A carteira é criptograficamente vinculada ao dispositivo no primeiro boot. Isso previne:

- Roubo de chave privada por cópia de arquivo
- Ataques de portabilidade de identidade

---

## 10. Roadmap

### ✅ Implementado (BUILD 009 — Mar 2026)

- Motor DAG com consenso e gossip P2P
- Criptografia Dilithium3 + ZK-SNARK BN128
- Sistema Sentinela (3 camadas + Slashing)
- Genesis Reserve com tiers MASTER/SENTINELA/APOIADOR (parâmetros atualizados 22/03/2026)
- Governança por saldo com pesos matemáticos
- App mobile (Android/iOS) com autenticação QR
- Dashboard web responsivo
- Landing page completa (17 páginas)
- Monitor de pagamentos USDC (Polygon)
- Suporte a 100+ idiomas

### 🟡 Próxima fase (V1.5)

- Script de auto-instalação de nó (1 comando)
- Dashboard de consumo de recursos por nó
- Marketplace P2P entre membros
- Reconexão automática de pares

### ⚪ Longo prazo (V2.0+)

- PlegmaVM — smart contracts completos
- IA autônoma de consenso no core
- Pacto dos 5 — Shamir Secret Sharing para recuperação
- Auditoria de segurança independente
- Cartão de débito integrado
- Rede social P2P completa
- **Queima da Chave DEUS** — protocolo totalmente autônomo

---

## 11. Princípios do Protocolo

A Plegma DAG opera por princípios imutáveis, não por declarações de intenção:

**Autonomia matemática** — Regras em código, não em promessas. Cada parâmetro de governança, emissão e segurança é verificável publicamente.

**Descentralização real** — Um smartphone tem o mesmo direito de participar que um servidor. A arquitetura foi desenhada para isso, não apenas declarada.

**Soberania do usuário** — Sua chave, seu dispositivo, seu dinheiro. O protocolo não tem custódia.

**Transparência radical** — O PMR existe porque a Chave DEUS existe. Preferiríamos não precisar dela. Quando o protocolo for suficientemente maduro, ela será destruída — em público, de forma irreversível, por decisão coletiva.

**Justiça Absoluta** — O preço fixo de $0,10 elimina assimetria de informação. Quem entrou primeiro não pagou menos que quem chegou no último dia. Regras iguais, condições iguais, sem preferência.

---

## Links

- Site oficial: [plegmadag.com](https://plegmadag.com)
- Telegram: [t.me/plegma_dag](https://t.me/plegma_dag)
- GitHub: [github.com/PlegmaDag](https://github.com/PlegmaDag)

---

*PLEGMA DAG Litepaper v1.1 — Março 2026*
*Parâmetros Genesis atualizados em 28/03/2026 conforme blog post "Genesis Reserve: distribuição, destino dos fundos e governança" (22/03/2026)*
*Este documento descreve a arquitetura lógica e o modelo econômico do protocolo. Não constitui oferta de valores mobiliários.*
