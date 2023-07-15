# ubuntu config notes 20.04

## content
- monaco.ttf          ==> font for terminator & vscode
- terminator/config  ==> configuation file for terminator, copy to ~/.config/terminator/config
- strider.png         ==> desktop background
- vimrc ==> configuration file for vim, copy to ~/.vimrc
- zshrc ==> configuration file for zsh shell, copy to ~/.zshrc
- molokai.vim ==> theme for vim

## drivers
- auto install drivers
  ```
  sudo ubuntu-drivers autoinstall
  ```

- manual install nvidia driver
  ``` shell
  sudo add-apt-repository ppa:graphics-drivers/ppa
  sudo apt-get install nvidia-driver-[distro]
  ```

## shell
- **zsh** & **oh-my-zsh** & add 
  ``` shell
  sudo apt-get install zsh
  chsh -s $(which zsh)
  sudo apt install curl wget git
  sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
  ```

- **powerline font** 
  ``` shell
  sudo apt-get install fonts-powerline
  ```

- **autoseggestion** oh-my-zsh plugin
  ``` shell
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  ```
  in .zshrc
  ``` shell
  plugins=(zsh-autosuggestions)
  ```

## develop environment
- **Node.js**

  [sourcenode]https://github.com/nodesource/distributions

- **CUDA**
  
  intall CUDA from NVIDIA [webpage](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=20.04&target_type=deb_local)

  after installation, cuda is located under ```/usr/local/```
    ``` shell
    source ~/.zshrc
    nvcc -V
    ```

- **cuDNN**

  NVIDIA installation [guide](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html)
  
  download cuDNN Runtime library, developer library, code samples .deb
  ``` shell
  sudo dpkg -i ~/Downloads/*.deb
  ```

  cuDNN headers are located at ```/usr/include/cudnn*.h```

  trouble shooting: could not find cudnn version when building densepose:
  ``` shell
  ~/anaconda2/lib/python2.7/site-packages/torch/share/cmake/Caffe2/public/cuda.make 
  ```
  line 137: change ```cudnn.h``` to ```cudnn_version.h```


## utility software
- **neofetch**
  ``` shell
  sudo apt-add-repository ppa:dawidd0811/neofetch
  sudo apt-get install neofetch
  ```

- **TLP** cpu power manager
  ``` shell
  sudo add-apt-repository ppa:linrunner/tlp
  sudo apt-get update
  ```
  [tlp webpage](https://linrunner.de/en/tlp/docs/tlp-linux-advanced-power-management.html)
  [tlp settings](https://github.com/drNoob13/batteryimprove/blob/5da50a4307c87e011bdc2484be5b5ada0b8b0f41/tlp#L76)

- **ubuntu-cleaner**:
  ``` shell
  sudo add-apt-repository ppa:gerardpuig/ppa
  sudo apt install ubuntu-cleaner
  ```

- **grub-customizer**
  ```
  sudo apt install grub-customizer
  ```

- **sougou**
  
  download official [package](https://pinyin.sogou.com/linux/?r=pinyin) 
  ``` shell
  sudo dpkg -i ~/Downloads/sogoupinyin*.deb; sudo apt -f install
  ```


## trouble-shooting
- view application 
  ``` shell
  dpkg --get-selections | grep [app name]
  ```

- update gazebo
  ``` shell
  gazebo -v 
  sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
  wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
  sudo apt-get update
  sudo apt-get install gazebo[distro]
  ```

- if install ros with root:
  ``` shell
  chown -R xihan /home/xihan/catkin_ws
  ```

- restore grub loader after win10 update
  ```
  sudo add-apt-repository ppa:yannubuntu/boot-repair
  sudo apt-get update
  sudo apt-get install -y boot-repair && boot-repair
  boot-repair
  ```

- compile upstream linux kernel
  1. download kernel files from https://www.kernel.org/pub/linux/kernel, for example
    ``` shell
    curl -SLO https://www.kernel.org/pub/linux/kernel/v5.x/linux-5.19.1.tar.xz
    curl -SLO https://www.kernel.org/pub/linux/kernel/v5.x/linux-5.19.1.tar.sign
    ```
    decompress:
    ``` shell
    xz -d *.xz
    ```
    2. verify file integrity
    ``` shell
    gpg2 --verify linux-*.tar.sign
    gpg2 --verify patch-*.patch.sign
    ```
    in case of missing public key:
    ``` shell
    gpg2  --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys $(missed public key)
    ```
  3. customize kernel configuration (important step!!!)
    ``` shell
    tar xf linux-*.tar
    cd linux-*/
    patch -p1 < ../patch-*.patch
    cp -v /boot/config-$(uname -r) .config
    make olddefconfig
    make menuconfig
    ```
    Navigate to Cryptographic API > Certificates for signature checking > Provide system-wide ring of trusted keys > Additional X.509 keys for default system keyring
    Remove “debian/canonical-certs.pem” and “debian/canonical-revoked-certs.pem”
  4. compile and installation
    ```
    make -j$(nproc) deb-pkg
    sudo dpkg -i ../linux-headers-*.deb ../linux-image-*.deb
    ```

- "system program problem detected"
  ``` shell
  cd /var/crash
  ls
  sudo rm /var/crash/*
  ```

- matlab ./install freeze
  ``` shell
  xhost +SI:localuser:root
  ```

