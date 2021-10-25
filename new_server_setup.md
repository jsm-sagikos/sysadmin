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
5. ``sh -c "$(wget https://raw.github.com/ohmybash/oh-my-bash/master/tools/install.sh -O -)"``
6. ``pico ~/.bashrc``
7. Change ``OSH_THEME="font"`` to ``OSH_THEME="powerline"`` 
9. Add the bottom add:
      * ``alias sai="sudo apt install -y"``
      * ``alias ls='lsd'``
12. ``sudo ufw allow 22/tcp``
13. ``sudo ufw allow 20,21,25,53,80,110,143,587,953,993,995,443,6277,10000,20000/tcp``
14. ``sudo ufw allow 53,10000,20000/udp``
15. ``sudo ufw enable``
16. ``groupadd sagikos``
17. ``useradd sagikos_oper -m -g sagikos -G sudo``
18. ``passwd sagikos_oper``. 
      * Use the above password generator link again. Obviously with a separate password.
19. ``reboot``
20. ``ssh-keygen``
      * generate your local key to connect without password to the new server. use a blank password if you want.
21. ``ssh-copy-id sagikos_oper@desld0001.sagikos.com``
22. ``ssh sagikos_oper@desld0001.sagikos.com``
23. ``sh -c "$(wget https://raw.github.com/ohmybash/oh-my-bash/master/tools/install.sh -O -)"``
24. ``wget https://software.virtualmin.com/gpl/scripts/install.sh``
25. ``sudo /bin/sh install.sh -f``
26. ``curl https://sh.rustup.rs -sSf | sh``
27. ``source $HOME/.cargo/env``
28. ``cargo install lsd``
29. Open https://desld0001.sagikos.com:10000 in a local web browser and log in as ``sagikos_oper`` and your generated password.
