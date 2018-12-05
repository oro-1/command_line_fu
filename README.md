# Command Line Fu
These are just notes for command line stuff I have learned over the years: shortcuts and so on.  They are in no order, esxcept the most recent discoveries are often on top.

#### Netstat 

    netstat -peanut
    
    ubuntu@vpn-strongswan:~$ netstat -peanut
    (Not all processes could be identified, non-owned process info
    will not be shown, you would have to be root to see it all.)
    Active Internet connections (servers and established)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       User       Inode       PID/Program name
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      0          15747       -               
    tcp        0      0 10.42.5.36:22           10.3.7.69:3151          ESTABLISHED 0          20138       -               
    tcp        0      0 10.42.5.36:22           10.3.7.69:36800         ESTABLISHED 0          24746       -               
    tcp        0    376 10.42.5.36:22           10.3.7.69:52220         ESTABLISHED 0          20293       -               
    tcp        0      0 10.42.5.36:22           10.3.7.69:62004         ESTABLISHED 0          23877       -               
    tcp        0      0 10.42.5.36:22           10.3.7.69:41994         ESTABLISHED 0          24438       -               
    tcp6       0      0 :::22                   :::*                    LISTEN      0          15760       -               
    udp        0      0 0.0.0.0:68              0.0.0.0:*                           0          14371       -               


#### Start a process on a different tty:
This was handy with the Raspberry pi when I ssh'd in and had to display an output on the attached screen

    setsid sh -c 'exec command <> /dev/tty2 >&0 2>&1'

#### Windows instead of snipping tool shortcut

    Win + shift + s
 
#### Show what inputs are attached and to disable touchpad on Levono laptops with Ubuntu

    xinput list

You will get output like:

    Virtual core pointer                      id=2    [master pointer  (3)]
    Virtual core XTEST pointer                id=4    [slave  pointer  (2)]
    SynPS/2 Synaptics TouchPad                id=12   [slave  pointer  (2)]
    Virtual core keyboard                     id=3    [master keyboard (2)]
    Virtual core XTEST keyboard               id=5    [slave  keyboard (3)]
    Power Button                              id=6    [slave  keyboard (3)]
    Video Bus                                 id=7    [slave  keyboard (3)]
    Power Button                              id=8    [slave  keyboard (3)]
    Sleep Button                              id=9    [slave  keyboard (3)]
    Laptop_Integrated_Webcam_1.3M             id=10   [slave  keyboard (3)]
    AT Translated Set 2 keyboard              id=11   [slave  keyboard (3)]
    Dell WMI hotkeys                          id=13   [slave  keyboard (3)]

Then type:

    xinput set-prop 12 "Device Enabled" 0

#### Un-ban someone in a running "fail2ban" setup

     fail2ban-client set ssh unbanip 123.123.123.123

#### Ansible notes:
Useful for things that need separate lines:

    extra_lines: |      
      first line
      second line
      third line
      
Will display:

    1. first line
    2. second line
    3. third line

Useful for things that need to be indented but in reality is all one line

    extra_lines: >
      first line
      second line
      third line

Will display:

    first line second line third line

Other stuff:

    --forks  # The default is 5.

    ansible-playbook -u ubuntu -i inventory/dev/ec2.py --vault-password-file=~/.ssh/ansible_vault_pass.txt dev-deploy.yml --private-key=~/.ssh/dev-key.pem --limit 192.168.5.4 --list-hosts

#### That software that has variables for docker in key/value pairs
- consul.io
- vaultproject.io

#### Unix permissions: dealing with my dyslexia 
The order is L->R: is User/Owner, Group, Everyone/Other

    -rwxr-xr--  1 amrood   users 1024  Nov 2 00:10  myfile

- User has read, write, execute permissions
- Group "users" has read, execute permissions
- Everyone else has read-only permissions

chmod 0 = no permissions

#### Access control lists:
acl has to be enabled on disk first:

    /dev/sdc1   /encrypt    ext3    defaults,acl    0 0
    
Remount the disk if needed:

    mount -a -o remount    
    
set and read the acl:

    setfacl -Rm "g:dev-interface:rw" /encrypt/sftp
    getfacl /encrypt/sftp

