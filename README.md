docker_for_rpi
==============

Docker for Raspberry Pi

Using this repository to store useful files to get the docker programme to run on a Raspberry Pi using the Raspbian debian Wheezy distribution.

I have uploaded the linux kernel which contains LXC and AUFS.

1.  linux/kerrnel.img - backup your old /boot/kernel.img on your RPI and copy this one into /boot
2. modules.tar - extract this is the directory /lib/modules
3. reboot

4. Now install lxc
sudo su
apt 
mkdir /opt/lxc
cd /opt/lxc
git clone https://github.com/lxc/lxc.git
apt-get install automake libcap-dev
cd lxc
./autogen.sh && ./configure && make && make install
Now to check that LXC is working correctly on the RPi type:

pi@raspberrypi /opt $ lxc-checkconfig
--- Namespaces ---
Namespaces: enabled
Utsname namespace: enabled
Ipc namespace: enabled
Pid namespace: enabled
User namespace: missing
Network namespace: enabled
Multiple /dev/pts instances: enabled

--- Control groups ---
Cgroup: enabled
Cgroup clone_children flag: enabled
Cgroup device: enabled
Cgroup sched: enabled
Cgroup cpu account: enabled
Cgroup memory controller: enabled

--- Misc ---
Veth pair device: enabled
Macvlan: enabled
Vlan: enabled
File capabilities: enabled

Note : Before booting a new kernel, you can check its configuration
usage : CONFIG=/path/to/config /usr/local/bin/lxc-checkconfig
This will show that the kernel is ready for docker to be installed from here. I installed docker by downloading the tar file from resin (https://github.com/resin-io/lxc-docker-PKGBUILD/releases) & extract as root from /

sudo su /
tar xvf docker*.tar.xz

5. Once downloaded the docker source I made some changes to the Dockerfile.
