# Wazuh - LIFE - Decoders & Rules

Decoders e rules customizados do Wazuh usados no ambiente LIFE/Alvo. Cobrem AD Manager Plus, DHCP (MS-DHCP), Fortigate, Office 365 e eventos de segurança do Windows.

## Estrutura

```
decoders/
  admanagerplus/
    ADAuditPlus.xml           # Decoders para logs do ManageEngine ADAuditPlus (usuário, motivo, origem, categoria, perfil de relatório)
  dhcp/
    dhcp_decoders.xml         # Decoder para logs de DHCP da Microsoft (extração de ID do registro)
  fortigate/
    override_fortigate-firewall-v6.xml   # Override de decoder do firewall Fortigate (devid, devname), restrito a um location específico

rules/
  admanger_plus/
    override_admanagerplus.xml   # Regra 600000 - falha de autenticação (ADAuditPlus)
  dhcp/
    override_ms_dhcp.xml         # Regras 100721-100723 - início de log, novo lease de IP etc. (MS-DHCP)
  eventos_windows/
    windows_security_custom_logoff.xml   # Regras 100137-100139 - logoff de contas de serviço (SRV/SNE) e descarte do restante
  office365/
    override_office365.xml       # Regras 191556-191557 - reclassificação de eventos de Threat Intelligence do Office 365
```

## Uso

1. Copiar os decoders para `/var/ossec/etc/decoders/` no Wazuh manager.
2. Copiar as rules para `/var/ossec/etc/rules/`.
3. Validar a configuração:
   ```
   /var/ossec/bin/wazuh-logtest
   ```
4. Reiniciar o manager:
   ```
   systemctl restart wazuh-manager
   ```

## Convenções

- IDs de rule customizadas na faixa `100000+` / `191000+` (overrides) para evitar conflito com regras nativas do Wazuh.
- Arquivos `override_*.xml` sobrescrevem ou complementam decoders/rules já existentes (usam `<parent>` ou `<if_sid>` apontando para o ID original).
- Grupos de compliance (PCI DSS, GDPR, HIPAA, NIST 800-53, TSC) são mapeados nas tags `<group>` quando aplicável.

> Este repositório não armazena credenciais, senhas ou tokens. Nenhum dado sensível deve ser incluído em decoders, rules ou logs de exemplo.
