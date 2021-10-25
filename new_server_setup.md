# New Server Setup in Sagikos
Log in as root.
1. ``passwd``, use at least 15 characters strong generated password at https://passwordsgenerator.net/?length=16&symbols=1&numbers=1&lowercase=1&uppercase=1&similar=1&ambiguous=1&client=1&autoselect=1

## Set hostname
1. ``hostnamectl set-hostname desld0001``
1. ``pico /etc/hosts``, edit hostname for ipv4 and ipv6.

## Create the Sagikos group
1. ``groupadd sagikos``

## Add a user
1. ``useradd sagikos_oper -m -g sagikos -G sudo``
2. ``passwd sagikos_oper``. Use the above password generator link again. Obviously with a separate password.
3. ``reboot``
