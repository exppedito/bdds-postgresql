# 🛡️ Lab de Administração e Segurança em PostgreSQL com Incus (Debian 13)

Este repositório armazena a documentação, scripts e cenários práticos desenvolvidos durante meus estudos de infraestrutura de servidores, redes e bancos de dados. Todo o ambiente foi construído utilizando contêineres **Incus** rodando **Debian 13 (Trixie)**, com foco em boas práticas de mercado, automação e segurança da informação.

---

## 🏗️ Arquitetura do Ambiente

O laboratório simula uma infraestrutura de rede local enxuta e isolada:
- **Hospedeiro (Host):** Máquina Virtual Debian 13.
- **Contêiner de Banco de Dados:** Instância Incus isolada (`pg-server`).
- **SGBD:** PostgreSQL 17.

---

## 🛠️ Linha do Tempo e Resolução de Problemas (Troubleshooting)

Durante a inicialização do ambiente, enfrentamos e solucionamos desafios reais de administração de sistemas Linux:
- **Concorrência no Gerenciador de Pacotes (`APT`):** Resolução de travamentos causados pelo serviço em segundo plano `packagekitd` (File Lock no banco de dados do dpkg).
- **Dependências Mínimas em Contêineres:** Identificação e instalação manual de pacotes utilitários do sistema operacional (como o agendador `cron`), ausentes por padrão em imagens limpas/mínimas do Incus.

---

## 🔒 Cenário 1: Controle de Acessos e Princípio do Menor Privilégio (PoLP)

Implementação de segurança a nível de banco de dados para uma aplicação simulada de E-commerce (`ecommerce_tcc`), garantindo a segregação de funções (SoD) baseada nas normas ISO 27001 e diretrizes do NIST.

### Estrutura Criada:
- **Banco de Dados:** `ecommerce_tcc`
- **Tabela Crítica:** `clientes` (Simulação de dados sensíveis).

### Papéis e Privilégios (RBAC):
1. **`app_user` (Desenvolvedor/Aplicação):** Permissões estritas de Manipulação de Dados (`SELECT`, `INSERT`, `UPDATE`). Bloqueado contra comandos de destruição estrutural (`DROP`, `ALTER`).
2. **`auditor_user` (Segurança/Redes):** Permissão exclusiva de leitura (`SELECT`). Ideal para auditoria de tráfego e relatórios sem riscos de alteração de integridade.

---

## 💾 Cenário 2: Disponibilidade e Plano de Continuidade de Negócios

Criação de uma rotina de tolerância a falhas para evitar a perda catastrófica de dados comerciais.

### Recursos Implementados:
- **Script de Automação em Shell Bash (`/backups/backup_banco.sh`):** Realiza o dump lógico do banco de dados (`pg_dump`) e compactação em tempo real (`gzip`).
- **Autenticação Segura via Unix Sockets:** Configuração de execução atrelada ao usuário nativo `postgres` para respeitar a política de autenticação de segurança `Peer` do Debian.
- **Política de Retenção de Dados:** Limpeza automatizada via comando `find` para descartar cópias de segurança com mais de 7 dias, evitando o esgotamento do armazenamento do contêiner.
- **Agendamento Crontab:** Automação completa configurada via `cron` para disparar a rotina de segurança diariamente às 00:00.

---

## 🚀 Como Executar o Ambiente

### Requisitos
- Git configurado com chave SSH.
- Contêiner Incus funcional com Debian 13.

### Comandos Rápidos de Verificação:
```bash
# Verificar status do banco de dados
systemctl status postgresql@17-main

# Verificar o agendador de automações
crontab -l
