
# Membuat Ping Otomatis Setiap 2 Jam
Pada bagian ini kalian akan membuat transaksi otomatis untuk melakukan ping pada validators kalian.

Buat Folder Untuk Log

```bash
    mkdir $HOME/nearcore/logs
```
  
Buat file ping.sh
  
```
    nano $HOME/nearcore/scripts/ping.sh
```
  
Lalu isi filenya dengan kode dibawah ini

    ganti `??` dengan nama wallet kalian tanpa `.shardnet.near` atau `.factory.shardnet.near`.

```bash
    #!/bin/sh
    # Ping call to renew Proposal added to crontab

    export NEAR_ENV=shardnet
    export LOGS=$HOME/nearcore/logs
    export POOLID=??
    export ACCOUNTID=??

    echo "---" >> $LOGS/all.log
    date >> $LOGS/all.log
    near call $POOLID.factory.shardnet.near ping '{}' --accountId $ACCOUNTID.shardnet.near --gas=300000000000000 >> $LOGS/all.log
    near proposals | grep $POOLID >> $LOGS/all.log
    near validators current | grep $POOLID >> $LOGS/all.log
    near validators next | grep $POOLID >> $LOGS/all.log
```
Lalu buka crontab
    
```bash
```

Lalu pilih nomor 1 untuk mengedit file dengan `nano`


Masukkan kode dibawah ini

```bash
    0 */2 * * * sh $HOME/nearcore/scripts/ping.sh
```
    
Lalu tekan `CTRL + O` dan `CTRL + X`.
    
Cek apakah crontab kalian sudah berjalan atau tidak.

```bash
    crontab -l
```
    
![Screenshot_65](https://user-images.githubusercontent.com/35837931/181238951-4ead8ebf-50ef-4ac2-8bc0-154b7d8c2ecc.png)

    
Cek Log (kalian harus menunggu 2 jam agar bisa muncul)
  
```bash
    cat $HOME/nearcore/logs/all.log
```
    
`(Optional)` atau kalian bisa jalankan scriptnya dahulu dengan cara berikut ini.
    
```bash
    sh $HOME/nearcore/scripts/ping.sh
```
    
Lalu kalian cek lagi lognya.
