# Security and hardening of your VPS

## Prevent root from logging in via ssh for increased security.

One of the most common ways that people try to take over a Linux host, is by trying to login in as the root user. One simple way to increase the security of the Linux VPS is to simply remove this access, as we have another normal user to use instead (user mue in the example above).

Start by logging into the VPS, and edit the `sshd_config` file:

	sudo nano -w /etc/ssh/sshd_config

Scroll down and look for the row `PermitRootLogin yes`  and change to `no`
Additionally, we can specify who may connect by adding the line, where `mue` is the user account on the VPS:

	AllowUsers mue

then we may save the configuration by `ctrl o` and `ctrl x`

That's it! The next time you need to access your VPS, login with the user you specified. Your VPS is now secured from users trying to bruteforce the root remote login.
