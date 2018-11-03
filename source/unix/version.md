# version

## バージョンを調べる

### カーネルのバージョン
  - cat /proc/version 
  - uname -a

### OS version
  - cat /etc/debian_version (Debian)
  - cat /etc/redhat-release (CentOS)
  - cat /etc/fedora-release (Fedora)

### installed packages version
```
yum list installed | grep <package_name>
rpm -aq | grep <package_name>
```
