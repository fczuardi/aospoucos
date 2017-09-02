## system update
```
sudo apt-get clean
sudo apt-get dist-upgrade
sudo apt-get update
```

## keyboard layout, timezone, hostname
```
sudo raspi-config
```

## wifi
```
sudo iwlist wlan0 scan|less
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
ifconfig wlan0
sudo wpa_cli reconfigure
ifconfig wlan0
```

se nao rolar, restart

```
sudo restart
```

## another sudo user
```
sudo adduser <username>
sudo visudo
```

    username     ALL=(ALL:ALL) NOPASSWD:ALL

## i3
```
sudo apt-get install xserver-xorg xinit i3
vim .config/i3/config 
```

    bindsym $mod+Return exec "stterm -e fish"
    bindsym $mod+Shift+e exec "i3-msg exit"

## stterm
```
sudo apt-get  install stterm
```

## fish
```
sudo apt-get install fish
fish_vi_key_bindings 
echo "set -x TERMINAL /usr/bin/stterm" >> ~/.config/fish/config.fish
```

## git
```
sudo apt-get install git
```

## xclip
```
sudo apt-get install xclip
```

## new ssh key
```
ssh-keygen -t rsa -b 4096 -C "fabricio@fabricio.org"
bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
xclip -sel ~/.ssh/id_rsa.pub
```

## vim, vim plug, mnmo nvim config
```
sudo apt-get install vim silversearcher-ag
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
mkdir mnmo; cd mnmo
git clone https://github.com/fczuardi/nvim
ln -s /home/fcz/mnmo/nvim/init.vim /home/fcz/.vimrc
```

## firefox
```
sudo apt-get install firefox-esr
```

## keepassx
```
sudo apt-get install kepassx
```

## raise swap
```
sudo vim /etc/dphys-swapfile
sudo /etc/init.d/dphys-swapfile stop
sudo /etc/init.d/dphys-swapfile start
free -m
```

## node js
```
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install -y build-essential
```
