
![Cutie Shell logo](https://avatars.githubusercontent.com/u/88682014?s=200&v=4)

### [View Development Progress](/progress.md)

## Pre-installation rootfs gsi (halium-9.0+)
* Flash rootfs in twrp https://github.com/cutie-shell/rootfs-api28gsi-all/releases/                             
* supported: arm64,amd64 android devices                                        



## Automatic installation (hybris api28+)

* Install Droidian hybris phosh and devtools on your device.  
* Connected the device to a PC running Linux: `ssh droidian@10.15.19.82`  

```
wget -O - https://raw.githubusercontent.com/cutie-shell/cutie-shell/bullseye-hybris/install.sh | bash
```


## Automatic installation (wayland-egl pinephone)

* Install linux debian based on your device.  
* Connected the device to a PC running Linux

```
wget -O - https://raw.githubusercontent.com/cutie-shell/cutie-shell/bullseye-eglfs/install.sh | bash
```


### Manual building

* Install droidian phosh and devtools on your device.  
* Connected the device to a PC running Linux: `ssh droidian@10.15.19.82`    

```
sudo apt update -y
sudo apt upgrade -y
sudo apt install git qtdeclarative5-dev qdbus qtcreator qml qtbase5-gles-dev qt5-qpa-hwcomposer-plugin g++ make libudev-dev qml-module-qtquick2 qml-module-qtquick-controls qml-module-qtquick-controls2 qml-module-qtsensors qml-module-qtwayland-compositor qml-module-qtquick-virtualkeyboard polkit-kde-agent-1 libqt5dbus5 libqt5waylandclient5 libqt5waylandclient5-dev qtwayland5 qtvirtualkeyboard-plugin qml-module-qt-labs-folderlistmodel -y
cd ~

sudo git clone https://github.com/cutie-shell/atmospheres.git /usr/share/atmospheres
git clone https://github.com/cutie-shell/cutie-shell
git clone https://github.com/cutie-shell/cutie-settings-daemon.git
git clone https://github.com/cutie-shell/qml-module-cutie.git

cd cutie-settings-daemon
qmake
make -j$(nproc)
sudo make install

cd ../qml-module-cutie
qmake
make -j$(nproc)
sudo make install

cd ../cutie-shell
sudo mkdir -p /etc/systemd/logind.conf.d/
sudo cp -R logind.conf.d/10-cutie.conf /etc/systemd/logind.conf.d/10-cutie.conf
cp gtk.css ~/.config/gtk-3.0/gtk.css
qmake
make -j$(nproc)
sudo make install
sudo cp cutie-ui-io.service /usr/lib/systemd/system/cutie-ui-io.service
sudo systemctl daemon-reload
sudo systemctl disable phosh
sudo systemctl enable cutie-settings-daemon
sudo systemctl enable cutie-ui-io
sudo reboot
```

### Troubleshooting

### To fix isues on scaling
* Connected the device to a PC running Linux: `ssh droidian@10.15.19.82` 

```
sudo nano /usr/lib/systemd/system/cutie-ui-io.service

There is this line in the service file: Environment=QT_SCALE_FACTOR= "4". By default, the scale is 4 like vollaPhone display 1080x2340, but if you have it too large, you can make it less (for example 2 like Redmi 7 display 720x1520).

ctrl x
y
enter
```
### Connecting to wifi on cutie-rootfs-minimal
* Connected the device to a PC running Linux: `ssh droidian@10.15.19.82` 
```
nmcli device wifi connect "YOUR_WIFI_SSID" password YOUR_WIFI_PASSWD name "YOUR_WIFI_SSID"
```

