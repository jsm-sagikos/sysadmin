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
3. ``apt update && apt upgrade -y && apt install sudo git ufw screen sqlite -y``
12. ``sudo ufw allow 22/tcp``
13. ``sudo ufw allow 20,21,25,53,80,110,143,587,953,993,995,443,6277,10000,20000/tcp``
14. ``sudo ufw allow 53,10000,20000/udp``
15. ``sudo ufw enable``
16. ``groupadd sagikos``
17. ``useradd sagikos_oper -m -g sagikos -G sudo``
18. Create a new oper account for yourself. Replace xy1234 with your Sagikos employee username:
     1. ``useradd xy1234_oper -m -g sagikos -G sudo``
20. ``passwd sagikos_oper``. 
      * Use the above password generator link again. Obviously with a separate password.
21. ``passwd xy1234_oper``. 
     * Again use the generator, don't re-use.       
23. ``reboot``
28. ``wget https://software.virtualmin.com/gpl/scripts/install.sh``
29. ``sudo /bin/sh install.sh -f``
33. Open https://desld0001.sagikos.com:10000 in a local web browser and log in as ``sagikos_oper`` and the generated password.
1. Update the password restrictions per corporate policy at https://desld0001.sagikos.com:10000/acl/edit_pass.cgi?xnavigation=1 (Webmin > Webmin Users > Password Restrictions:

     1.  Minimum password length: 15.
     1.  Days before password must be changed: 90.
     1.  Days before un-changed password locks account: 7.
     1.  Disallow passwords containing username: Yes.
     1.  Disallow dictionary word passwords: Yes.
     1.  Number of old passwords to reject: 1 passwords.
1. Webmin > Webmin users > *Create a New Webmin group*.
     1. Group name: *opers*
     2. Available Webmin modules: *Select all*
1. Webmin > Webmin users > *Create a New Webmin group*.
     1. Group name: *employees*
1. Webmin > Webmin users > *Create a New Webmin group*.
     1. Group name: *contractors*
1. https://desld0001:10000/fail2ban/?xnavigation=1
     * Start at boot? Yes. Then click the button.
1. Virtualmin > System Settings > Account Plans > Default Plan > Limit on number of virtual servers: *Unlimited*. Save.
2. Virtualmin > System Settings > Features and Plugins > Check *SQLite Databases* and *Git repositories*. Save.
3. Virtualmin > System Settings > Server Templates > Default Settings > 
     4. Administration user > Chroot jail new domain Unix users: Yes. Save and next.
     5. BIND DNS domain > 
          1. Address records for new domains: uncheck m.${DOM}.
          2. Ensure *Additionally manually configured nameservers* is correct: *ns2.sagikos.com*.
          3. Create DNSSEC key and sign new domains: Yes. 
          4. Save and next.
     6. Mail for domain >
          1. Email message to send upon server creation: Message below ..
          2. Default quota for mail users: 300 MiB.
     7. Spam filtering >
          1. Automatically delete old spam: 30 days.
4. Virtualmin > System Settings > Virtualmin Configuration >
     1. SSL Settings >
          1. Redirect HTTP to HTTPS by default: Yes. Save.
5. Virtualmin > Email Settings > Mail Client Configuration: Enable mail client autoconfiguration: Yes. Save.
6. Virtualmin > Email Settings > New Mailbox Email > 
     1. Send email: Yes.
     2. Send email to: check both *User's mailbox* and *Virtual server owner*. Save.
7. Virtualmin > Email Settings > Updated Mailbox Email: Send email to: check both *User's mailbox* and *Virtual server owner*. Save.

## User Customization
For each user you want this:
1. ``sh -c "$(wget https://raw.github.com/ohmybash/oh-my-bash/master/tools/install.sh -O -)"``
1. ``pico ~/.bashrc``
     * Change ``OSH_THEME="font"`` to ``OSH_THEME="powerline"`` 
     * Add the bottom add:
          * ``alias sai="sudo apt install -y"``
          * ``alias ls='lsd'``
1. ``source ~/.bashrc``
24. On your local machine:
     * ``ssh-keygen``      
     * ``ssh-copy-id sagikos_oper@desld0001.sagikos.com``
     * ``ssh sagikos_oper@desld0001.sagikos.com``
30. ``curl https://sh.rustup.rs -sSf | sh``
31. ``source $HOME/.cargo/env``
32. ``cargo install lsd``

