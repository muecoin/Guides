# Security and hardening of your VPS

## Prevent root from logging in via ssh for increased security.

One of the most common ways that people try to take over a Linux host, is by trying to login in as the root user. One simple way to increase the security of the Linux VPS is to simply remove this access, as we have another normal user to use instead (user mue in the example above).

Start by logging into the VPS, and edit the `sshd_config` file:

	sudo nano -w /etc/ssh/sshd_config

Scroll down and look for the row `PermitRootLogin yes`  and change to `no`
Additionally, we can specify who may connect by adding the line, where `mue` is the user account on the VPS:

	AllowUsers mue

then we may save the configuration by `ctrl o` and `ctrl x`

That's it! The next time you need to access your VPS, login with the user you specified. Your VPS is now secured from users trying to brute force the root remote login, they will just fill up the logs with unsuccessful login attempts.

---------------------------------------------------------

## Install Fail2ban for blocking ssh brute force attempts

Fail2ban is a tool that checks the `SSHD` logs, and searches for brute force attempts. If it finds an attack, where a user has more failed login attempts than a predefined threshold, the IP is banned in the firewall. Again, after a time limit, the IP is unbanned and the user may try again.

Running ssh on the default port 22 will give a lot of intrusion attempts from malicious boxes and script kiddies trying to guess the login of a host. Many times the attacker runs a script that goes through various common login names like adam, betty, carrie and david..... hoping for weak and bad passwords.

In order to keep the admin's sanity, Fail2ban will stop the attackers and keep the logs cleaner.

Please check the following guide for more information on how to install and setup Fail2ban on various Linux systems:
https://www.linode.com/docs/security/using-fail2ban-for-security


---------------------------------------------------------

## Install and configure a Firewall

For a Ubuntu host, you may use the UFW firewall, see the guide here:
[Setting up UFW on a Ubuntu server](https://github.com/muecoin/Guides/blob/master/ufw-firewall.md)

Alternatively, for any kind of VPS on Digital Ocean, it is now possible to put up a basic firewall from the Digital Ocean admin console. It allows all users to have a basic firewall, not specific to any distribution, but part of the Digital Ocean infrastructure itself:
[Setting up a simple Firewall on Digital Ocean](https://github.com/muecoin/Guides/blob/master/DO_general_firewall.md)
