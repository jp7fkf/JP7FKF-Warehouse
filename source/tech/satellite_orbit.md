# 衛星・軌道

## 衛星の軌道
- 高度による分類
  - 低軌道(LEO: Low Earth Orbit)
    - 高度2,000km以下
    - ISSなど
  - 中軌道(MEO: Midium Earth Orbit)
    - 高度2,000kmから地球同期軌道(35,786km)
    - 衛星コンスタレーションによる通信衛星など
    - a.k.a. ICO(Intermediate Circular Orbit)
  - 高軌道(HEO: High Earth Orbit)
    - 地球同期軌道/対地同期軌道(35,786km)より外
- 軌道傾斜角による分類　
  軌道傾斜角とは赤道面と軌道面のなす角のこと．
  - 傾斜軌道(Inclined orbit)
    - 衛星の軌道傾斜角が惑星の赤道に対して傾いている軌道
  - 極軌道(Polar Orbit)
    - 惑星の極，または極近傍の上空を通過する軌道．軌道傾斜角は90°に近くなる．
    - ex.) 極太陽同期軌道
      - 極軌道に近く，赤道を常に同じ現地時間で通過する軌道．影が常に同じ場所にできるので画像の撮影に便利である．
  - 順行軌道(prograde orbit)
    - 軌道傾斜角が90°以下の軌道．惑星の自転と同方向に周回する．
  - 逆行軌道(retrograde orbit)
    - 軌道傾斜角が90°以上の軌道．惑星の自転方向とは逆向きに周回する．太陽同期軌道は逆行軌道の内の一つである．
- 離心率による分類　
  - 円軌道(Circular orbit)
    - 軌道離心率が0．円の形をした軌道．
  - 楕円軌道(Elliptic orbit)
    - 軌道離心率が0より大きく1より小さい軌道．楕円を描く．軌道離心率が特に大きいものは，長楕円軌道(Highly elliptical orbit,HEO)と呼ばれる．
    - ex.) 静止トランスファ軌道 (GTO)
      - 近地点が低軌道上で，遠地点が静止軌道上にある楕円軌道を代表例とする，静止軌道への遷移を目的とした軌道（という意味では，パラメータではなく目的に着目した分類である．たとえば通常のGTOの他，スーパシンクロナス・トランスファ軌道がある．詳しくは静止トランスファ軌道の記事を参照）．
    - ex.) モルニア軌道(Molniya orbit)
      軌道傾斜角が63.4°で，周回周期が地球の自転周期の半分である楕円軌道．
    - ex.) ツンドラ軌道(Tundra orbit)
      - 軌道傾斜角が63.4°で，周回周期が地球の自転周期と同じである楕円軌道．
    - ex.) 準天頂軌道 (QZO: quasi-zenith orbit)
      - 適切な軌道傾斜角と軌道離心率を持たせることで，特定の1地域の上空に長時間とどまる軌道．日本の準天頂軌道衛星「みちびき」は，離心率がおよそ0.1で，軌道傾斜角がおよそ45°，周回周期が地球の自転周期と同じである楕円軌道．
  - 双曲線軌道(hyperbolic trajectory)
    - 1以上の離心率を持つ軌道．第二宇宙速度以上の速度を持ち，天体の引力を振り切る．
  - 放物線軌道(parabolic trajectory)
    - 離心率が1である軌道．第二宇宙速度と同じ速度を持ち，地球の引力を振り切る．速度が増加すれば双曲線軌道になる．
    - 脱出軌道(EO: escape orbit)
      - 物体が第二宇宙速度で地球から離れていく放物線軌道．
    - 捕捉軌道(CO: capture orbit)
      - 物体が第二宇宙速度で地球に近づいていく放物線軌道．
- 周期性による分類
  - 回帰軌道(recurrent orbit, Repeat Ground Track Orbit)
    - 人工衛星の１日当たりの周回数がちょうど整数になる軌道．地球が１回転する間に，衛星が何回か地球をまわり，次の日の同じ時刻に，同じ地点の上空に再びその衛星が飛来する．
    - ex.) 同期軌道 (SO: Synchronous Orbit)
      - 惑星の自転周期と衛星の公転周期が等しい軌道．地上観測者から見ると衛星はアナレンマ上を動く．
      - ex.) 対地同期軌道 (GEO: Geo Stationary Orbit, Geosynchronous Equatorial Orbit)
        - 地球を周回する同期軌道．高度約，35,786 km．
        - ex.) 静止軌道 (GSO: Geosynchronous Orbit)
          - 軌道傾斜角が0°の対地同期軌道．地上の観測者からは衛星が空に固定されているように見える[3]．アーサー・C・クラークに因んでクラーク軌道とも呼ばれる．
        - ex.) 墓場軌道
          - 地球同期軌道の数百km上の軌道．衛星は任務終了時にここに移動する．
        - ex.) 分同期軌道
          - 静止軌道・地球同期軌道のすぐ下にあるドリフト軌道．衛星は東にドリフトする．
    - ex.) 準同期軌道(semi-geo-synchronous orbit, Sub-Synchronous Orbit)
      - 公転周期が惑星の自転周期の2分の1に等しい軌道．
  - 準回帰軌道(Quasi-recurrent Orbit)
    - 1日のうちに地球を何度か周回し，その日のうちには戻らないが，定数日後に元の地表面上空に戻る軌道．
    - ex.) 太陽同期準回帰軌道
      - 準回帰軌道で，かつ太陽同期軌道である衛星軌道．
  - 太陽同期軌道 (SSO: Sub-Synchronous Orbit)
    - 衛星の軌道面に入射する太陽からの光の角度が同じになる軌道．極軌道に近く，赤道を常に同じ現地時間で通過する軌道．太陽同期軌道（衛星側）から地球を見ると太陽光の入射角が常に同じとなり，同一条件下での地球表面の観測が可能となるので地球観測衛星などの画像の撮影に適している．

### Referenes
- [人工衛星の軌道 - Wikipedia](https://ja.wikipedia.org/wiki/%E4%BA%BA%E5%B7%A5%E8%A1%9B%E6%98%9F%E3%81%AE%E8%BB%8C%E9%81%93)

## 宇宙速度(Cosmic Velocity)
- 第一(first cosmic velocity)
  - 約7.9km/s(28,400 km/h)
- 第二(second cosmic velocity)
  - 約11.2km/s(40,300 km/h）
  - 第一宇宙速度の sqrt(2)倍
- 第三(third cosmic velocity)
  - 約16.7km/s(60,100 km/h)

## [LIVE REAL TIME SATELLITE TRACKING AND PREDICTIONS](https://www.n2yo.com/)

## [Search Satellite Database](https://www.n2yo.com/database/)

## [Welcome to the NSSDCA](https://nssdc.gsfc.nasa.gov/)
- [NASA - NSSDCA - Master Catalog - Spacecraft Query](https://nssdc.gsfc.nasa.gov/nmc/SpacecraftQuery.jsp)

## [国際衛星識別符号 - Wikipedia](https://ja.wikipedia.org/wiki/%E5%9B%BD%E9%9A%9B%E8%A1%9B%E6%98%9F%E8%AD%98%E5%88%A5%E7%AC%A6%E5%8F%B7)

## [高軌道 - Wikipedia](https://ja.wikipedia.org/wiki/%E9%AB%98%E8%BB%8C%E9%81%93)
