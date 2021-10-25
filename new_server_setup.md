# New Server Setup in Sagikos
Log in as root.
1. ``passwd``

      use at least 15 characters strong generated password at https://passwordsgenerator.net/?length=16&symbols=1&numbers=1&lowercase=1&uppercase=1&similar=1&ambiguous=1&client=1&autoselect=1

1. ``hostnamectl set-hostname desld0001.sagikos.com``
1. ``pico /etc/hosts``

      edit hostname for ipv4 and ipv6.
      
1. ``pico /etc/default/useradd``

      ``SHELL=/bin/bash``

3. ``apt update && apt upgrade -y && apt install sudo git ufw -y``
5. ``sh -c "$(wget https://raw.github.com/ohmybash/oh-my-bash/master/tools/install.sh -O -)"``
6. ``pico ~/.bashrc``
7. Change ``OSH_THEME="font"`` to ``OSH_THEME="powerline"``
8. Add at the bottom: ``alias sai="sudo apt install -y"``
9. ``sudo ufw allow 20,21,22,25,53,80,110,143,587,953,993,995,443,6277,10000,20000/tcp``
10. ``sudo ufw allow 53,10000,20000/udp``
11. ``sudo ufw enable``
12. ``groupadd sagikos``
13. ``useradd sagikos_oper -m -g sagikos -G sudo``
14. ``passwd sagikos_oper``. 

      Use the above password generator link again. Obviously with a separate password.
1. ``reboot``
1. ``ssh-keygen``

      generate your local key to connect without password to the new server. use a blank password if you want.

1. ``ssh-copy-id sagikos_oper@desld0001.sagikos.com``
1. ``ssh sagikos_oper@desld0001.sagikos.com``
1. ``sh -c "$(wget https://raw.github.com/ohmybash/oh-my-bash/master/tools/install.sh -O -)"``
