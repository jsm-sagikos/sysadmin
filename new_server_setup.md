# New Server Setup in Sagikos
1. Log in as root.

## Set hostname
2. hostnamectl set-hostname desld0001
3. pico /etc/hosts, edit hostname for ipv4 and ipv6.

## Create the Sagikos group
4. groupadd sagikos

## Add a user
5. useradd sagikos_oper -m -g sagikos -G sudo


6. reboot
