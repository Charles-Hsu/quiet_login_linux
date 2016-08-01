# quiet_login_linux

Linux Login Step

1. /etc/issue show before the login prompt
2. show <host_name> login:
3. /etc/motd show after login

$ps -ef | grep tty
root 1470 1414 0 15:16 tty7 00:00:19 /usr/lib/xorg/Xorg -core :0 -seat seat0 -auth /var/run/lightdm/root/:0 -nolisten tcp vt7 -novtswitch
root 1471 1 0 15:16 tty1 00:00:00 /sbin/agetty --noclear tty1 linux
root 2706 2588 0 15:41 pts/18 00:00:00 sudo vim serial-getty@ttyS0.service
root 2707 2706 0 15:41 pts/18 00:00:00 vim serial-getty@ttyS0.service
root 3708 2649 0 16:10 pts/17 00:00:00 man agetty
root 3797 1 0 16:11 ttyS0 00:00:00 /sbin/agetty --nohostname --noissue -L ttyS0 115200 vt100

These settings are different from the configuration method descripted in Disable logon prompt show hostname http://askubuntu.com/questions/434950/disable-logon-prompt-show-hostname. 

It must be defined in somewhere else.

After googling, I found a page written in Japanness, http://www.mztn.org/kvm/kvm2.html. Is is defined in /lib/systemd/system/serial-getty@.service, follow the guide as bellow step

$ sudo cp /lib/systemd/system/serial-getty@.service /etc/systemd/system/serial-getty@ttyS0.service
$ sudo vim /etc/systemd/system/serial-getty@ttyS0.service
[Service]
#ExecStart=-/sbin/agetty --keep-baud 115200,38400,9600 %I $TERM
ExecStart=-/sbin/agetty --nohostname --noissue -L ttyS0 115200 vt100

$ sudo systemctl daemon-reload
$ sudo systemctl stop serial-getty\@ttyS0.service 
$ sudo systemctl start serial-getty\@ttyS0.service

done!
