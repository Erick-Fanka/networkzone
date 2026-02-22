# Cenário de Rede — FankaTech (Infraestrutura Corporativa)

## 1) Contexto
A FankaTech é uma empresa em expansão que precisa de uma infraestrutura de rede robusta, segmentada e segura. O objetivo deste laboratório foi projetar uma topologia hierárquica usando um Core L3 para roteamento Inter-VLAN, serviços centralizados em uma DMZ (Server Farm) e integração com a internet utilizando NAT e roteamento dinâmico.

- Departamentos corporativos (Diretoria, TI, Financeiro, Suporte) possuem segmentação lógica via VLANs e recebem IPs via DHCP Relay.
- A rede de Visitantes (VLAN 50) adota o modelo **Zero Trust**, tendo acesso restrito: conseguem acessar serviços web públicos e a internet, mas são totalmente bloqueados de alcançar a rede interna da corporação.

## 2) Equipamentos
- 1 Roteador de Borda (Router-Borda) → Conecta a empresa à "Internet" e realiza NAT (PAT).
- 1 Roteador Interno (Router-Interno) → Faz a ponte via OSPF entre o Core e a Borda.
- 1 Switch Core L3 (Switchcore-L3) → Concentrador principal, responsável pelo roteamento Inter-VLAN (SVIs) e Gateway da rede.
- 3 Switches de Acesso (Gigabit/FastEthernet):
  - SW1 — Conecta Diretoria e TI.
  - SW2 — Conecta Financeiro e Suporte.
  - SW3 — Conecta Visitantes.
- 3 Servidores Dedicados (VLAN 60 - DMZ):
  - SRV-DHCP (Distribui IPs para toda a rede).
  - SRV-DNS (Resolve nomes internos/externos).
  - SRV-WEB (Hospeda o site corporativo).

## 3) Endereçamento
| VLAN | Nome | Rede | Gateway (SVI Core L3) |
|------|------|------|---------|
| 10 | Diretoria | 192.168.10.0/24 | 192.168.10.1 |
| 20 | TI | 192.168.20.0/24 | 192.168.20.1 |
| 30 | Financeiro | 192.168.30.0/24 | 192.168.30.1 |
| 40 | Suporte | 192.168.40.0/24 | 192.168.40.1 |
| 50 | Visitantes | 192.168.50.0/24 | 192.168.50.1 |
| 60 | Servidores | 192.168.60.0/24 | 192.168.60.1 |
| 99 | Management | 192.168.99.0/24 | 192.168.99.1 |

**Redes de Ponto-a-Ponto (Roteamento):**
- Core L3 ↔ Router-Interno: 10.0.0.0/30
- Router-Interno ↔ Router-Borda: 20.0.0.0/30
- Saída de Internet (R1): 200.0.0.0/24

**Servidores (IPs Fixos):**
- DHCP: 192.168.60.10
- DNS: 192.168.60.20
- WEB: 192.168.60.30

## 4) Topologia física e mapeamento de portas
**Router-Borda**
- g0/0 ↔ Router-Interno (IP: 20.0.0.1)
- g0/1 ↔ Internet (IP: 200.0.0.1) - Interface NAT Outside

**Router-Interno**
- g0/0 ↔ Router-Borda (IP: 20.0.0.2)
- g0/1 ↔ Switchcore-L3 (IP: 10.0.0.1)

**Switchcore-L3**
- g0/1 ↔ Router-Interno (Porta Roteada / No Switchport - IP: 10.0.0.2)
- f0/1 a f0/3 ↔ Trunks para os Switches de Acesso (SW1, SW2, SW3)
- f0/4 a f0/6 ↔ Servidores (Access VLAN 60)

**Switches de Acesso**
- **SW1:** f0/1-7 (VLAN 10), f0/8-15 (VLAN 20), f0/24 (Trunk p/ Core)
- **SW2:** f0/1-7 (VLAN 30), f0/8-15 (VLAN 40), f0/24 (Trunk p/ Core)
- **SW3:** f0/1-7 (VLAN 50), f0/24 (Trunk p/ Core)

## 5) Configurações conceituais

### Switch Core L3
- **Roteamento Interno:** Ativado via `ip routing`. Gateways configurados nas interfaces virtuais (SVI).
- **DHCP Relay:** Comando `ip helper-address 192.168.60.10` aplicado nas SVIs das VLANs 10, 20, 30, 40 e 50 para centralizar a distribuição de IPs.
- **Segurança:** ACL Estendida (`BLOQUEIO_VISITANTES`) aplicada na interface VLAN 50 (inbound).

### ACL_VISITANTES (VLAN 50)
1. Permite acesso ao SRV-DHCP (192.168.60.10)
2. Permite acesso ao SRV-DNS (192.168.60.20)
3. Permite acesso ao SRV-WEB (192.168.60.30)
4. Bloqueia tráfego para qualquer rede interna (`deny ip 192.168.50.0 0.0.0.255 192.168.0.0 0.0.255.255`)
5. Permite tráfego para qualquer outro destino (Internet).

### Roteamento e Borda (OSPF e NAT)
- **OSPF (Área 0):** Roda no Core L3, R-Interno e R-Borda, distribuindo as redes 10.x, 20.x e 192.168.x.
- **Rota Padrão:** Injetada pelo Router-Borda para o resto da rede via comando OSPF `default-information originate`.
- **NAT Overload (PAT):** Configurado no Router-Borda, traduzindo as redes internas `192.168.0.0/16` (Inside) para o IP público da interface g0/1 (Outside).

## 6) Checklist de testes

- ✅ PCs corporativos (VLANs 10, 20, 30, 40) recebem IP automaticamente via DHCP Relay.
- ✅ Roteamento Inter-VLAN operando: Diretoria consegue acessar arquivos da TI (e vice-versa).
- ✅ Todos os PCs conseguem converter nomes via servidor DNS e abrir a página HTTP corporativa.
- ✅ PC de Visitantes (VLAN 50) consegue pingar o servidor WEB e acessar a Internet.
- ❌ PC de Visitantes tenta pingar o gateway da Diretoria (`192.168.10.1`) e recebe "Destination host unreachable" (ACL funcionando).
- ✅ Tráfego alcança a rede 200.0.0.x comprovando a eficácia do OSPF, NAT e da Rota Padrão.

## 7) Possíveis melhorias futuras (Próximos passos)
- Segmentar ainda mais a rede através da implementação do conceito *Zero Trust* (ex: impedir que o Financeiro acesse livremente a Diretoria usando novas ACLs).
- Implementar Port Security nos switches de acesso para mitigar ataques de MAC Flooding.
- Configurar SSH em todos os equipamentos para gerenciamento remoto seguro (substituindo o uso do cabo Console).
- Configurar HSRP ou VRRP (First Hop Redundancy Protocol) adicionando um segundo Switch Core L3 para alta disponibilidade.