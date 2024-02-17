<h1 align="center"> Dymension Node Taşıma
 <br/> <br>
 
![image](https://www.fotech.com.tr/fotech/dosyalar/kcfinder/images/image%2815%29.png)

## Taşıma Öncesi yapılacak adımlar.

> `priv_validator_key.json` dosyasını taşıyacağımız node içersinden yedekliyoruz. 

> Dosyanın bulunduğu yer `/root/.dymension/config/priv_validator_key.json` mobaxterm yada winscp gibi uygulamalar ile rahatlıkla indirebiliriz.

> Öncelikle yeni taşıyacağımız sunucu senkronize olmuş ve hazır konumda olacak.

> İlk olarak işimizin bittiği sunucuyu

```
sudo systemctl stop dymension.service 
```

> ile durduruyoruz.

> Sonra yeni sunucumuza, yedeklediğimiz `priv_validator_key.json` dosyasını. `/root/.dymension/config/priv_validator_key.json` buradaki yere import ediyoruz.

> Sonra node restart yapıyoruz.  

```
sudo systemctl restart dymension.service
```

> İşlem bittikten sonra,

> Aşağıdaki komutlar ile Blokların durumunu kontrol ediyoruz.

```
sudo journalctl -u dymension.service -f --no-hostname -o cat
``` 

> Burada sorun yok ise işlem başarılıdır! 
