NEAR-CLI adalah antarmuka baris perintah yang berkomunikasi dengan blockchain NEAR melalui panggilan prosedur jarak jauh (RPC)

Pertama, mari kita pastikan mesin linux up-to-date.

```
sudo apt update && sudo apt upgrade -y
```

##### Install developer tools, Node.js, and npm

```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
````
````
sudo apt install build-essential nodejs
````
````
PATH="$PATH"
````

Check Versi `Node.js` and `npm` version:
```
node -v
```
> v18.x.x
```
npm -v
```
> 8.x.x

##### Install NEAR-CLI
Inilah Repositori Github untuk NEAR CLI.: https://github.com/near/near-cli. Untuk menginstal NEAR-CLI, ( kecuali Anda login sebagai root, tidak disarankan menggunakan `sudo` untuk menginstal NEAR-CLI sehingga biner dekat disimpan ke /usr/local/bin

```
sudo npm install -g near-cli
```

###### Environment
Network perlu diatur setiap kali shell baru diluncurkan untuk memilih jaringan yang benar.

```
export NEAR_ENV=shardnet
```

```
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
```