#### VirtualBox management on command line:

Get IP address of running guest:

    VBoxManage guestproperty get {UUID no brackets} "/VirtualBox/GuestInfo/Net/0/V4/IP"
    
Get list of vms/running vms:

    VBoxManage list vms/runningvms
    
#### To change the default editor in Ubuntu

    sudo update-alternatives --config editor

#### That key you like (shorter than the rest, more secure for now):

    ssh-keygen -t ed25519

#### Command line shortcuts 

    CTRL+U : cut everything before pointer
    CTRL+K : cut everything after pointer
    CTRL+A : Go the beginning of line
    CTRL+E: go to end of line
    CTRL+left/right arrow : Go to beginning of word

    CTRL+W : delete word before pointer
    CTRL+Y : paste erased
    CTRL+D : exit
    CTRL+C : SIGTERM
    CTRL+R : Search history
    CTRL+B : move back one character
    CTRL+F : move forward one character
    CTRL+I : insert a tab
    CTRL+insert : copy
    SHIFT+insert: paste
    ALT+B : move back one word
    ALT+F : move forward one word

#### Gives you vi functionality on the command line.

    set -o vi

#### memory info

    vmstat
    free -h
    perf (command)  # May need linux-tools installed
    dstat -ta       # Live stats on cpu/disk/network

#### strace will tell you the path of every file the program tries to open.
Say you've got a utility, and you think it's trying to read a config file, but you don't know what path it's looking for. 

    strace -o [output file]

#### List open interface for port 22
    lsof -i :22

#### Find all logs which have changed within 5 minutes

    find /var/log -mmin -5 

#### Sometimes instead of netcat you can use socat. Sure it isn't as simple but has a lot more features.

#### Packages you like (note, some may be named different in deb vs. rpm packages):
ncdu screen telnet vim-enhanced multitail lynx bind-utils curl wget openssh-clients lsof net-snmp rsync mlocate man zip unzip htop at yum-utils tree tmux policycoreutils-python boxes figlet sysstat tcpdump 

#### To check an ssh port on SELinux (you need the policycoreutils-python package):
	
    semanage port -l | grep ssh

#### To add an ssh port on SELinux:
	
    semanage port -a -t ssh_port_t -p tcp 222

#### Mailing when people login
In /etc/pam.d/sshd

	session    optional     pam_exec.so /usr/local/bin/send-mail-on-ssh-login.sh

#### show context in grep 2 lines before AND after

    grep -C2 "foo" ./milefile.txt
    
#### Output to clipboard

    command | xclip
    
#### For logs that get rotated, you may want tail -F or tail --follow=name so you start following the new log rather than the old file that got moved out.

    tail -F rotating.log

#### You can also tail -f multiple files simultaneously!  Package multitail also works (with color, too):
    
    tail -f /var/log/*.log is very useful!

#### To terminate an ssh session that's hung or something, type this sequence 

	[Enter] [~] [.]

More of these escape sequences can be listed with [Enter] [~] [?]

	Supported escape sequences:
	  ~.  - terminate session
	  ~B  - send a BREAK to the remote system
	  ~R  - Request rekey (SSH protocol 2 only)
	  ~#  - list forwarded connections
	  ~?  - this message
	  ~~  - send the escape character by typing it twice
	(Note that escapes are only recognized immediately after newline.)

#### Put in your .bashrc or /etc/bash.bashrc for history control

	export HISTCONTROL=ignoredups:erasedups  # no duplicate entries
	export HISTSIZE=100000                   # big big history
	export HISTFILESIZE=100000               # big big history
	shopt -s histappend                      # append to history, don't overwrite it

	# Save and reload the history after each command finishes
	export PROMPT_COMMAND="history -a; history -c; history -r; $PROMPT_COMMAND"


#### awk using multiple field separators, use pipe:
	
    awk -F "=|," 

#### Save a file you edited in vim without the needed permissions

	:w !sudo tee %

#### edit a command in the editor

	ctrl-x-e
	export EDITOR=/usr/bin/vim (to change from nano, put in .bashrc to make permanent)

#### Runs previous command replacing foo by bar every time that foo appears

	!!:gs/foo/bar
	(As opposed to ^foo^bar, which only replaces the first occurrence of foo, this one changes every occurrence.)

#### Put a console clock in top right corner	

	while true; do echo -ne "\e[s\e[0;$((COLUMNS-27))H$(date)\e[u"; sleep 1; done &
	
#### Glob expanding

	[esc] *
	
#### Terminal emulation

	UTF-8 to get proper lines and ncurses and remove â and so on
	
#### Replace a whole line on one match (note "dot star" at beginning and end)

	sed -e "s/.*Alex.*/###########/" deleteme.txt

