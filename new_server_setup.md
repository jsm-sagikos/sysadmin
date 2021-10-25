# New Server Setup in Sagikos
1. Log in as root.
2. passwd, use at least 15 characters strong generated password at https://passwordsgenerator.net/?length=16&symbols=1&numbers=1&lowercase=1&uppercase=1&similar=1&ambiguous=1&client=1&autoselect=1

## Set hostname
1. hostnamectl set-hostname desld0001
1. pico /etc/hosts, edit hostname for ipv4 and ipv6.

## Create the Sagikos group
4. groupadd sagikos

## Add a user
5. useradd sagikos_oper -m -g sagikos -G sudo


6. reboot
