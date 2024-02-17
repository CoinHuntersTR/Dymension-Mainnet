<h1 align="center"> Dymension Mainnet Validator
  
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

### Binaries Kurulumu

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


> Bu arada Dokümanın orjinali'ne [BURADAN](https://github.com/ruesandora/mangata-AVS/blob/main/README.md) ulaşabilirsiniz. Rues hazırladığı dokümanı kullandım ve yeni başlayanlar için ayrıntıları ekledim.

> Rues reposunda ödüllü olduğunu ve KYC yapılabileceğini belirtmiş. Ona göre kendi kararınızı verebilirsiniz.

