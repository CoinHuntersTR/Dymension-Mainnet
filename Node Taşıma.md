<h1 align="center"> Dymension Node Taşıma
 <br/> <br>
 
![image](https://www.fotech.com.tr/fotech/dosyalar/kcfinder/images/image%2815%29.png)

## Taşıma Öncesi yapılacak adımlar.

> `priv_validator_key.json` dosyasını taşıyacağımız node içersinden yedekliyoruz. 

> Dosyanın bulunduğu yer `/root/.dymension/config/priv_validator_key.json` mobaxterm yada winscp gibi uygulamalar ile rahatlıkla indirebiliriz.

> Öncelikle yeni taşıyacağımız sunucu senkronize olmuş ve hazır konumda olacak.
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
