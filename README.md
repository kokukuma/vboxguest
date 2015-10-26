## このレポジトリについて
+ vboxsfを速くするための実験的レポジトリ

## 含まれている修正
+ page cacheの利用
  + read/writeに, `generic_file_read_iter`、`generic_file_write_iter`を利用 
  + `sf_inode_revalidate`で, 古いキャッシュのクリアを実施.
  + `jiffies - dentry->d_time = 0`の場合, `sf_stat`を実施しない.
  + `sf_write_begin`, `sf_write_end`で, page cacheを利用する.
+ read aheadの利用
  + `ra_pages=32`に設定.
  + `sf_readpages`の追加.
+ バグ修正
  + mountオプション`ttl>0`設定時, permissionの反映が遅くなるバグの解消

## install
+ 既存のvboxguestがinstall済みのVMを立てる.
  + [vagrantのvboxguestインストールのガイド](https://docs.vagrantup.com/v2/virtualbox/boxes.html)
+ ゲストOS内でvboxguestを更新する.
  + このレポジトリを落とす.
    ```
    git clone https://github.com/kokukuma/vboxguest
    ```
    
  + ゲストOS内で、/var/lib/VBoxGuestAdditions/config を修正する.
    ```
    INSTALL_DIR='/home/vagrant/vboxguest'
    INSTALL_VER='5.0.4'
    ```
    
  + vboxadd setup を実行する.
    ```
    sudo /etc/init.d/vboxadd setup
    ```
    
+ VMを再起動

## その他
+ linux kernel 3.16.7でしか確認してない.
