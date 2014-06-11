docker_for_rpi
==============

Docker for Raspberry Pi

Using this repository to store useful files to get the docker programme to run on a Raspberry Pi using the Raspbian debian Wheezy distribution.

DISCLAIMER:  warranties, whether express or implied, including, without limitation, any implied warranties or fitness for a particular purpose are not provided. Use of anything in this repository is entirely at your own risk.

I have uploaded the linux kernel which contains LXC and AUFS.

1.  linux/kerrnel.img - backup your old /boot/kernel.img on your RPI and copy this one into /boot
2. modules.tar - extract this is the directory /lib/modules
3. reboot

4. Now install lxc
<pre>
$sudo su
$mkdir /opt/lxc
$cd /opt/lxc
$git clone https://github.com/lxc/lxc.git
$apt-get install automake libcap-dev
$cd lxc
$./autogen.sh && ./configure && make && make install
</pre>
Now to check that LXC is working correctly on the RPi type:

$lxc-checkconfig

The steps below are my notes on how to build docker on a raspberry Pi to get the very latest release. The resin.io guys stopped at 0.8:
https://github.com/resin-io/lxc-docker-PKGBUILD/releases

If you don't need the very latest docker than download the 0.8 release and untar it to the root partition.


5. As root and from / tar xvf docker-1.0.0-armv6h.pkg.tar

6. I have provided an example start_docker script to set the LD_LIBRARY_PATH which docker needs.

I encountered the following error when trying to run an image:
Error: Cannot start container fe60acdc3c0ef5ab84f5ce506c1358ff37af69a2a60444a8cf4231165b6cbd2a: mkdir /sys/fs/devices: no such file or directory

The following link helped me fix this:
https://github.com/ubergarm/debian-docker-runit


Stop your old docker daeamon

Remove cgroup mount point from /etc/fstab if you have it e.g.  cgroup /sys/fs/cgroup cgroup defaults 0 0

$sudo umount /sys/fs/cgroup

You may need to apt-get remove systemd
You may need to reboot.

Install runit and cgroupfs-mount script

This will start the docker daemon up and running and it will come back after reboots.
<pre>
$sudo -i
$apt-get install runit
$cd /usr/local/bin 
$wget https://raw.github.com/tianon/cgroupfs-mount/master/cgroupfs-mount
$chmod a+x cgroupfs-mount
$cd /etc/sv
$git clone https://github.com/ubergarm/debian-docker-runit
$ln -s /etc/sv/debian-docker-runit/docker /etc/service/docker

More commands

$man sv
$sv status docker
$sv stop docker
$sv start docker
</pre>

I noticed that the CPU was running a little high at idle so I added the line:
<pre>
cgroup_disable=memory
</pre>
to the file /boot/config.txt to resolve this.

The steps below summarise what I did to build docker from the latest source code. 

1. Once downloaded the docker source I made some changes to the Dockerfile, which I have included in the docker directory. 

2. Need to edit a number of docker go files to change amd64 to arm
I used:
find . -name "*.go" -print -exec grep amd64 {} \; | less

Then:
perl -p -i -e "s/amd64/arm/g" *.go

3. In the docker directory and with docker running type sudo make
