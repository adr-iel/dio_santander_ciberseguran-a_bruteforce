# dio_santander_ciberseguran-a_bruteforce
Desafio de projeto DIO carreira Cibersegurança Santander referente a bruteforce usando Kali Linux e a ferramenta Medusa

1. Cenário 1 — Auditoria no Serviço FTP da Metasploitable 2
   
  1.1 Descoberta de portas

  Primeiro, realizamos a varredura para identificar portas abertas:
  
    nmap -sV <IP_METASPLOITABLE>
  
  
  Isso permitiu observar serviços vulneráveis, incluindo o FTP (porta 21) ativo.

  1.2 Preparação de Wordlists

Criamos duas listas simples:

    users.txt
    
    passwords.txt

Esses arquivos simulam listas vazadas ou fracas para teste.

  1.3 Ataque de autenticação simulada (Medusa)

Executamos o Medusa apontando:

    IP da máquina alvo
    
    lista de usuários
    
    lista de senhas
    
    serviço FTP
    
    threads paralelas para acelerar o teste

  1.4 Resultado

Após algumas tentativas, uma combinação válida foi encontrada, permitindo acesso ao serviço FTP.
Em ambiente real, isso seria uma vulnerabilidade crítica.

2. Cenário 2 — Análise de força bruta em formulários web (DVWA)
  2.1 Observando o comportamento do formulário

Acessamos o DVWA no nível “Low Security”.

Através das DevTools → Aba Network, analisamos:

    O formulário utiliza método POST

Possui parâmetros:

    username
    
    password
    
    Login
    
    Quando a autenticação falha, retorna: "Login failed"

Essas informações ajudam a entender como um atacante tentaria automatizar requisições — e como um defensor pode prever e bloquear esse comportamento.

  2.2 Testes com Medusa (HTTP Form Mode)

Usamos novamente nossas listas simples e simulamos tentativas automatizadas.

O foco aqui é entender:

    Como um serviço web pode ser abusado com tentativas repetidas
    
    Como a aplicação retorna mensagens úteis (ou perigosas)
    
    Como limitar tentativas e aplicar mecanismos de proteção

  2.3 Resultado

O Medusa identificou combinações corretas enviando os parâmetros adequados.
Isso demonstra como formulários sem proteção podem ser alvos fáceis.

3. Cenário 3 — Password Spraying e Enumeração de Usuários em SMB

   3.1 Enumeração de usuários

    Em ambientes vulneráveis como Metasploitable 2, é possível obter informações via serviços SMB inseguros.
    
    Objetivo aqui:
    Entender como nomes de usuário podem ser expostos inadvertidamente.

  3.2 Password Spraying

Diferente do brute force tradicional, o spraying tenta uma única senha para vários usuários, reduzindo risco de bloqueio.

4. Medidas de Mitigação

Com base nos cenários estudados:

  4.1 Fortalecer políticas de senha

    usar MFA
    
    exigir comprimento mínimo
    
    evitar senhas comuns

  4.2 Limitar tentativas de autenticação

    bloqueio temporário
    
    captcha
    
    backoff exponencial

  4.3 Ocultar mensagens detalhadas de erro

    Mensagens como “usuário inexistente” ajudam atacantes — sempre usar respostas genéricas.

  4.4 Monitorar logs

    Tentativas repetidas podem indicar ataque.

  4.5 Atualizar e endurecer serviços

    FTP deve ser substituído por SFTP/FTPS
    
    SMB antigo (como SMBv1) deve ser desativado
    
    aplicações web devem usar frameworks atualizados
