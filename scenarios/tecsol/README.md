# Cenário de Rede — TecSol Ltda 

## 1) Contexto
A TecSol Ltda possui um escritório com 2 andares. Objetivo: montar uma rede pequena, mas realista, com separação de departamentos via VLANs, um servidor em DMZ e uma rede de gerenciamento para os switches.

- Administração (VLAN 10) e Suporte (VLAN 20) podem se comunicar livremente.
- Vendas (VLAN 30) tem acesso restrito: somente serviços internos/DMZ e saída para internet.

## 2) Equipamentos
- 1 Roteador (R1) → Router-on-a-Stick com subinterfaces para cada VLAN.
- 3 Switches Gigabit:
  - SW1 (Core) — conecta ao R1, aos outros switches e ao servidor.
  - SW2 (Acesso - Andar 1) — conecta PCs da Administração e Suporte.
  - SW3 (Acesso - Andar 2) — conecta PCs de Vendas.
- 1 Servidor (SRV1) → DNS + HTTP (VLAN 50 - DMZ).
- 13 PCs:
  - Administração (VLAN 10): 4 PCs
  - Suporte (VLAN 20): 3 PCs
  - Vendas (VLAN 30): 5 PCs
  - Gerência (VLAN 99): 1 PC (PC-MGMT para gerenciamento dos switches)

## 3) Endereçamento
| VLAN | Nome | Rede | Gateway |
|------|------|------|---------|
| 10 | Administração | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Suporte | 192.168.20.0/24 | 192.168.20.1 |
| 30 | Vendas | 192.168.30.0/24 | 192.168.30.1 |
| 50 | DMZ | 192.168.50.0/24 | 192.168.50.1 |
| 99 | Gerência | 192.168.99.0/24 | 192.168.99.1 |
| 999 | Native | (trunks) | — |

- Servidor SRV1: 192.168.50.10/24 (fixo, gateway 192.168.50.1)
- Gateways (.1) → subinterfaces do R1
- DHCP → VLANs 10, 20, 30, 99

## 4) Topologia física e mapeamento de portas (Gigabit)
**R1**
- Gi0/0 ↔ SW1 Gi0/1 (trunk)

**SW1 (Core)**
- Gi0/1 ↔ R1 Gi0/0 (trunk)
- Gi0/2 ↔ SW2 Gi0/1 (trunk)
- Gi0/3 ↔ SW3 Gi0/1 (trunk)
- Gi0/4 ↔ SRV1 (access VLAN 50)
- Gi0/9 ↔ PC-MGMT (access VLAN 99)

**SW2 (Andar 1)**
- Gi0/1 ↔ SW1 Gi0/2 (trunk)
- Gi0/2~Gi0/5 → PCs Administração (VLAN 10)
- Gi0/6~Gi0/8 → PCs Suporte (VLAN 20)

**SW3 (Andar 2)**
- Gi0/1 ↔ SW1 Gi0/3 (trunk)
- Gi0/2~Gi0/6 → PCs Vendas (VLAN 30)

## 5) Configurações conceituais
### VLANs
- Criadas em todos os switches: 10, 20, 30, 50, 99, 999
- VLAN 999 como native em trunks

### Trunks
- SW1↔R1, SW1↔SW2, SW1↔SW3
- VLANs permitidas: 10,20,30,50,99
- VLAN 999 como native

### SVIs (gerência)
| Switch | VLAN 99 IP |
|--------|------------|
| SW1 | 192.168.99.2 |
| SW2 | 192.168.99.3 |
| SW3 | 192.168.99.4 |
- Gateway: 192.168.99.1 (R1)

### Roteador (R1)
- Subinterfaces dot1Q para VLANs 10,20,30,50,99,999
- Pools DHCP configurados para 10,20,30,99
- ACL aplicada na subinterface VLAN 30 (Vendas):

**ACL_VENDAS**
1. Permite DHCP: bootpc/bootps
2. Permite acesso ao servidor DMZ (192.168.50.10): HTTP, HTTPS, DNS
3. Bloqueia acesso às redes internas (10, 20, 99)
4. Permite tráfego para internet

### Servidor SRV1
- IP fixo: 192.168.50.10/24
- Gateway: 192.168.50.1
- DNS: ativo, www.empresa.local → 192.168.50.10
- HTTP: página simples

### PCs
- DHCP ativado
- PC-MGMT: IP fixo ou via DHCP na VLAN 99

## 6) Checklist de testes

- ✅ PCs Admin (VLAN 10) recebem IP 192.168.10.x e pingam 192.168.10.1  
- ✅ PCs Suporte (VLAN 20) recebem IP 192.168.20.x e pingam 192.168.20.1  
- ✅ PCs Vendas (VLAN 30) recebem IP 192.168.30.x e pingam 192.168.30.1  
- ✅ PC-MGMT acessa SVIs dos switches (192.168.99.2/3/4)  
- ✅ Todos PCs conseguem pingar 192.168.50.10 (servidor DMZ)  
- ✅ PC-Vendas acessa http://www.empresa.local (HTTP OK)  
- ❌ PC-Vendas não consegue pingar PCs da VLAN 10/20/99 (ACL funcionando)  
- ✅ PCs Admin e Suporte comunicam-se entre si normalmente  

## 7) Exercícios extras
- Bloquear apenas ICMP da VLAN 30 para VLAN 10, mas permitir HTTP/HTTPS
- Ativar SSH no roteador e permitir apenas acesso da VLAN 99
- Adicionar servidor FTP e permitir apenas VLAN 10 e 20
- Configurar DHCP relay em servidor na DMZ
- Simular falha SW1↔SW3 e fazer troubleshooting

