## install RT-kernel 5.4.115
- [source1]https://docs.ros.org/en/foxy/Tutorials/Building-Realtime-rt_preempt-kernel-for-ROS-2.html
- [source2]https://frankaemika.github.io/docs/installation_linux.html

### download source files
curl -SLO https://www.kernel.org/pub/linux/kernel/v5.x/linux-5.4.115.tar.xz
curl -SLO https://www.kernel.org/pub/linux/kernel/v5.x/linux-5.4.115.tar.sign
curl -SLO https://www.kernel.org/pub/linux/kernel/projects/rt/5.4/patch-5.4.115-rt57.patch.xz
curl -SLO https://www.kernel.org/pub/linux/kernel/projects/rt/5.4/patch-5.4.115-rt57.patch.sign
### decompress
xz -d linux-5.4.115.tar.xz
xz -d patch-5.4.115-rt57.patch.xz


### debugging
1. install dependencies
```shell
sudo apt-get build-dep linux
sudo apt-get install libncurses-dev flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf fakeroot
```

2. deb-pkg err
```shell
sudo gedit .config
```
comment out CONFIG_SYSTEM_TRUSTED_KEY CONFIG_MODULE_SIG_KEY

3. change default kernel setting
install grub-customizer
```shell
sudo add-apt-repository ppa:danielrichter2007/grub-customizer
sudo apt-get update
sudo apt-get install grub-customizer
```
