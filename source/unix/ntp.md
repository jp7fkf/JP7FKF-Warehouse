# ntp

## init
  - port: udp 123

## Interactive mode
  - `ntpq`
  - ? を打つと使えるコマンドが出る．
  - peers でntpq -p と同様の情報が得られる．
  - 抜けるときは quit/exit.

## サクッと確認
  - ntpq -p

## ntpq peers の表は何を示しているのか
- `refid`: 参照しているサーバーの更に上位のNTPサーバを示す．
- `st`: stratum番号．サーバーが第何階層目かを示す．
- `when`: 前回サーバーを参照してから経過した秒数．pollに達すれば再度参照する．
- `poll`: 参照する間隔(秒数)．
- `reach`: 過去８回の参照結果．同期した場合1，同期しなかった場合は0．8進数で表示されており，毎回同期されると377(oct)となります．つまり11111111(bin)．
- `delay`: ポーリングインターバルの遅延見積もり（単位: ミリ秒）
- `offset`: NTPサーバとのずれ(ミリ秒)
- `jitter`: NTPサーバがどのくらい正確なのかを示す指標．値が小さいほど正確．

## 時刻の同期方式
- step方式: 時刻のずれを即座に修正
- Slew方式: 時刻の進み具合をずらしていくことで、少しずつ時刻を合わせていく方式
- ntpdはSlew方式．同期までしばらく時間がかかることがある．

- システムに問題がなければ、`ntpdate [server address]` で一気に同期することも可能．  
  ntpdateを使用する場合はntpdは停止している必要がある．

## chrony
- `systemctl start chronyd`
- `/etc/chrony.conf` にntpd的なconfigを書く．フォーマットはほぼ一緒っぽい．
- `rtcsync` をconfigファイルに書いておくとハードウェアクロックも同期してくれる．
- `server <ntp_server> [options]` で参照先サーバ指定．`iburst`オプションで起動直度同期を積極的に行う．
- `chronyc sources` で`ntp -q`的なやつが観れる．

## hwclock と同期したい(manual)
- `hwclock --systohc`を叩くと現在のsystem clock がhardware clock(RTC)に同期されるっぽい．

## mac でsntp
- `sudo sntp -sS <host>`
- ex.) `sudo sntp -sS ntp.nict.jp`

## ntpdateで強制同期
  - する．
  - '-d'オプションでdebug．
  ```
  -B: slewモードで強制的に時刻を修正．slewモードはadjtime()を使って徐々に時刻を修正．
  -b: stepモードで強制的に時刻を修正．stepモードはsettimeofday()を使いすぐに時刻を修正．
  -d: debug mode
  -q: NTPサーバーに問い合わせのみ行う．
  -s: 実行結果をsyslogに出力．
  -v: verbose
  ```

## ntpdateのdebug
  - Server dropped: no data
    - ntpサーバが起きてない，ntpパケットが通ってない．
  - Server dropped: strata too high
    - ntpサーバに到達しているが，サーバがまだ利用可能ではない．syncが終わっていない．stratum値が規定外の16になっている．
  - Server dropped: Leap not in sync
    - ntpサーバに到達しているが，当該サーバが信頼できない．due to 起動してからの時間が短い/時刻同期を実行している最中である．

## 代表的なntp
  - ntp.nict.jp
  - ntp.jst.mfeed.ad.jp

## hw clock
  - 表示
    - sudo hwclock
  - sync
    - sudo hwclock --systohc
    - sudo hwclock --hctosys