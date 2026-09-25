🌐 Enterprise HSRP Network Infrastructure — Alta Disponibilidade & Redundância de Gateway

HSRP VLAN 802.1Q OSPF Router-on-a-Stick DHCP NAT/PAT Network Emulator

Console e laboratório de infraestrutura corporativa de redes, desenvolvido para demonstrar alta disponibilidade, redundância de gateway, segmentação por VLAN, roteamento Inter-VLAN e convergência dinâmica através de OSPF.

A arquitetura utiliza dois roteadores redundantes com HSRP, um Core Switch e múltiplos switches de acesso, reproduzindo uma topologia corporativa hierárquica com diferentes segmentos de usuários, servidores e dispositivos de infraestrutura.

🌐 Visão Geral da Infraestrutura

A topologia foi projetada para garantir continuidade de conectividade em caso de falha do gateway principal, utilizando um endereço IP virtual compartilhado entre os roteadores.

                         🌐 INTERNET / ISP
                                │
                 ┌──────────────┴──────────────┐
                 │                             │
        ┌────────▼────────┐           ┌────────▼────────┐
        │    Router1      │           │    Router2      │
        │    ACTIVE       │           │    STANDBY      │
        │ Priority: 110   │           │ Priority: 100   │
        └────────┬────────┘           └────────┬────────┘
                 │                             │
                 └──────────────┬──────────────┘
                                │
                         802.1Q TRUNK
                                │
                      ┌─────────▼─────────┐
                      │    CORE SWITCH    │
                      │      LAYER 2      │
                      └─────────┬─────────┘
                                │
             ┌──────────────────┼──────────────────┐
             │                  │                  │
      ┌──────▼──────┐    ┌──────▼──────┐    ┌──────▼──────┐
      │  ACCESS-01  │    │  ACCESS-02  │    │  ACCESS-03  │
      │  ADMIN / RH │    │   OPS / FIN │    │ TI / INFRA   │
      └──────┬──────┘    └──────┬──────┘    └──────┬──────┘
             │                  │                  │
          💻 PCs              💻 PCs             🖥️ Servers
                                                📡 IoT / Wi-Fi

🚀 Funcionalidades & Recursos

⚡ Alta Disponibilidade com HSRP: Router1 opera como gateway ACTIVE com prioridade 110, enquanto Router2 permanece em STANDBY com prioridade 100, utilizando IPs virtuais para os gateways das VLANs.

🔄 Failover Automático: Em caso de indisponibilidade do roteador principal, o segundo roteador pode assumir o gateway virtual, mantendo a conectividade dos dispositivos da rede.

🔒 Segmentação por VLAN: Separação lógica dos departamentos através de VLANs 802.1Q, reduzindo os domínios de broadcast e organizando a infraestrutura por setores.

🌐 Inter-VLAN Routing: Comunicação entre diferentes segmentos da rede através de Router-on-a-Stick e subinterfaces com encapsulamento dot1Q.

📡 Trunking 802.1Q: Enlaces trunk entre o Core e os switches de acesso para transporte de múltiplas VLANs.

🧭 Roteamento Dinâmico com OSPF: Convergência das rotas utilizando OSPF na Area 0, permitindo adaptação dinâmica da tabela de roteamento.

🛒 DHCP Integrado: Distribuição automática de endereços IP, gateways e parâmetros de rede para os dispositivos finais.

🔐 NAT Overload (PAT): Tradução de endereços privados para permitir comunicação dos dispositivos internos com a rede externa.

📊 Monitoramento de Rede: Análise de throughput, conectividade, estado dos dispositivos, eventos DHCP e comportamento dos enlaces durante os testes.

🏢 Segmentação da Rede
VLAN	Setor	Rede	Gateway Virtual
10	ADMIN / IoT	10.10.10.0/24	10.10.10.1
20	RH	10.10.20.0/24	10.10.20.1
40	FIN / TI	10.10.40.0/24	10.10.40.1
70	SERVER / WIFI	10.10.70.0/24	10.10.70.1
🔄 Gateway Redundante
VLAN	Router1	Router2	HSRP
10	10.10.10.2	10.10.10.3	10.10.10.1
20	10.10.20.2	10.10.20.3	10.10.20.1
40	10.10.40.2	10.10.40.3	10.10.40.1
70	10.10.70.2	10.10.70.3	10.10.70.1

Router1: 🟢 ACTIVE — Priority 110
Router2: 🟡 STANDBY — Priority 100

📊 Testes & Métricas

A infraestrutura foi submetida a testes de conectividade, tráfego e disponibilidade no ambiente de simulação.

Resultados observados

📈 Throughput de até 1.5 Gbps

📦 Monitoramento de perda de pacotes

🔄 Testes de failover HSRP

🌐 Verificação da convergência OSPF

📡 Validação do DHCP/DORA

🔀 Testes de comunicação Inter-VLAN

🔐 Validação de NAT/PAT

🖥️ Monitoramento do estado dos dispositivos

Os valores apresentados correspondem às medições realizadas no ambiente de simulação utilizado para o projeto.

🖼️ Evidências do Projeto
🌐 Topologia Geral

Arquitetura completa da infraestrutura corporativa.

📈 Monitoramento de Tráfego

Dashboard utilizado para acompanhamento do tráfego e throughput dos enlaces.

📋 DHCP & OSPF

Registros dos processos DHCP/DORA e eventos relacionados ao roteamento.

🖥️ Monitoramento NOC

Painel de monitoramento dos dispositivos e consumo da infraestrutura.

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

🧩 Tecnologias & Protocolos








Principais conceitos utilizados:

HSRP · VLAN · 802.1Q · Router-on-a-Stick · Inter-VLAN Routing · OSPF · NAT/PAT · DHCP · Trunking · High Availability

☁️ Como Replicar a Topologia

Para reproduzir o laboratório no network-emulator.io:

1. Criar Router1 e Router2
2. Conectar os roteadores ao ISP
3. Conectar os roteadores ao CORE
4. Criar as VLANs
5. Configurar os trunks 802.1Q
6. Conectar ACCESS-01, ACCESS-02 e ACCESS-03
7. Aplicar os arquivos de configuração
8. Validar DHCP e Inter-VLAN Routing
9. Validar HSRP e realizar teste de failover
10. Validar OSPF e conectividade externa
11. Executar os testes de tráfego

⚙️ Arquivos de Configuração

As configurações individuais dos equipamentos estão disponíveis em:

/configs


Router1-ACTIVE.txt → Configuração do gateway principal

Router2-STANDBY.txt → Configuração do gateway redundante

CORE-SWITCH.txt → VLANs e trunks

ACCESS-SWITCHES.txt → Portas de acesso e segmentação

🎯 Objetivo do Projeto

Este projeto foi desenvolvido para demonstrar, de forma prática, conceitos fundamentais de infraestrutura de redes corporativas, com foco em:

Disponibilidade → Redundância → Segmentação → Roteamento → Monitoramento

A proposta combina diferentes tecnologias de redes em uma única topologia, simulando um ambiente corporativo com múltiplos departamentos e necessidade de continuidade operacional.

👩‍💻 Autoria & Licença

Desenvolvido por Emily Ângelo

Projeto desenvolvido para fins educacionais, acadêmicos e de demonstração de conhecimentos em infraestrutura de redes.

<p align="center"> 🌐 <strong>Enterprise HSRP Network Infrastructure</strong><br> High Availability • VLAN Segmentation • Dynamic Routing • Network Monitoring </p>
