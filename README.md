<h1 align="center"> Dymension Mainnet Validator


 <br/> 
![image](https://cdn.airdropalert.com/images/metadata/dymension3333333.jpeg)

## Sistem gereksinimleri:
### Ubunutu 22.04
NODE TİPİ | CPU     | RAM      | SSD     |
| ------------- | ------------- | ------------- | -------- |
| Mangata  | 8          | 16        | 1 TB  |
  

## Kurulum

### Güncelleme

```
sudo apt -q update
```
```
sudo apt -qy install curl git jq lz4 build-essential
```
```
sudo apt -qy upgrade
```

### Go Kurulumu
```
sudo rm -rf /usr/local/go
curl -Ls https://go.dev/dl/go1.21.7.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
eval $(echo 'export PATH=$PATH:/usr/local/go/bin' | sudo tee /etc/profile.d/golang.sh)
eval $(echo 'export PATH=$PATH:$HOME/go/bin' | tee -a $HOME/.profile)
```

### Dymension İndirme ve Binaries Kurulumu
```
cd $HOME
```
```
rm -rf dymension
```
```
git clone https://github.com/dymensionxyz/dymension.git
```
```
cd dymension
```
```
git checkout v3.0.0
```

### Binaries Çalıştıralım.

```
make build
```

```
mkdir -p $HOME/.dymension/cosmovisor/genesis/bin
```
```
mv build/dymd $HOME/.dymension/cosmovisor/genesis/bin/
```
```
rm -rf build
```
```
sudo ln -s $HOME/.dymension/cosmovisor/genesis $HOME/.dymension/cosmovisor/current -f
```
```
sudo ln -s $HOME/.dymension/cosmovisor/current/bin/dymd /usr/local/bin/dymd -f
```
### Cosmovisor indiriyoruz ve Service oluşturuyoruz.

```
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.5.0
```

```
sudo tee /etc/systemd/system/dymension.service > /dev/null << EOF
[Unit]
Description=dymension node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.dymension"
Environment="DAEMON_NAME=dymd"
Environment="UNSAFE_SKIP_BACKUP=true"
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:$HOME/.dymension/cosmovisor/current/bin"

[Install]
WantedBy=multi-user.target
EOF
```
```
sudo systemctl daemon-reload
```
```
sudo systemctl enable dymension.service
```
### Node Çalıştıralım.
```
dymd config chain-id dymension_1100-1
```
```
dymd config keyring-backend file
```
```
dymd config node tcp://localhost:14657
```

> "" kalıyor. VALIDATOR İSMİNE istediğiniz bir ismi verebilirsiniz.
```
MONIKER="VALIDATOR İSMİ"
```
```
dymd init $MONIKER --chain-id dymension_1100-1
```
```
curl -Ls https://snapshots.kjnodes.com/dymension/genesis.json > $HOME/.dymension/config/genesis.json
```
```
curl -Ls https://snapshots.kjnodes.com/dymension/addrbook.json > $HOME/.dymension/config/addrbook.json
```
