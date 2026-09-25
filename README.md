# enterprise-hsrp-network-infrastructure

🌐 Enterprise HSRP Network Infrastructure

Infraestrutura corporativa de alta disponibilidade com HSRP, VLANs, Inter-VLAN Routing, OSPF e arquitetura hierárquica em 3 camadas.

Projeto de infraestrutura de redes desenvolvido para demonstrar alta disponibilidade, redundância de gateway, segmentação de rede e roteamento dinâmico, utilizando uma arquitetura corporativa baseada em HSRP, VLANs 802.1Q, Router-on-a-Stick, OSPF e NAT/PAT.

📌 Visão Geral

A topologia foi projetada para proporcionar tolerância a falhas no First-Hop Gateway, isolamento dos domínios de broadcast por departamento e conectividade entre diferentes segmentos da rede corporativa.

A infraestrutura é composta por:

🔄 Redundância de gateway utilizando HSRP

🏢 Segmentação da rede através de VLANs

🔀 Roteamento Inter-VLAN

🌐 Roteamento dinâmico utilizando OSPF

🔐 NAT Overload (PAT) para acesso externo

📡 Trunks 802.1Q

📦 DHCP para distribuição automática de endereços

📊 Monitoramento de tráfego e dispositivos

🏗️ Arquitetura hierárquica em 3 camadas

⚡ Alta disponibilidade para os gateways corporativos

🏗️ Arquitetura da Rede

A topologia segue uma estrutura hierárquica composta por Core Layer e switches de acesso, com dois roteadores atuando como gateways redundantes através do HSRP.

                         ┌───────────────────┐
                         │    INTERNET/ISP   │
                         └─────────┬─────────┘
                                   │
                    ┌──────────────┴──────────────┐
                    │                             │
          ┌─────────▼─────────┐         ┌─────────▼─────────┐
          │     Router1       │         │      Router2      │
          │      ACTIVE       │         │     STANDBY       │
          │ HSRP Priority 110 │         │ HSRP Priority 100│
          └─────────┬─────────┘         └─────────┬─────────┘
                    │                             │
                    └──────────────┬──────────────┘
                                   │
                              802.1Q TRUNK
                                   │
                         ┌─────────▼─────────┐
                         │    CORE SWITCH    │
                         │    Layer 2        │
                         └─────────┬─────────┘
                                   │
             ┌─────────────────────┼─────────────────────┐
             │                     │                     │
      ┌──────▼──────┐       ┌──────▼──────┐       ┌──────▼──────┐
      │  ACCESS-01  │       │  ACCESS-02  │       │  ACCESS-03  │
      │  Andar 1    │       │  Andar 2    │       │  Andar 3    │
      │    ADM      │       │    OPS      │       │   INFRA      │
      └──────┬──────┘       └──────┬──────┘       └──────┬──────┘
             │                     │                     │
        ┌────┴────┐           ┌────┴────┐           ┌────┴────┐
        │         │           │         │           │         │
      ADMIN      RH          FIN       TI         SERVER     IoT/WiFi

🖼️ Topologia Implementada

Imagem principal do projeto

Coloque aqui uma imagem da topologia completa:

images/01-topologia-geral.png


🔄 Alta Disponibilidade — HSRP

O Hot Standby Router Protocol (HSRP) foi utilizado para fornecer redundância do gateway padrão.

O Router1 atua como roteador principal, enquanto o Router2 permanece em estado de standby.

Equipamento	Função	Prioridade	Estado
Router1	Gateway Principal	110	🟢 ACTIVE
Router2	Gateway Redundante	100	🟡 STANDBY

O gateway utilizado pelos dispositivos finais é um Virtual IP (VIP). Dessa forma, caso o Router1 apresente uma falha, o Router2 pode assumir o gateway virtual.

Benefícios

🔄 Redundância do primeiro salto

⚡ Failover automático

🛡️ Maior disponibilidade

🚫 Redução do impacto de falhas no gateway

🔧 Preempt configurado no roteador principal

🔒 Segmentação por VLAN

