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
3. ``apt update && apt upgrade -y && apt install sudo git ufw screen -y``
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
     2.  Minimum password length: 15.
     3.  Days before password must be changed: 90.
     4.  Days before un-changed password locks account: 7.
     5.  Disallow passwords containing username: Yes.
     6.  Disallow dictionary word passwords: Yes.
     7.  Number of old passwords to reject: 1 passwords.


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

