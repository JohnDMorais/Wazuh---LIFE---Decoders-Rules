# Wazuh - LIFE - Decoders & Rules

## Visão Geral

Este repositório reúne decoders e rules customizados do Wazuh, desenvolvidos para o ambiente corporativo, contemplando:

* Decoder para eventos do ManageEngine ADAuditPlus.
* Decoder para logs de DHCP da Microsoft (MS-DHCP).
* Override de decoder para eventos de firewall Fortigate.
* Rules de correlação para ADAuditPlus, MS-DHCP, eventos de segurança do Windows e Office 365.

O objetivo é padronizar a interpretação de logs de diferentes fontes e ampliar a visibilidade de eventos de autenticação, rede e ameaças no SIEM.

## Estrutura do Repositório

```text
.
├── decoders/
│   ├── admanagerplus/
│   │   └── ADAuditPlus.xml
│   ├── dhcp/
│   │   └── dhcp_decoders.xml
│   └── fortigate/
│       └── override_fortigate-firewall-v6.xml
│
├── rules/
│   ├── admanger_plus/
│   │   └── override_admanagerplus.xml
│   ├── dhcp/
│   │   └── override_ms_dhcp.xml
│   ├── eventos_windows/
│   │   └── windows_security_custom_logoff.xml
│   └── office365/
│       └── override_office365.xml
│
└── README.md
```

## Decoders

### ADAuditPlus

Interpreta os logs enviados pelo ManageEngine ADAuditPlus, extraindo campos relevantes para posterior correlação.

Informações extraídas:

* Usuário
* Motivo do evento
* Origem (IP)
* Categoria
* Perfil do relatório

### MS-DHCP

Decoder para logs de DHCP da Microsoft, responsável pela extração do ID do registro de log como base para as rules de correlação.

### Fortigate

Override de decoder do firewall Fortigate, restrito a um location específico, para extração de campos adicionais (`devid`, `devname`) não cobertos pelo decoder padrão.

## Rules

As rules utilizam os eventos decodificados para gerar alertas e reclassificar a severidade no Wazuh.

* **ADAuditPlus**: falha de autenticação de usuário.
* **MS-DHCP**: início do serviço de log e novo lease de IP concedido a um cliente.
* **Eventos Windows**: logoff de contas de serviço (padrão de nomenclatura `SRV`/`SNE`), com descarte do restante dos eventos de logoff.
* **Office 365**: reclassificação de eventos de Threat Intelligence (impersonation e bloqueio de malware/phishing/URL).

As regras podem ser adaptadas conforme a política de monitoramento da organização.

## Requisitos

* Wazuh Manager
* Fontes de log integradas: ADAuditPlus, MS-DHCP, Fortigate, Windows (eventos de segurança), Office 365
* Permissão para inclusão de decoders e rules customizadas

## Convenções

* IDs de rule customizadas na faixa `100000+` / `191000+` (overrides), evitando conflito com regras nativas do Wazuh.
* Arquivos `override_*.xml` sobrescrevem ou complementam decoders/rules já existentes, usando `<parent>` ou `<if_sid>` apontando para o ID original.
* Grupos de compliance (PCI DSS, GDPR, HIPAA, NIST 800-53, TSC) são mapeados nas tags `<group>` quando aplicável.

## Objetivo do Projeto

Este projeto busca padronizar a ingestão e correlação de logs de múltiplas fontes, reduzindo esforço operacional e ampliando a visibilidade de eventos de segurança no SIEM. As automações podem servir de base para expansão a novas fontes de log e adequação a requisitos de compliance.

## Licença

Este projeto pode ser utilizado, adaptado e expandido conforme a necessidade da organização, respeitando as políticas internas de segurança e governança.

> Este repositório não armazena credenciais, senhas ou tokens. Nenhum dado sensível deve ser incluído em decoders, rules ou logs de exemplo.
