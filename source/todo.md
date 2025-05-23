# Todo
- つくりたいものとか，やりたいこととか．
- やると決めたことくらいちゃんとやれ
  - 壁はあるだろうが，あきらめずにやれ．やるときめたんだったら．

## Prio list
- CCNP
- AWS
- English

## やってみる
- 実装がなかったら実装する
- EVPN(PBB, MPLS, VXLAN)
- TI-LFA/FRR
- GRE BGP/OSPF
- Bird/GoBGP/Exabgp/FRR
- VPLS/VPWS w MPLS
- MPLS TE
- RSVP
- ISIS
- MPLS IP-VPN

## Make
- 配達物IoT/Logger
  - 特に国際郵便/貨物などで複数の配送業者を経由する場合内容物の状況といつ，どの時点でそうなったかという情報を知りたい
  - 荷物にGPSと各種センサ（加速度，ジャイロ，ひずみ，温度，湿度，光など）をつけて荷物が自分の想定通りの環境を保って配送されたかを確認できるソリューションがあってもよくないか
  - 特に高価なもの，複数の配送業者を経由するものなどでは十分需要があるのではないか．
  - LTE等を利用したリアルタイムトラッキングは貨物要件(航空機)等の需要で厳しい可能性はあるが，電池動作でGPS等は受信のみなので電波を発しない部分だけでも良い気がする．
