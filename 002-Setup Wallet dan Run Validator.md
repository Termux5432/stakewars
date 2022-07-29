# Setup Wallet dan Run Validator

Cek Spesifikasi (Supported/Not Supported)
  ```bash
    lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null \
    && echo "Supported" \
    || echo "Not supported"
  ```
   ![Screenshot_7](https://user-images.githubusercontent.com/35837931/180378418-393a50ae-11a1-405b-91df-4da90ec3abbf.png)
   
Install Developer Tools
```bash
    sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo
    ```
 
    ![Screenshot_8](https://user-images.githubusercontent.com/35837931/180378519-74d78b04-96ee-465b-b0db-dd49bac9dc5a.png)
  
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