A rede foi dividida em diferentes VLANs para separar os departamentos e reduzir o domínio de broadcast.

VLAN	Departamento	Rede
10	ADMIN / IoT	10.10.10.0/24
20	RH	10.10.20.0/24
40	FIN / TI	10.10.40.0/24
70	SERVER / WIFI	10.10.70.0/24

A utilização de VLANs permite uma separação lógica dos diferentes setores da organização, facilitando a administração e o controle da infraestrutura.

🌐 Inter-VLAN Routing

O roteamento entre as VLANs é realizado através de Router-on-a-Stick.

Os roteadores utilizam subinterfaces com encapsulamento 802.1Q, permitindo que diferentes VLANs compartilhem o mesmo enlace físico através de um trunk.

Exemplo conceitual:

Router
 │
 └── Interface física
      │
      ├── VLAN 10 → 10.10.10.0/24
      ├── VLAN 20 → 10.10.20.0/24
      ├── VLAN 40 → 10.10.40.0/24
      └── VLAN 70 → 10.10.70.0/24

📋 Endereçamento e Gateways Redundantes
VLAN	Departamento	Router1	Router2	Virtual IP
10	ADMIN / IoT	10.10.10.2/24	10.10.10.3/24	10.10.10.1
20	RH	10.10.20.2/24	10.10.20.3/24	10.10.20.1
40	FIN / TI	10.10.40.2/24	10.10.40.3/24	10.10.40.1
70	SERVER / WIFI	10.10.70.2/24	10.10.70.3/24	10.10.70.1
Estado dos roteadores
VLAN	Router1	Router2
10	🟢 ACTIVE	🟡 STANDBY
20	🟢 ACTIVE	🟡 STANDBY
40	🟢 ACTIVE	🟡 STANDBY
70	🟢 ACTIVE	🟡 STANDBY
📡 Trunking 802.1Q

Os enlaces entre o Core e os switches de acesso utilizam 802.1Q Trunking, permitindo o transporte de múltiplas VLANs através de um único enlace físico.

                 CORE SWITCH
                      │
          ┌───────────┼───────────┐
          │           │           │
       TRUNK        TRUNK       TRUNK
       802.1Q       802.1Q      802.1Q
          │           │           │
       ACCESS-01   ACCESS-02   ACCESS-03

📡 DHCP

A infraestrutura possui serviço de DHCP para automatizar a configuração dos dispositivos finais.

O processo de obtenção do endereço utiliza o fluxo:

DHCP DISCOVER
      ↓
DHCP OFFER
      ↓
DHCP REQUEST
      ↓
DHCP ACK


Esse processo permite distribuir automaticamente:

Endereço IP

Máscara de rede

Gateway padrão

Informações necessárias para conectividade dos hosts

🌐 OSPF

O OSPF (Open Shortest Path First) é utilizado como protocolo de roteamento dinâmico.

A infraestrutura utiliza a Área 0 (Backbone Area) para permitir a convergência das rotas.

                OSPF AREA 0
                     │
          ┌──────────┴──────────┐
          │                     │
       Router1               Router2
          │                     │
          └──────────┬──────────┘
                     │
                 CORE NETWORK

Características

🔄 Roteamento dinâmico

⚡ Convergência automática

🧭 Seleção dinâmica de rotas

🌐 Backbone através da Area 0

🔐 NAT / PAT

Para permitir o acesso da rede interna à rede externa, foi utilizado NAT Overload (PAT).

O mecanismo permite que múltiplos dispositivos da rede privada compartilhem endereços públicos utilizando diferentes portas de origem.

LAN / VLANs
    │
    ▼
 Router
    │
   NAT/PAT
    │
    ▼
 INTERNET / ISP

📊 Monitoramento e Performance

Durante os testes realizados no ambiente de simulação, foram observados indicadores de desempenho relacionados ao tráfego, conectividade e funcionamento dos dispositivos.

Métricas observadas

📈 Throughput de até 1.5 Gbps

📦 Monitoramento de perda de pacotes

🔄 Funcionamento do HSRP

🌐 Convergência OSPF