#### How to I write to dev null again? Fucking hate crontab reports.

	[command] > /dev/null 2>&1
	
#### Check if something is in a bash array:

	myarray=(one two three)
	case "${myarray[@]}" in  *"two"*) echo "found" ;; esac

#### Do some stuff with the console (helpful with virsh console [guest]) in Linux KVM

	stty cols 80
	stty rows 50
	TERM=vt100; export TERM

#### Change the text and other Screensaver properties in KDE:
You need to have an ~/.xscreensaver file. There needs to be at least this line: 

		textMode:       program
		textLiteral:    XScreenSaver
		textFile:       
		textProgram:    fortune
		textURL:        http://planet.debian.org/rss20.xml
	
#### yum groups

		yum grouplist 								# Will show you list.  
		yum groupinstall "X Desktop Server" 		# Then you have to enclose it in quotes

#### List a lot of cronbtabs at once:

	ls /etc/cron* + cat
	for user in $(cat /etc/passwd | cut -f1 -d:); do crontab -l -u $user; done
	
#### ASCII "Video" you keep forgetting:

	http://aa-project.sourceforge.net/
	
#### snmpd.conf 

	'dontLogTCPWrappersConnects'      => 'true # To keep "snmpd[3458]: Connection from UDP: [127.0.0.1]:48911" from filling your logs'
	
#### Install XFCE on CentOS 6

	yum groupinstall Xfce
	yum install xorg-x11-fonts-Type1 xorg-x11-fonts-misc
	/sbin/telinit 5
	or startxfce4

#### set in bash

    -e will make the script stop at the first error (instead of coninuting along cheerfully)
    -u will not allow unset/undefined variables (like perl strict)
    -x debug mode 

#### String maniuplation in bash
len() in bash

	$ var='Hello, World!'
	$ echo "${#var}"
	13
	
left() in bash

    $ var='Hello, World!'
	#${string:position:length}
	$ echo "${var:0:5}"
	Hello
	
right() in bash

	$ var='Hello, World!'
	#${string:position:length}
	$ echo "${var:7:${#var}}"
	World!
	#or a litte more dynamic.. (the 6 most right chars)
	echo "${var:$((${#var}-6)):${#var}}"
	World!
	
mid() in bash

	$ var='Hello, World!'
	#${string:position:length}
	$ echo "${var:4:4}"
	o, W

string replace first in bash (substitute)

	$ var='Hello, World!'
	#${string/substring/replacement}
	$ echo "${var/o/a}"
	Hella, World!

string replace all in bash (substitute)

	$ var='Hello, World!'
	#${string//substring/replacement}
	$ echo "${var//o/a}"
	Hella, Warld!

string to lower case

        $ string="A FEW WORDS"
        $ echo "${string,}"
        a FEW WORDS
        $ echo "${string,,}"
        a few words
        $ echo "${string,,[AEIUO]}"
        a FeW WoRDS
    
string to upper case (all caps)

        $ string="a few words"
        $ echo "${string^}"
        A few words
        $ echo "${string^^}"
        A FEW WORDS
        $ echo "${string^^[aeiou]}"
        A fEw wOrds


#### Human copy with progress bar	

	rsync -WavP --human-readable --progress

#### This is a quick step to generate self-signed certificate :

	openssl genrsa 2048 > host.key
	openssl req -new -x509 -nodes -sha256 -days 3650 -key host.key > host.cert
	#[enter *.domain.com for the Common Name]
	openssl x509 -noout -fingerprint -text < host.cert > host.info
	cat host.cert host.key > host.pem
	chmod 400 host.key host.pem

