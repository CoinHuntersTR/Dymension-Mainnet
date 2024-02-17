<h1 align="center"> Snapshot
 <br/> <br>
 
![image](https://coinmuhendisi.com/blog/wp-content/uploads/2023/12/GAMMyjOXAAAkCbp.jpeg)


```
sudo systemctl stop dymension.service
```
```
cp $HOME/.dymension/data/priv_validator_state.json $HOME/.dymension/priv_validator_state.json.backup
```
```
rm -rf $HOME/.dymension/data
```

### SnapShot ve Yeniden Başlatma
```
curl -L https://snapshots.kjnodes.com/dymension/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.dymension
```

```
mv $HOME/.dymension/priv_validator_state.json.backup $HOME/.dymension/data/priv_validator_state.json
```

```
sudo systemctl start dymension.service && sudo journalctl -u dymension.service -f --no-hostname -o cat

```