- ESP-WROOM-02でAPIがうてる物理リモコン
- ESP-WROOM-02 x SSR API switching
- 郵便到着通知IoT
- ガフガン3D
- 光ピロピロ，普通のピロピロ．トーンプローブ．イベント会場で紐の行き先がわからなくなる．
  - [Pro3000™ Tone and Probe Series | Fluke Networks](https://jp.flukenetworks.com/datacom-cabling/installation-tools/Pro3000-Analog-Tone-and-Probe)
    - だいたい下記．
    ```
    Pro3000 アナログ式トーナーの仕様

    ユーザー・インターフェース
      継続またはトーン・モードを選択するスライド・スイッチ
      SOLID、ALT、OFF を選択する押しボタン・スイッチ

    連続周波数: 1000 Hz 公称値
    交互周波数: 1000/1500 Hz 公称値
    過電圧保護: トーナー / 極性モードで60 Vdc
    トーン・モードでの出力パワー: 600 Ω にて 8 dBm
    導通モードの出力電圧レベル: 新しいバッテリーで8 Vdc
    バッテリー: 9V アルカリ
    ```
    - [signal - How does a cable tone-and-probe kit work? - Electrical Engineering Stack Exchange](https://electronics.stackexchange.com/questions/144267/how-does-a-cable-tone-and-probe-kit-work)
    - 仕組み的には上記っぽい．
- [VisiFault™ 可視光源 - 光ケーブル導通テスター | フルーク・ネットワークス](https://jp.flukenetworks.com/datacom-cabling/fiber-testing/VisiFault-Visual-Fault-Locator)
  - だいたい下記くらいのspecificationっぽい．レンズで十分集光してfibreの中に突っ込まないとダメそう．
    ```
    波長: 650 nm （可視波長）
    レーザーの安全等級: クラスII
    出力: < 1.3 mW
    出力モード: 連続（一定）およびフラッシュ（2 ～ 3Hz パルス）
    ```
  - たぶんfs.comで3000円くらいの光源買ったほうがいい．
- 電源をモニタリングする何か．電源品質，電圧電流がwebでグラフ化できるとか．周波数とかも．
- 望遠鏡
- カンバン方式なブレインストーミングツール
- VU用スタックマッチ
- JTとかdigital通信の免許
- YCQマイクアンプ
- いいVM
- QSLカードをOCRしてみる
- スポット溶接機
- YCQナレッジまとめ
- ANTアナライザ回路設計
- オーディオスペクトラ
- USB電流計
- 時計の方が回る時計
- I2CでSFPからデータ抜いてみたい
- FKF_HAMLAB
  - 直近のtodo
    - bootstrapちゃんといれる.
  - CW Freak web version
  - 周波数データベース的な奴！webで編集（追加？）できて，webで検索できる！
  - TTDBと周波数帳のDB仕様まとめ
  - フリスの公式計算機
  - コールサインと自分の緯度経度から自動計算くんGLから国がすぐにわかるくん
  - バカンスをいいかんじに
    - チケットシステム
      - ticket，messageモデル生成
      - 見た目
      - オープンチケット，割り当てチケットの表示
      - チケット絞り込み機能実装
      - message と連携．ticketクリックしたらちゃんと関連のmessagesがソートされて出てくるように．
    - vacance
      - 見た目一旦調整
      - placeもdraggableに
      - placenにindex_id column追加か．
      - Operatorフィールドを実装
      - server sideリファクタ
      - client side リファクタ
      - websocket実装
    - QSO-Logging
      - 見た目
      - validation 追加
      - javascでinput formの最適化
      - csv出力機能等の追加
    - Others
      - Operator list 自動生成
      - contest スコアリングsystem実装
      - slimを用いて書き直す．
- AD8362を用いたパワーセンサ設計
- 回路設計: D-amp, イコライザ，ヘッドホンアンプ
- 電子負荷装置回路設計
- やすりホルダ
- ベクトル制御
- makefileのかきかた
- 経路可視化面白そう．GoBGPあたり連携してできないかな．

## 資格(上にあるほど狙いたい度が高い)
- FP2級
- 簿記2級
- 2級ボイラー技士
- CKA
- CISSP
- 技術士
- 一総通
- 気象予報士
- [公益社団法人日本サウナ・スパ協会のホームページ](https://www.sauna.or.jp/adviser/)
- CCNP/CCIE
- 危険物甲種
- ワインエキスパート
- Oracle master
- LPIC(もうとらなくていいか...) -> LinuC?
- G検定
- FE/PE試験を受けたい
- 救命講習？
- Cカード
- AWS系資格(AWS Solution Arch. Assoc.?はとりたい．)
- 日本さかな検定
- ハーブ検定
- 身に付けたい能力
  - 手話技能検定/手話通訳士
  - 中国語，スペイン語 or フランス語
- [パンシェルジュ検定 公式サイト](https://www.kentei-uketsuke.com/pancierge/)
- [星検（星空宇宙天文検定）：星検協会（一般社団法人星空宇宙天文検定協会）が主催する検定試験](https://www.hoshiken.org/)
- [旅行地理検定](https://www.chirikentei.jp/)

### 長期的に
  - 2級鍵師
  - 宅地建物取引士
  - 中小企業診断士
  - 旅行業務取扱管理者
  - 総合旅行業務取扱管理者
  - 放射線取扱主任者
  - 学芸員
  - 弁理士
  - 社会保険労務士
  - 証券アナリスト
  - 統計検定1級
  - 医療情報技師検定試験
  - comptia?([CompTIA CTT+ (TK0-201)　類似問題：解答｜CompTIA JAPAN (コンプティア 日本支局)](https://www.comptia.jp/cert_about/samplequestion/comptia_ctt/tk0-201_answer.html))
  - 高圧ガス製造保安責任者: 冷凍2種くらいは最低．乙種化学・乙種機械責任者とかadditional.
  - 毒劇物取扱者
  - クレーンデリック
  - 衛生管理者
    - 労働衛生の実務経験が必須のため難しそう
  - 甲種・乙種火薬類取扱保安責任者 17000円
  - 消防設備士
  - エネルギー管理士

## 本を読む
- [全てのプログラマが読むべき本 まとめ - 5/75 ページ](http://cielquis.net/programming-books/5.html)
- ソーシャル物理学
- 安全なwebアプリケーションの作り方

## etc
- ラジオメータを光らせる
  - 電界結合もやってみたいから，電界結合でも共鳴でも付け替えればOKみたいな感じにしたい．レギュ入れて．
- 顕微鏡のLEDね．
- 電気壁 vs 磁気壁
- ycq
  - ナレッジベース，
  - 上級部員が把握しておくべきこと
  - コンテスト参加の流れ
- QSLカードOCR
- kyuko_cal をlambda化できる気が
  - つーかkyuko-calのpromotionしたほうがいいんじゃ．
- ナミヤ雑貨店の奇蹟
- NW-L1な本を書いてみるとか．なかなかない．
- モールスでニュース記事を読み上げる君
- goでmibブラウザ
- [AWS 認定 ソリューションアーキテクト - プロフェッショナル | AWS](https://pages.awscloud.com/TRAINCERT_GOTOPRO_Confirmation.html)
- [統計検定 1 級に合格する方法 - Qiita](https://qiita.com/drken/items/089b8443305df047b44e)
  - いつかチャレンジしてみたい
- react
- zookeeper
- hadoop
- etcd
- kubeadm
- kafka

## blog
- spectrum analyzer修理した話

## 言語化リスト
言語化リスト（言語化してせつめいしたいことがでたとき，再利用性が有りそうなものについて説明資料を設ける
- スライドにまとめていく
- 調べればわかることのレベル感の話
