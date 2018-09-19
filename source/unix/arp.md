## arp cache を消したい！

### Linux(CentOS7)
ip -s -s neigh flush all

### macos
sudo arp -d -a

### インタフェース指定して消す
sudo arp -d -i en0 -a
