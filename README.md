# enterprise-hsrp-network-infrastructure

Console e documentação técnica da infraestrutura corporativa de alta disponibilidade, focado na implementação de redundância de gateway com HSRP (Hot Standby Router Protocol), segmentação Layer 2 por VLANs, roteamento Inter-VLAN (Router-on-a-Stick), convergência OSPF e distribuição hierárquica em 3 camadas.🌐 Visão Geral da ArquiteturaA topologia foi desenhada para garantir tolerância total a falhas no First-Hop Gateway e isolamento estrito de domínios de broadcast por setor corporativo, suportando uma vazão de tráfego superior a 1.5 Gbps com 0% de perda de pacotes.Plaintext                           [ INTERNET / ISP ]
                                   │
                    ┌──────────────┴──────────────┐
                    │                             │
           +--------+--------+           +--------+--------+
           |  Router1 (PRI)  |           |  Router2 (SEC)  |
           | HSRP Priority 110|           | HSRP Priority 100|
           +--------+--------+           +--------+--------+
                    │                             │
                    └──────────────┬──────────────┘
                                   │ (802.1Q Trunk)
                           +-------+-------+
                           |  CORE SWITCH  |
                           |  (L2 Trunk)   |
                           +-------+-------+
                                   │
        ┌──────────────────────────┼──────────────────────────┐
        │                          │                          │
+-------+-------+          +-------+-------+          +-------+-------+
|   ACCESS-01   |          |   ACCESS-02   |          |   ACCESS-03   |
| (Andar 1/ADM) |          | (Andar 2/OPS) |          | (Andar 3/INFRA|
+-------+-------+          +-------+-------+          +-------+-------+
    │       │                  │       │                  │       │
[PC-ADMIN] [PC-RH]          [PC-FIN] [PC-TI]          [SERVER] [IOT/WIFI]
🚀 Funcionalidades & Recursos⚡ Redundância HSRP Ativo/Standby (Group Failover): Mapeamento de IPs Virtuais (VIPs) onde o Router1 assume o papel ACTIVE (Prioridade 110 com Preempt) e o Router2 atua como STANDBY (Prioridade 100) para tempo zero de downtime na falha do enlace primário.🔒 Segmentação Layer 2 (802.1Q Trunking): Isolamento lógico de departamentos através de VLANs dedicadas, prevenindo tempestades de broadcast (broadcast storms) entre setores sensíveis (ex: FIN, TI e SERVERS).📊 Inter-VLAN Routing (Router-on-a-Stick): Roteamento entre sub-redes configurado via subinterfaces virtuais com encapsulamento dot1Q diretamente na borda.🛒 Serviços Atribuição Dinâmica (DHCP Integrado): Distribuição automatizada do pool de IPs e gateways para estações finais via DORA.📈 NOC & Monitoramento em Tempo Real: Análise contínua de throughput e consumo de ativos por departamento no simulador de tráfego.🌐 Roteamento Dinâmico & Saída ISP: Convergência OSPF na Área 0 com NAT Overload (PAT) para acesso à rede pública externa.📋 Tabela de Endereçamento & Gateways RedundantesVLANDepartamentoIP Subinterface R1IP Subinterface R2Virtual IP / GatewayEstado R1Estado R210ADMIN / IOT10.10.10.2 /2410.10.10.3 /2410.10.10.1ACTIVE 🟢STANDBY 🟡20RH10.10.20.2 /2410.10.20.3 /2410.10.20.1ACTIVE 🟢STANDBY 🟡40FIN / TI10.10.40.2 /2410.10.40.3 /2410.10.40.1ACTIVE 🟢STANDBY 🟡70SERVER / WIFI10.10.70.2 /2410.10.70.3 /2410.10.70.1ACTIVE 🟢STANDBY 🟡📊 Performance & Métricas do NOCDurante a execução dos testes de vazão no painel do Network Operations Center (NOC), a infraestrutura manteve 0 drops de pacotes operando sob carga total de tráfego.1. Métricas de Enlaces e Throughput (1.5 Gbps / 0 Drops)2. Registos do Plano de Controle & DHCP DORA3. Painel de Saúde dos Dispositivos em Tempo Real📁 Estrutura do ProjetoPlaintextEnterprise-Network/
├── README.md                      # Documentação oficial do projeto
├── configs/
│   ├── Router1-ACTIVE.txt         # Script de CLI completo do Roteador Principal
│   ├── Router2-STANDBY.txt        # Script de CLI completo do Roteador Secundário
│   ├── CORE-SWITCH.txt            # Configurações de Troncos L2 e VLANs
│   └── ACCESS-SWITCHES.txt        # Configurações de Portas Access por Setor
└── images/
    ├── 01-topologia-geral.png            # Arquitetura visual completa
    ├── 02-monitor-trafego-enlaces.png    # Dashboard de tráfego dos links
    ├── 03-registro-eventos-dhcp.png      # Logs de evento do DORA e OSPF
    └── 04-noc-monitor-dispositivos.png   # Monitor de consumo e saúde dos hosts
☁️ Como Replicar a TopologiaPara importar e validar a topologia no network-emulator.io:Suba os roteadores Router1 e Router2 e conecte-os ao ISP na WAN1 e ao CORE na LAN2.Crie os troncos (Trunks) 802.1Q no CORE para as portas port4, port5 e port6 ligando aos switches de acesso ACESS1, ACESS2 e ACESS3.Aplique os scripts contidos na pasta /configs correspondentes a cada dispositivo.Execute o gerador de tráfego para validar a redundância HSRP e convergência de links.👩‍💻 Autoria & LicençaDesenvolvido por Emily Ângelo
