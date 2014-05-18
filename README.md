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

The steps below are not yet fully working. I am attempting to build docker on a raspberry Pi to get the very latest release. The resin.io guys stopped at 0.8:
https://github.com/resin-io/lxc-docker-PKGBUILD/releases

If you don't need the very latest docker than download the 0.8 release and untar it to the root partition.

The steps below summarise what I have been trying to get a docker build to work from the latest source code. I have built docker, but it doesn't work :-(

5. Once downloaded the docker source I made some changes to the Dockerfile.

6. Need to edit docker/engine.go and change:
func checkKernelAndArch() error {
  // Check for unsupported architectures
- if runtime.GOARCH != "amd64" {
+ if runtime.GOARCH != "amd64" && runtime.GOARCH != "arm" {
  return fmt.Errorf("The docker runtime currently only supports amd64 (not %s). This will change in the future. Aborting.", runtime.GOARCH)
  
7. A number of files in the docker path have something like:
build lunux amd64

All of the amd64 strings need to be changed to arm

I used:
find . -name "*.go" -print -exec grep amd64 {} \; | less

Then:
perl -p -i -e "s/amd64/arm/g" *.go
