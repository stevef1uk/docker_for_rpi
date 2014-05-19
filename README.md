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

The steps below are my notes on how to build docker on a raspberry Pi to get the very latest release. The resin.io guys stopped at 0.8:
https://github.com/resin-io/lxc-docker-PKGBUILD/releases

If you don't need the very latest docker than download the 0.8 release and untar it to the root partition.


WARNING: When I tried the new docker version I got errors when attempting to run existing images and I fixed these by deleting the contents of /var/lib/docker and pulling down resin/rpi_raspbian again. This may be a bug in that the images are not compatible between docker versions. I would push any images or export them if you want to keep them safe before installing the docker tar file if you have an existing set-up.

5. As root and from / tar xvf docker-0.11.1-armv6h.pkg.tar

6. I have provided an example start_docker script to set the LD_LIBRARY_PATH which docker needs.

The steps below summarise what I did to build docker from the latest source code. 

1. Once downloaded the docker source I made some changes to the Dockerfile, which I have included in the docker directory. 

2. Need to edit a number of docker go files to change amd64 to arm
I used:
find . -name "*.go" -print -exec grep amd64 {} \; | less

Then:
perl -p -i -e "s/amd64/arm/g" *.go

3. In the docker directory and with docker running type sudo make
