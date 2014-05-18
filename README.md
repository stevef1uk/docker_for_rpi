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

5. Once downloaded the docker source I made some changes to the Dockerfile.

6. Need to edit docker/engine.go and change:

checkKernelAndArch() to have the line:
if runtime.GOARCH != "amd64" && runtime.GOARCH != "arm" {


