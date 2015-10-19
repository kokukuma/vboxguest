## install
+ vboxguestを落とす.
```
git clone https://github.com/kokukuma/vboxguest
```
+ /var/lib/VBoxGuestAdditions/config を修正する.
```
INSTALL_DIR='/home/vagrant/vboxguest'
INSTALL_VER='5.0.4'
```
+ vboxadd setup を実行する.
```
sudo /etc/init.d/vboxadd setup
```
+ 再起動

## その他
+ linux kernel 3.16.7でしか確認してない.
