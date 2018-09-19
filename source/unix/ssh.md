## 鍵ペアをつくる
- RSA でビット長4096なやつ
```
ssh-keygen -t rsa -b 4096
```


## どんな鍵だったっけ
```
ssh-keygen -l -v -f ~/.ssh/id_rsa.pub
```
  - -v つけるとフィンガープリントが出る．