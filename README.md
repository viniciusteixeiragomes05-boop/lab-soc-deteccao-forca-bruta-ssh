# 🛡️ Lab SOC: Detecção e Mitigação de Força Bruta SSH no Linux

## 📋 Objetivo
Simular um ataque de força bruta (Brute Force) contra o serviço SSH em ambiente Linux, realizar a investigação forense dos eventos nos logs do sistema e implementar uma regra de resposta automatizada para contenção do atacante.

## 🛠️ Ambiente e Ferramentas
* **Virtualização:** Oracle VirtualBox (Rede em Modo Bridge)
* **Sistema Alvo:** Kali Linux (`192.168.2.108`)
* **Host Atacante:** Windows PowerShell (`192.168.2.105`)
* **Serviços e Defesa:** OpenSSH Server, UFW e Fail2Ban
* **Análise de Logs:** `journalctl` / `systemd`

---

## 🔍 Etapas Executadas e Evidências

### 1. Simulação do Ataque (Brute Force SSH)
Execução de múltiplas tentativas de autenticação inválida via SSH a partir de um host externo utilizando o usuário `invasor`:

<img width="871" height="521" alt="65e8b217-8508-4259-90ba-0b771f320992" src="https://github.com/user-attachments/assets/9970ca03-0d9f-42cf-a9ee-935b52f1af74" />


### 2. Detecção e Análise Forense (Monitoramento)
Identificação dos Indicadores de Comprometimento (IOCs), origem do tráfego (`192.168.2.105`) e usuário alvo através da filtragem de logs do serviço SSH (`sudo journalctl -u ssh | grep "Failed password"`):

<img width="700" height="446" alt="1c1bd48c-8003-473b-97df-e685e573bfa2" src="https://github.com/user-attachments/assets/321977ee-30fa-43af-8d42-c4429e8ddf1c" />


### 3. Contenção e Resposta Automatizada (Fail2Ban)
Configuração de política customizada em `/etc/fail2ban/jail.local` (`maxretry = 3`, `bantime = 600s`, `backend = systemd`). Após atingir o limite de tentativas, o IP atacante foi banido automaticamente:

<img width="697" height="453" alt="b365f3f6-54a2-4859-b26d-f2092d25ca7f" src="https://github.com/user-attachments/assets/087fdc17-297e-435a-b630-c8ddbbe01fc7" />


---

## 📌 Recomendações de Governança (ISO 27001 / Boas Práticas)
Para mitigação definitiva em ambiente corporativo, recomenda-se:
1. Desabilitar autenticação por senha no SSH (`PasswordAuthentication no`), exigindo exclusivamente chaves criptográficas (SSH Keys).
2. Alterar a porta padrão do serviço SSH e restringir o acesso via VPN corporativa.
3. Encaminhar os logs do `journald` / `fail2ban` para um SIEM centralizado (ex: Wazuh ou Splunk) para correlação de alertas.# lab-soc-deteccao-forca-bruta-ssh
Simulação de ataque de força bruta SSH em Linux, análise forense de logs e resposta automatizada com Fail2Ban e UFW.
