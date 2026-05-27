- Banco de dados 'ecommerce_tcc'
- Tabela 'clientes'
1. 'app_user': Permissões de leitura e escrita (SELECT, INSERT, UPDATE). Não pode deletar tabelas.
2. 'auditor_user': Permissão de apenas leitura (SELECT) para auditoria de redes.
## Cenário 2: Disponibilidade e Backup Automatizado
- Implementando script em Shell Bash para realizar 'pg_dump' automatizado do banco 'ecommerce_tcc'
- Uso do comando 'su - postgres' para respeitar a autenticação Peer do Debian 13.
- Política de retenção de dados configurada para manter os arquivos por 7 dias utilizando o agendador 'cron'.
