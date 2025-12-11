# 💀 Project Deathstar: CTF Walkthrough


![Status](https://img.shields.io/badge/Status-Rooted-brightgreen?style=for-the-badge)
![Security](https://img.shields.io/badge/Focus-Web%20Enumeration%20%26%20Brute%20Force-red?style=for-the-badge)
![OS](https://img.shields.io/badge/OS-Linux%20(Debian)-blue?style=for-the-badge)

> **Uma jornada técnica explorando vulnerabilidades web, criação de wordlists contextuais e quebra de criptografia.**

---

## 🎯 Objetivo
Comprometer o servidor alvo explorando falhas de configuração e vulnerabilidades de aplicação web, escalando privilégios de acesso externo até a obtenção de acesso **ROOT**.

---

## 🛠️ Tech Stack & Ferramentas
| Categoria | Ferramentas Utilizadas |
| :--- | :--- |
| **Reconhecimento** | `Nmap`, `Gobuster`, `Robots.txt`, `HTML Inspection` |
| **Weaponization** | `CeWL` (Wordlist Generator), `Crunch` (Numeric Generator) |
| **Ataque** | `Hydra` (Brute-Force), `Wget` |
| **Cracking** | `John the Ripper` (Hash cracking) |

---

## ⚡ Kill Chain (Passo a Passo)

### 1. Reconhecimento & Enumeração
A varredura inicial revelou as portas **22 (SSH)** e **80 (HTTP)**. A análise do arquivo `robots.txt` expôs o diretório oculto `/0VVNZMMGM1`.

- **Vulnerabilidade Encontrada:** *Information Disclosure* em comentários HTML.
- **Exploit:** Credenciais hardcoded baseadas em hash de arquivo PDF (`admin`).

### 2. Movimentação Lateral (Web)
A aplicação guiava o invasor através de diferentes portais de login (`/restrict98712`, `/millenium3000falcon`).

#### 🔓 Usuário: Darth
- **Técnica:** Dicionário Contextual.
- **Execução:** Utilizei o **CeWL** para extrair palavras da página `estreladamorte2023.html`.
- **Comando:** `hydra -l darth -P palavras_estrela.txt ...`
- **Senha Descoberta:** `Chewbacca`

#### 🔓 Usuário: Kenobi
- **Técnica:** Força Bruta Numérica.
- **Dica:** O código-fonte indicava uma senha de 7 dígitos.
- **Execução:** Gerei uma wordlist com **Crunch** (`0000000-9999999`).
- **Senha Descoberta:** `0009165`

### 3. Escalação de Privilégios (System Ownage)
O acesso ao painel do Kenobi revelou a falha crítica.

- **Vulnerabilidade:** Exposição do arquivo `/etc/shadow` via script PHP.
- **Ação:** Extração do hash SHA-512 da usuária `leia`.
- **Cracking:** Quebra do hash utilizando **John the Ripper** (`rockyou.txt`).
- **Senha SSH:** `catherine`


## 🛡️ Mitigação & Correções Recomendadas
Para proteger este sistema, as seguintes ações são necessárias:
1.  **Sanitização:** Remover comentários de desenvolvimento do código em produção.
2.  **Permissões:** Restringir leitura do `/etc/shadow` pelo usuário `www-data`.
3.  **Senhas:** Implementar políticas de complexidade de senha e rate-limiting no login.

---
*Lab realizado para fins educacionais e de treinamento em Ethical Hacking.*
