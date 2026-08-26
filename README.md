<div align="center">

# 🌐 Networks

**Projetos, configurações e laboratórios práticos focados em infraestrutura de redes de computadores.**

[![Cisco](https://img.shields.io/badge/Cisco-Networking-1D71B8?style=for-the-badge&logo=cisco&logoColor=white)](https://www.cisco.com/)
[![Packet Tracer](https://img.shields.io/badge/Tool-Packet%20Tracer-00507d?style=for-the-badge)](https://www.netacad.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-brightgreen?style=for-the-badge)](#)

<p align="center">
  <a href="#-visão-geral">Visão Geral</a> •
  <a href="#-tecnologias">Tecnologias</a> •
  <a href="#-estrutura-do-repositório">Estrutura</a> •
  <a href="#-cenários-e-labs">Laboratórios</a> •
  <a href="#-como-usar">Como Usar</a> •
  <a href="#-autor">Autor</a>
</p>

</div>

---

## 🔭 Visão Geral

O **Network Zone** é um repositório dedicado aos estudos e projetos práticos na área de **Redes de Computadores**. Ele reúne topologias, arquivos de configuração e cenários simulados focados em infraestrutura, com ênfase em switching, roteamento, VLANs e boas práticas de conectividade.

Através de cenários com empresas fictícias e laboratórios estruturados, o repositório demonstra a aplicação prática de conceitos fundamentais para a arquitetura de redes corporativas.

### 💡 Por que este repositório existe?
> Consolidar o aprendizado técnico através de laboratórios práticos, simulando demandas de infraestrutura do mundo real com o apoio de ferramentas de simulação como o **Cisco Packet Tracer**.

---

## 🛠️ Tecnologias e Ferramentas

| Categoria | Tecnologia / Recurso | Finalidade no Repositório |
| :--- | :--- | :--- |
| **Simulação** | Cisco Packet Tracer | Ambiente para montagem de topologias e testes de conectividade |
| **Equipamentos** | Switches & Roteadores | Configuração de portas, modos de acesso e entroncamento (*trunk*) |
| **Rede L2 / L3** | VLANs, Trunks, STP | Segmentação de rede, entroncamento e prevenção de loops |
| **Roteamento** | Inter-VLAN (*Router-on-a-Stick*) | Comunicação entre diferentes sub-redes/VLANs |
| **Segurança** | ACLs (Access Control Lists) | Controle de tráfego e filtragem de pacotes |
| **Serviços** | DHCP, DNS, HTTP | Configuração de serviços essenciais de rede |

---

## 📁 Estrutura do Repositório

```bash
networks/
├── ccna2/                 # Anotações, resumos e estudos teóricos (ex: Módulos CCNA)
├── networks-configs/      # Configurações isoladas (Router-on-a-Stick, VoIP)
├── scenarios/             # Cenários e topologias de empresas fictícias (FankaTech, TecSol)
└── README.md              # Documentação principal do repositório
```

---

## 🏢 Cenários e Laboratórios

Cada pasta de laboratório ou cenário contém seus próprios arquivos de configuração e documentação:

- **`networks-configs/`**: Exemplos práticos isolados (ex: roteamento inter-VLAN, VoIP).
- **`scenarios/`**: Estudos de caso baseados em empresas fictícias (**FankaTech**, **TecSol**), contendo topologias visuais, diagramas e comandos aplicados.

---

## ⚙️ Como Usar

1. **Baixe ou clone** o repositório em sua máquina:
   ```bash
   git clone https://github.com/Erick-Fanka/networks.git
   ```
2. Abra o **Cisco Packet Tracer** ou o simulador de sua preferência.
3. Navegue até a pasta do laboratório desejado (ex: `networks-configs/router-on-a-stick/`) para consultar os arquivos de configuração (`.cfg`).
4. Utilize os comandos como referência para implementar as topologias descritas nos diagramas (`.png`).

---

## 📄 Licença

Este projeto está sob a licença **MIT**. Consulte o arquivo [LICENSE](LICENSE) para obter detalhes.

---

## 👨‍💻 Autor

<table align="center">
  <tr>
    <td align="center">
      <a href="https://github.com/Erick-Fanka">
        <img src="https://avatars.githubusercontent.com/Erick-Fanka" width="120px;" alt="Foto de Erick Fanka" style="border-radius: 50%;" />
      </a>
      <br />
      <strong>Erick Fanka</strong>
    </td>
    <td>
      <strong>Competidor WorldSkills | Cloud Computing | Redes | AWS | Python</strong><br />
      <a href="https://www.linkedin.com/in/erick-fanka">
        <img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn" />
      </a>
      <a href="https://github.com/Erick-Fanka">
        <img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>
  </tr>
</table>

---
> 💡 **Nota:** Este repositório é voltado para **fins educacionais e práticas de laboratório**. Nenhuma empresa real está associada aos cenários aqui descritos.
