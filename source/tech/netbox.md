# netbox

## netbox installation
```
sudo apt update
sudo apt install python3
sudo apt install -y postgresql libpq-dev

sudo apt install -y python3 python3-pip python3-dev build-essential libxml2-dev libxslt1-dev libffi-dev libpq-dev libssl-dev redis-server zlib1g-dev
wget https://github.com/netbox-community/netbox/archive/vX.Y.Z.tar.gz
tar -xzvf v2.7.8.tar.gz -C /opt
cd /opt/
sudo ln -s netbox-2.7.8/ netbox
cd /opt/netbox/

pip3 install -r requirements.txt

```

## References
- [Installing NetBox - NetBox](https://netbox.readthedocs.io/en/stable/installation/)