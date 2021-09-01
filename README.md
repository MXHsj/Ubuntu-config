## ubuntu config notes for stumpjumper

1.  view application 

``` shell
dpkg --get-selections | grep [app name]
```

2. update gazebo

``` shell
gazebo -v 
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
sudo apt-get update
sudo apt-get install gazebo[distro]
```

4. download neofetch
``` shell
sudo apt-add-repository ppa:dawidd0811/neofetch
sudo apt-get install neofetch
```

5. install nvidia driver
``` shell
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get install nvidia-driver-[distro]
```

6. install TLP cpu power manager
``` shell
sudo add-apt-repository ppa:linrunner/tlp
sudo apt-get update
```
tlp webpage:
https://linrunner.de/en/tlp/docs/tlp-linux-advanced-power-management.html

tlp settings:
https://github.com/drNoob13/batteryimprove/blob/5da50a4307c87e011bdc2484be5b5ada0b8b0f41/tlp#L76

7. setup zsh oh my zsh & add powerline font
``` shell
sudo apt-get install zsh
chsh -s $(which zsh)
sudo apt install curl wget git
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
install fonts
``` shell
sudo apt-get install fonts-powerline
```
install autoseggestion
``` shell
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

plugins=(zsh-autosuggestions)
```

8. ubuntu cleaner repository:
``` shell
sudo add-apt-repository ppa:gerardpuig/ppa
```

9. install Node.js
[sourcenode]https://github.com/nodesource/distributions


10. install sougou input method

download official package from https://pinyin.sogou.com/linux/?r=pinyin
``` shell
sudo dpkg -i ~/Downloads/sogoupinyin*.deb; sudo apt -f install
```

11. if install ros with root:
``` shell
chown -R xihan /home/xihan/catkin_ws
```

12. restore grub loader after win10 update
```
sudo add-apt-repository ppa:yannubuntu/boot-repair
sudo apt-get update
sudo apt-get install -y boot-repair && boot-repair
boot-repair
```
