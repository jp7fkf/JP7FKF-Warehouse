# Todo
  - つくりたいものとか，やりたいこととか．

## Make
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
      - 
    - [VisiFault™ 可視光源 - 光ケーブル導通テスター | フルーク・ネットワークス](https://jp.flukenetworks.com/datacom-cabling/fiber-testing/VisiFault-Visual-Fault-Locator)
      - だいたい下記くらいのspecificationっぽい．レンズで十分集光してfibreの中に突っ込まないとダメそう．
        ```
        波長: 650 nm （可視波長）
        レーザーの安全等級: クラスII
        出力: < 1.3 mW
        出力モード: 連続（一定）およびフラッシュ（2 ～ 3Hz パルス）
        ```
  - 電源をモニタリングする何か．電源品質，電圧電流がwebでグラフ化できるとか．周波数とかも．
  - 望遠鏡
  - カンバン方式なブレインストーミングツール
  - VU用スタックマッチ
  - JTとかdigital通信の免許
  - YCQマイクアンプ
  - いいVM
  - QSLカードをOCRしてみる
  - dotfilesまとめる
  - スポット溶接機
  - YCQナレッジまとめ
  - ANTアナライザ回路設計
  - オーディオスペクトラ
  - USB電流計
  - 時計の方が回る時計
  - I2CでSFPからデータ抜いてみたい
  - FKF_HAMLAB
    - 直近のtodo
      - bootstrapちゃんといれる．
      
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

## 資格
  - 一総通
  - 潜水士
  - Xray作業主任
  - 簿記3級
  - FE/PE試験を受けたい
  - 救命講習？
  - Oracle master
  - ワインエキスパート
  - CCNP
  - LPIC(もうとらなくていいか...) -> LinuC?
  - エンベデットシステムスペシャリスト
  - 情報処理安全確保支援士
  - エネ管
  - Cカード
  - AWS系資格(AWS Solution Arch. Assoc.?はとりたい．)
  - 身に付けたい能力
    - 手話技能検定/手話通訳士
    - スペイン語 or フランス語
  - 長期的に
    - 弁理士
    - 社会保険労務士
    - 証券アナリスト
    - 統計検定1級
    - 医療情報技師検定試験
    - comptia?(https://www.comptia.jp/cert_about/samplequestion/comptia_ctt/tk0-201_answer.html)
    - 旅行業務取扱管理者
    - 総合旅行業務取扱管理者

## 本を読む
  - [全てのプログラマが読むべき本 まとめ - 5/75 ページ](http://cielquis.net/programming-books/5.html)
  - ソーシャル物理学
  - 安全なwebアプリケーションの作り方

### In Progress
  - 秋月USB-DAC付きD球アンプ改造

## etc
- ラジオメータを光らせる
  - 電界結合もやってみたいから，電界結合でも共鳴でも付け替えればOKみたいな．感じにしたい．レギュ入れて．
- 顕微鏡のLEDね．
- 電気壁 vs 磁気壁
- ycq
  - ナレッジベース，
  - 上級部員が把握しておくべきこと
  - コンテスト参加の流れ
- QSLカードOCR
- kyuko_cal をlambda化できる気が
  - つーかkyuko-calのpromotionしたほうがいいんじゃ．
- 