📡 Tráfego entre VLANs

📋 Processo DHCP/DORA

🖥️ Estado dos dispositivos

Observação: os valores apresentados correspondem aos testes realizados no ambiente de simulação utilizado no projeto.

🖼️ Evidências do Projeto

Esta seção reúne as capturas de tela e evidências da implementação.

01 — Topologia Geral

Visão geral da arquitetura implementada.

02 — Monitoramento de Tráfego

Monitoramento do tráfego e throughput dos enlaces.

03 — DHCP e OSPF

Registros relacionados ao DHCP/DORA e à convergência OSPF.

04 — Monitoramento dos Dispositivos

Painel de monitoramento e estado dos dispositivos.

📁 Estrutura do Projeto
enterprise-hsrp-network-infrastructure/
│
├── README.md
│
├── configs/
│   ├── Router1-ACTIVE.txt
│   ├── Router2-STANDBY.txt
│   ├── CORE-SWITCH.txt
│   └── ACCESS-SWITCHES.txt
│
└── images/
    ├── 01-topologia-geral.png
    ├── 02-monitor-trafego-enlaces.png
    ├── 03-registro-eventos-dhcp.png
    └── 04-noc-monitor-dispositivos.png

🚀 Como Replicar a Topologia

Para reproduzir o ambiente no network-emulator.io, siga as etapas abaixo.

1. Criar os dispositivos

Adicione:

Router1

Router2

ISP

Core Switch

Access Switch 01

Access Switch 02

Access Switch 03

Hosts necessários para os testes

2. Conectar os dispositivos

Configure:

Router1 ── WAN ── ISP
Router2 ── WAN ── ISP

Router1 ── LAN ── CORE
Router2 ── LAN ── CORE

CORE ── ACCESS-01
CORE ── ACCESS-02
CORE ── ACCESS-03

3. Configurar os trunks

Configure os enlaces entre o Core e os switches de acesso como 802.1Q Trunk.

4. Configurar as VLANs

Crie as VLANs:

VLAN 10 → ADMIN / IoT
VLAN 20 → RH
VLAN 40 → FIN / TI
VLAN 70 → SERVER / WIFI

5. Aplicar as configurações

Utilize os arquivos disponíveis na pasta:

/configs

6. Validar a infraestrutura

Realize testes de:

Conectividade entre hosts

Comunicação entre VLANs

DHCP

HSRP

Failover do gateway

OSPF

NAT/PAT

Tráfego e throughput

🧪 Testes de Validação
Teste	Objetivo	Resultado esperado
Ping entre hosts	Validar conectividade	✅ Comunicação
DHCP	Validar atribuição automática	✅ IP recebido
Inter-VLAN	Validar roteamento	✅ Comunicação
HSRP	Validar redundância	✅ Failover
OSPF	Validar roteamento dinâmico	✅ Convergência
NAT/PAT	Validar saída externa	✅ Acesso externo
Tráfego	Avaliar desempenho	📊 Monitoramento
🛠️ Tecnologias e Conceitos








Principais conceitos utilizados:

HSRP

VLAN

802.1Q

Router-on-a-Stick

Inter-VLAN Routing

OSPF

NAT/PAT

DHCP

Trunking

Redundância de Gateway

Alta disponibilidade

Arquitetura hierárquica de redes

🎯 Objetivos do Projeto

O projeto teve como objetivo demonstrar, em um ambiente de simulação, a implementação de uma infraestrutura corporativa capaz de oferecer:

Alta disponibilidade do gateway;

Segmentação lógica dos departamentos;

Comunicação controlada entre redes;

Roteamento dinâmico;

Distribuição automática de endereços IP;

Acesso à rede externa;

Monitoramento da infraestrutura;

Tolerância a falhas no gateway principal.

👩‍💻 Autoria

Emily Ângelo

Projeto acadêmico/prático de infraestrutura de redes.

📄 Licença

Este projeto está disponível para fins educacionais e de demonstração.

<p align="center"> Desenvolvido com foco em redes, alta disponibilidade e infraestrutura corporativa. 🌐 </p>