#### I have a keypair. How do I determine the key length?

	openssl rsa -in private.key -text -noout

The top line of the output will display the key size.

    Private-Key: (2048 bit)
	
To view the key size from a certificate:

	openssl x509 -in public.pem -text -noout | grep "RSA Public Key"
	
	RSA Public Key: (2048 bit)	

#### How do I compare a key to a cert?

	openssl x509 -noout -modulus -in /etc/ssl/certs/example.crt | openssl md5;\
    openssl rsa -noout -modulus -in /etc/ssl/private/example.key | openssl md5


#### Print out separate colors for each output	

	for foo in $(seq 0 2); do 
		color="[3"
		color+=$(($foo + 1))
		color+="m"
		echo -e "\e$color-------\nprod-sessions-slave$foo\n"
		ssh root@prod-sessions-slave$foo.shrm.org 'df -h'
	done
	echo -e "\e[0m"
	
#### Expand via curly braces:

	for foo in server-cache-{00..12..2}; do
		echo $foo
	done

#### My rsync reminder (because I always get confused)

  rsync -auvz source/ dest/
  rsync -auvz source/ dest
  rsync -auvz source/* dest

will take all the files and subdirectories from source/ and put it into dest/	

#### Formatting time display

    function displaytime {
      local T=$1
      local D=$((T/60/60/24))
      local H=$((T/60/60%24))
      local M=$((T/60%60))
      local S=$((T%60))
      (( $D > 0 )) && printf '%d days ' $D
      (( $H > 0 )) && printf '%d hours ' $H
      (( $M > 0 )) && printf '%d minutes ' $M
      (( $D > 0 || $H > 0 || $M > 0 )) && printf 'and '
      printf '%d seconds\n' $S
    }

#### BASH Auto complete
If you happen to have the servernames listed in a textfile, or if you need to prefix your ssh connections with a username, you could create that server list file like this (with or without the username prefix, depending on how you wish to connect):

    glarson@ndi-web-dev
    glarson@ndi-portal-dev
    glarson@cache-serv001.fishbowl.punkadyne.net 

and so on.  This way, the tab completion will include your username prefix. And to utilize this particular file, you would enter something like this (of course, pointing to your particular server list, not mine):

    complete -W "`cat /home/glarson/serverlist.txt`" ssh

and again, be careful with the quotes and back-ticks.

#### READ LINES WITH SPACES AND SHIT
    
    #!/bin/bash
    file="/home/glarson/list-o-servers.txt"
    while IFS= read -r line
    do
            # display $line or do somthing with $line
            printf '%s\n' "$line"
    done <"$file"

#### You can also read field wise:

    #!/bin/bash
    file="/etc/passwd"
    while IFS=: read -r f1 f2 f3 f4 f5 f6 f7
    do
            # display fields using f1, f2,..,f7
            printf 'Username: %s, Shell: %s, Home Dir: %s\n' "$f1" "$f7" "$f6"
    done <"$file"
    
#### To lock out a user account.  Several ways:

    1. passwd -l $USERACCT                  # -u is unlock, -e expire and set user to change on login
    2. chage -E 0 $USERACCT                 # Setting it to -1 will enable no expiration, otherwise, YYYY-MM-DD
    3. usermod -e 0 $USERACCT               # YYYY-MM-DD format otherwise
    4. usermod -s /sbin/nologin $USERACCT   # Not recommended but can be useful for other purposes

#### To check an ssh port on SELinux (you need the policycoreutils-python package):

	semanage port -l | grep ssh

#### To add an ssh port on SELinux:

	semanage port -a -t ssh_port_t -p tcp 222
	

#### Make a 4gb swap file

    sudo dd if=/dev/zero of=/swapfile bs=1024 count=4194304
    sudo mkswap /swapfile
    sudo chmod 600 /swapfile
    sudo swapon /swapfile
    sudo echo "/swapfile none  swap  sw 0  0" >> /etc/fstab
    sudo echo "vm.swappiness=50" >> /etc/sysctl.conf
    
    
