Dropbear fork made specially for embedded devices
====================================================

This fork of dropbear solve a few problems with compiling and using dropbear server on embedded devices.
1) No more problems of login failure because of `chdir` error (In case no home directory was set in /etc/passwd)
2) No need to authenticate with anything. There is a new flag which defines what is the UID you want to login to (without the need of any password or ssh-key).
3) scp is now integrated to the dropbear server and you can use it even if the scp binary is not present on the device

Prerequisites
--------------------------------------
1) realpath (apt install realpath)
2) autoconf (apt install autoconf)
3) wget 	(apt install wget)

Notes
--------------------------------------
The repo is not a complete fork of dropbear but just a patch file and a build script to help you build the dropbear.

In order to make the build script function corectlly, you need to have your toolchain compiler in you PATH environment variable and use the `CROSS_COMP` environment variable to declare what cross compiler you want to use

The build script compiles the binary as a static binary, and `strip`s it at the end, the script also links the binary with zlib (Both the stripping and usage of zlib can be dismissed from the script manually).

Building
--------------------------------------
	git clone https://github.com/foxman69/dropbear-embedded.git
	cd dropbear-embedded
	chmod +x build.sh
	CROSS_COMP=mipsel ./build.sh
	file dropbearmulti

USAGE of the CROSS_COMP environment variable is not mandatory!

New features in the binary help flag
--------------------------------------
	./dropbearmulti dropbear -h
	Dropbear server v2019.78 https://matt.ucc.asn.au/dropbear/dropbear.html
	Usage: dropbear [options]
	-b bannerfile   Display the contents of bannerfile before user login
	                (default: none)
	-r keyfile  Specify hostkeys (repeatable)
	                defaults:
	                dss /etc/dropbear/dropbear_dss_host_key
	                rsa /etc/dropbear/dropbear_rsa_host_key
	                ecdsa /etc/dropbear/dropbear_ecdsa_host_key
	-R              Create hostkeys as required
	-F              Don't fork into background
	(Syslog support not compiled in, using stderr)
	-w              Disallow root logins
	-G              Restrict logins to members of specified group
	-s              Disable password logins
	-g              Disable password logins for root
	-B              Allow blank password logins
	-T              Maximum authentication tries (default 10)
	-j              Disable local port forwarding
	-k              Disable remote port forwarding
	-a              Allow connections to forwarded ports from any host
	-c command      Force executed command
	-p [address:]port
	                Listen on specified tcp port (and optionally address),
	                up to 10 can be specified
	                (default port is 22 if none specified)
	-P PidFile      Create pid file PidFile
	                (default /var/run/dropbear.pid)
	-i              Start for inetd
	-W <receive_window_buffer> (default 24576, larger may be faster, max 1MB)
	-K <keepalive>  (0 is never, default 0, in seconds)
	-I <idle_timeout>  (0 is never, default 0, in seconds)
	-U <uid/gid> UID And GID to override
	-V    Version

As you can see at the end a new -U flag is added...


Disclaimers
--------------------------------------
This repo patch only Dropbear version 2019.78
You still have to make sure /etc/dropbear folder exists (In order to create the host keys).
If you want you can edit the path in the `default_options.h` file, Lines 22-24.