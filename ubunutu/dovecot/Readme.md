# Email Server Setup

Assign hostname

```bash
sudo hostnamectl set-hostname mail.your-domain.com
# Add to your etc/hosts
127.0.0.1 mail.your-domain.com localhost

# verify
hostname –fqdn OR hostname -f

# DNS list
systemd-resolve --status

# Update, clean and reboot system
sudo sh -c "apt update;apt full-upgrade -y;apt autoremove -y;apt autoclean;shutdown -r 1"

```
## Installation of iRedMail 

Get iRedMail Download: [Download iRedMail](https://www.iredmail.org/download.html)

Go with the install defaults 
e.g. /var/vmail , Nginx , MariaDB , yourDBpassword , your-email-domain.com , yourEmailAdminPassword , choose minimum of Roundcubeemail, iRedAdmin and you do not need SOGo nor Fail2ban, allow firewall entry changes

### To allow insecure POP3/IMAP connections, edit dovecot.conf
```bash
sudo nano /etc/dovecot/dovecot.conf
```

### Configure two settings as below
ssl=yes
disable_plaintext_auth=no

### Allow insecure SMTP port 25 connections
```bash
sudo nano /etc/postfix/main.cf
```
### uncomment below two lines
smtpd_sasl_auth_enable = yes
smtpd_sasl_security_options = noanonymous
### leave the commented line
#smtpd_tls_auth_only=yes
