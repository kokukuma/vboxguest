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
    $ git clone https://github.com/kokukuma/vboxguest
    ```
    
  + ゲストOS内で、/var/lib/VBoxGuestAdditions/config を修正する.
    ```
    INSTALL_DIR='/home/vagrant/vboxguest'
    INSTALL_VER='5.0.4'
    ```
    
  + vboxadd setup を実行する.
    ```
    $ sudo /etc/init.d/vboxadd setup
    ```
    
+ VMを再起動
+ インストールできているかどうかの確認
  + 適当に大きめのファイル作ってcache効いてるか確認する.
    ```
    $ cd /vagrant/
    $ dd if=/dev/zero of=5mb bs=1000 count=5000
    $ ls -lh
    total 4.8M
    -rwxrwxrwx 1 vagrant vagrant 4.8M Oct 26 04:00 5mb
    -rwxrwxrwx 1 vagrant vagrant 3.4K Oct 26 02:08 Vagrantfile
    $ free -h 
                 total       used       free     shared    buffers     cached
    Mem:          494M        61M       432M       4.6M       2.1M        19M
    -/+ buffers/cache:        40M       454M
    Swap:         461M         0B       461M
    $ time cat 5mb >/dev/null
    
    real    0m0.047s
    user    0m0.000s
    sys     0m0.036s
    $ free -h
                 total       used       free     shared    buffers     cached
    Mem:          494M        66M       428M       4.6M       2.1M        24M <- cacheに乗ってる.
    -/+ buffers/cache:        40M       454M
    Swap:         461M         0B       461M
    $ time cat 5mb >/dev/null
    
    real    0m0.006s  <- cache効いてる. 
    user    0m0.004s
    sys     0m0.000s
    ```

## その他
+ linux kernel 3.16.7でしか確認してない.
