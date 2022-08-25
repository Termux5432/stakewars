# Setup Wallet dan Run Validator


Cek Spesifikasi (Supported/Not Supported)

```
bash
lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null \
&& echo "Supported" \
|| echo "Not supported"
```
    
![Screenshot_7](https://user-images.githubusercontent.com/35837931/180378418-393a50ae-11a1-405b-91df-4da90ec3abbf.png)

    
    jika `Not Supported`, maka kalian harus ganti spesifikasi VPS kalian yang lebih tinggi lagi
    
Install Developer Tools

```bash
    sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo
```

    
Install Python pip

```bash
sudo apt install python3-pip
```
    
Set Configuration
    
```bash
USER_BASE_BIN=$(python3 -m site --user-base)/bin
export PATH="$USER_BASE_BIN:$PATH"
```

Install Building Environment

```bash
sudo apt install clang build-essential make
```

Install Rust dan Cargo

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
    
![Screenshot_13](https://user-images.githubusercontent.com/35837931/180378912-a25707e0-a17e-4192-8435-0fb56773d50e.png)
    
    Pilih 1 dan enter

Source the Environment

```bash
source $HOME/.cargo/env
```

## Clone `nearcore` Project

Clone nearcore repository untuk menjalankan validatornya nanti.

Clone Repository

```bash
git clone https://github.com/near/nearcore
cd nearcore
git fetch
```
    
    
Checkout Commit

```bash
git checkout 0d7f272afabc00f4a076b1c89a70ffc62466efe9
```
[New commit] ( https://github.com/near/stakewars-iii/blob/main/commit.md )  

Compile `nearcore` Binary

Pastikan kalian posisinya masih didalam folder `nearcore`.
    
```bash
cargo build -p neard --release --features shardnet
```
       
Proses ini membutuhkan waktu yang cukup lama, jadi kalian perlu bersabar.
    
Inisialisasi Working Directory

Lakukan ini agar NEAR Node kalian bekerja dengan lancar dan tidak ada kendala.
    
Delete old `genesis.json`

```bash
rm ~/.near/genesis.json
 ```
        
Download new `genesis.json`
    
```bash
./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis
```
              
Menggunakan Snapshot `(Optional)`
        
Install `AWS-CLI`
        
```bash
sudo apt-get install awscli -y
```
        
Download Snapshot
        
```bash
cd ~/.near
# download
aws s3 --no-sign-request cp s3://build.openshards.io/stakewars/shardnet/data.tar.gz . 
# tar 
tar -xzvf data.tar.gz
# Hapus data
rm -rf data.tar.gz
```

    
Replace `config.json`
```bash
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
 ```
    
![Screenshot_19](https://user-images.githubusercontent.com/35837931/180382093-dfc50021-49e3-4174-825d-72535efe4fba.png)

    
Jalankan Node

    Pada bagian ini kalian harus menunggu download headers ke 100% dan kemudian baru melakukan sinkronisasi block. _Jangan `CTRL + C` agar tidak terjadi error saat menjalankan service nanti.
    
```bash
cd ~/nearcore
./target/release/neard --home ~/.near run
```
    
![Screenshot_20](https://user-images.githubusercontent.com/35837931/180382205-4ed8ca60-d216-4bcc-84bf-34514bc3dc87.png)

    
    Setelah download headers dan download block mencapai 100%,`CTRL + C`.
    
Membuat Service
    Karena saya menggunakan DigitalOcean, maka usernya otomatis menggunakan directory root
    
```bash
sudo vi /etc/systemd/system/neard.service
```
Lalu masukkan kode berikut ini :
       
```bash
        [Unit] 
        Description=NEARd Daemon Service 
        [Service] 
        Type=simple 
        User=$USER
        #Group=near 
        WorkingDirectory=$HOME/.near
        ExecStart=$HOME/nearcore/target/release/neard run 
        Restart=on-failure 
        RestartSec=30 
        KillSignal=SIGINT 
        TimeoutStopSec=45 
        KillMode=mixed 
        [Install] 
        WantedBy=multi-user.target
```
Kemudian tekan `ESC` dan ketik `:wq` `enter`.

Aktifkan Service

```bash
    sudo systemctl daemon-reload
    sudo systemctl enable neard
    sudo systemctl start neard
```
    
Jika kamu mengalami masalah pada `neard.service` kalian bisa lakukan perintah berikut :
    
```
    sudo systemctl stop neard
    sudo systemctl daemon-reload
    sudo systemctl start neard
```

Cek Log

```bash
    journalctl -n 100 -f -u neard
```
Jika kalian ingin log-nya terlihat berwarna, kalian bisa install `ccze` dengan command dibawah ini.
    
```bash
    sudo apt install ccze
```
    
Lalu pakai command dibawah ini untuk melihat log kalian lebih berwarna.
    
```bash
    journalctl -n 100 -f -u neard | ccze -A
```

## Menghubungkan Wallet ke NEAR-CLI dan Generate Validator Key

Pada bagian ini kalian harus memasukkan shardnet wallet kalian untuk menjalankan validatornya, kalian bisa ikuti caranya dibawah ini.
https://wallet.shardnet.near.org/

Lakukan autorisasi wallet

```bash
    near login
```
Copy Link untuk Autorisasi ke Browser kalian

![Screenshot_21](https://user-images.githubusercontent.com/35837931/180382353-112ce348-3dec-4797-9c90-f61c366049bb.png)


Klik Next dan Beri akses ke `NEAR-CLI` dengan klik `connect`
  
Lalu masukkan `Account ID` kalian dan klik confirm (contoh : termux.shardnet.near). `termux` bisa diganti dengan nama wallet kalian.

Setelah Memberi Akses, Kalian akan melihat gambar berikut.


![Screenshot_29](https://user-images.githubusercontent.com/35837931/180382929-a7947e62-a9c8-49a3-a764-c003512807ae.png)


Jangan khawatir, jika sudah muncul gambar tersebut, maka kalian sudah memberi akses wallet kalian ke `NEAR-CLI`.
    
Kembali ke VPS dan masukkan `Account ID` yang sudah kalian buat tadi dan tekan enter


Generate Key untuk `validator_key.json`
    
Ganti ?? dengan nama wallet kalian (Contoh : termux yang sudah saya buat tadi tanpa shardnet.near).
    
```bash
    near generate-key xx.factory.shardnet.near
```

Lalu pindahkan file `validator_key.json` ke folder `.near`
    
Ganti ?? dengan nama wallet kalian seperti cara sebelumnya.
    
```bash
    cp ~/.near-credentials/shardnet/xx.factory.shardnet.near.json ~/.near/validator_key.json
```
    
Kemudian ganti kata `private_key` ke `secret_key` dibagian `validator_key.json` file

```bash
    nano ~/.near/validator_key.json
```
    
Setelah di ubah, kemudian tekan `CTRL + O` dan `CTRL + X`
    
Simpan `node_key.json` dan `validator_key.json` file kamu ke PC kalian agar kedepannya jika VPS kalian mati masih ada backup file yang digunakan ke VPS baru supaya bisa menjalankan kembali validator kalian sebelumnya.
    
- Copy isi dari file `node_key.json` dan simpan kedalam notepad
    
```bash
        nano ~/.near/node_key.json
```
    
- Copy isi dari file `validator_key.json` dan simpan kedalam notepad

```bash
nano ~/.near/validator_key.json
```
        

# Lanjut ke Challenge 003

[Mounting Staking Pool](https://github.com/Termux5432/stakewars/blob/main/003-Mounting%20Staking%20Pool.md)


