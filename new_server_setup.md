# New Server Setup in Sagikos
Log in as root.
1. ``passwd``
      * use at least 15 characters strong generated password at https://passwordsgenerator.net/?length=16&symbols=1&numbers=1&lowercase=1&uppercase=1&similar=1&ambiguous=1&client=1&autoselect=1

1. ``hostnamectl set-hostname desld0001.sagikos.com``
1. ``pico /etc/hosts``
      * edit hostname for ipv4 and ipv6.     
1. ``pico /etc/default/useradd``
      * ``SHELL=/bin/bash``
1. ``pico /etc/ssh/sshd_config``
      * ``ClientAliveInterval 10``
      * ``ClientAliveCountMax 30``
3. ``apt update && apt upgrade -y && apt install sudo git ufw screen sqlite php-zip letsencrypt dnsutils bzip2 make -y``
13. ``ufw allow 20,21,22,25,53,80,110,143,587/tcp``
14. ``ufw allow 953,993,995,443,6277,10000,20000/tcp``
15. ``ufw allow 53,10000,20000/udp``
16. ``ufw enable``
17. ``groupadd sagikos``
18. Create a new sudo account for yourself. Replace xy1234 with your Sagikos employee username:
     1. ``useradd xy1234 -m -g sagikos -G sudo``
21. ``passwd xy1234``. 
      * Use the above password generator link again. Obviously with a separate password.
23. ``reboot``
24. From https://github.com/aristocratos/btop/releases/latest download x86_64 file.
25. ``tar -xjvf btop-*.tbz --one-top-level``
26. cd in to that new directory.
27. ``sudo make install``
28. ``cd`` to get into your home directory.
29. ``wget https://software.virtualmin.com/gpl/scripts/install.sh``
30. ``sudo /bin/sh install.sh -f``
31. Open https://desld0001.sagikos.com:10000 in a local web browser and log in as ``sagikos_oper`` and the generated password.
32. Update the password restrictions per corporate policy at https://desld0001.sagikos.com:10000/acl/edit_pass.cgi?xnavigation=1 (Webmin > Webmin Users > Password Restrictions:
     1.  Minimum password length: 15.
     2.  Days before password must be changed: 90.
     3.  Days before un-changed password locks account: 7.
     4.  Disallow passwords containing username: Yes.
     5.  Disallow dictionary word passwords: Yes.
     6.  Number of old passwords to reject: 1 passwords.
33. Webmin > Webmin users > *Create a New Webmin group*.
     1. Group name: *opers*
     2. Available Webmin modules: *Select all*
34. Webmin > Webmin users > *Create a New Webmin group*.
     1. Group name: *employees*
35. Webmin > Webmin users > *Create a New Webmin group*.
     1. Group name: *contractors*
36. https://desld0001:10000/fail2ban/?xnavigation=1
     * Start at boot? Yes. Then click the button.
37. Virtualmin > System Settings > Account Plans > Default Plan > Limit on number of virtual servers: *Unlimited*. Save.
38. Virtualmin > System Settings > Features and Plugins > Check *SQLite Databases* and *Git repositories*. Save.
39. Virtualmin > System Settings > Server Templates > Default Settings > 
     1. Administration user > Chroot jail new domain Unix users: Yes. Save and next.
     2. BIND DNS domain > 
          1. Address records for new domains: uncheck m.${DOM}.
          2. Ensure *Additionally manually configured nameservers* is correct: *ns2.sagikos.com*.
          3. Create DNSSEC key and sign new domains: Yes. 
          4. Save and next.
     3. Mail for domain >
          1. Email message to send upon server creation: Message below ..
          2. Default quota for mail users: 300 MiB.
     4. Spam filtering >
          1. Automatically delete old spam: 30 days.
40. Virtualmin > System Settings > Virtualmin Configuration >
     1. SSL Settings >
          1. Redirect HTTP to HTTPS by default: Yes. Save.
41. Virtualmin > Email Settings > 
     1. Mail Client Configuration: Enable mail client autoconfiguration: Yes. Save.
     2. New Mailbox Email > 
          1. Send email: Yes.
          2. Send email to: check both *User's mailbox* and *Virtual server owner*. Save.
     3. Updated Mailbox Email: Send email to: check both *User's mailbox* and *Virtual server owner*. Save.

41. Virtualmin > Create Virtual Server.
     1. Domain name: *desld0001.sagikos.com*.
     2. Description: *default virtual server*.
     3. Administration password: Generate new password.
     4. Enabled features:
          * Setup DNS zone
          * Setup Apache website
          * Setup Apache SSL website
          * Create MariaDB database
          * Accept mail for domain
          * Setup spam filtering
          * Setup virus filtering
          * Setup Webalizer for web logs
          * Create Webmin login
          * Allow Git repositories.

## User Customization
For each unix user you want to customize:
1. ``bash -c "$(wget https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh -O -)"``
2. ``pico ~/.bashrc``
     * Change ``OSH_THEME="font"`` to ``OSH_THEME="powerline"`` 
     * Add the bottom add:
          * ``alias sai="sudo apt install -y"``
          * ``alias ls='lsd'``
3. ``source ~/.bashrc``
24. On your local machine:
     * ``ssh-keygen``      
     * ``ssh-copy-id sagikos_oper@desld0001.sagikos.com``
     * ``ssh sagikos_oper@desld0001.sagikos.com``
30. ``curl https://sh.rustup.rs -sSf | sh``
31. ``source $HOME/.cargo/env``
32. ``cargo install lsd``

## Docker
1. ``apt -y install ca-certificates curl gnupg lsb-release``
2. ``curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg``
3. ``echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null``
4. ``apt update``
5. ``apt -y install docker-ce docker-ce-cli containerd.io``
6. 
