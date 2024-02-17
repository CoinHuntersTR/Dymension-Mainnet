<h1 align="center"> Dymension Mainnet Validator
 <br/> <br>
 
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
### Seeds Ekliyoruz.

```
sed -i -e "s|^seeds *=.*|seeds = \"400f3d9e30b69e78a7fb891f60d76fa3c73f0ecc@dymension.rpc.kjnodes.com:14659\"|" $HOME/.dymension/config/config.toml
```
```
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"20000000000adym\"|" $HOME/.dymension/config/app.toml
```
```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.dymension/config/app.toml
```
```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:14658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:14657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:14660\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:14656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":14666\"%" $HOME/.dymension/config/config.toml
```
```
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:14617\"%; s%^address = \":8080\"%address = \":14680\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:14690\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:14691\"%; s%:8545%:14645%; s%:8546%:14646%; s%:6065%:14665%" $HOME/.dymension/config/app.toml
```

### Hızlı Senkronizasyon için SnapShot yüklüyoruz.

```
curl -L https://snapshots.kjnodes.com/dymension/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.dymension
[[ -f $HOME/.dymension/data/upgrade-info.json ]] && cp $HOME/.dymension/data/upgrade-info.json $HOME/.dymension/cosmovisor/genesis/upgrade-info.json
```

### Node çalıştırıyoruz.

```
sudo systemctl start dymension.service && sudo journalctl -u dymension.service -f --no-hostname -o cat
```

### Senkronizasyon kontrolü için

```
dymd status 2>&1 | jq .SyncInfo
```

> Nodemuz senkronize olduktan sonra `dymd status 2>&1 | jq .SyncInfo` sonucu false vermesi gerekiyor.

### Cüzdanımızı açalım.

> `wallet` ismi yerine istediğiniz ismi girebilirsiniz.

```
dymd keys add wallet
```
> Eğer eski cüzdanınızı tekrar yüklemek isterseniz. Yine `wallet` ismini istediğiniz gibi değiştirebilirsiniz.

```
dymd keys add wallet --recover

```


