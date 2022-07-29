
## Membuat Monitoring Node Status

Pada bagian ini, kalian akan membuat monitoring node status untuk melihat versi node dan lain sebagainya.

Install jq

```bash
    sudo apt install curl jq
```

Cek versi node kalian

```bash
    curl -s http://127.0.0.1:3030/status | jq .version
```
    
![Screenshot_33](https://user-images.githubusercontent.com/35837931/180384432-a17d6a2c-d8ff-4980-aa22-535c405aab9c.png)

    
- Cek Delegators dan Stake
Seperti biasa, ganti `??` dengan nama wallet kalian.

```bash
        near view ??.factory.shardnet.near get_accounts '{"from_index": 0, "limit": 10}' --accountId ??.shardnet.near
 ```
- Cek Block Produced
Ganti `??` dengan nama wallet kalian.
        
```bash
        curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq  '.result.current_validators[] | select(.account_id | contains ("??.factory.shardnet.near"))'
```
        
- Cek Reason Validator Kicked
        
Ganti `??` dengan nama wallet kalian.
        
```bash
        curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.prev_epoch_kickout[] | select(.account_id | contains ("??.factory.shardnet.near"))' | jq .reason
```

Cek Sinkronisasi

Pastikan status `syncing` adalah `false` agar validators kalian tidak tertinggal block. Jika masih `true` maka node kalian masih belum sepenuhnya sinkron.

```
    curl -s http://127.0.0.1:3030/status | jq .sync_info
```
