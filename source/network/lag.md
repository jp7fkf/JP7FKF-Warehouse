# LAG/LACP

## LAGとは
- Link Aggregation
- 複数の物理回線を論理的には1回線とみなして利用する．
- 帯域拡張，冗長性の担保等に用いられる．

## LAGの種類
- static
- dynamic
- multi-chassis(MLAG)

## LACP
- 802.3adとかをみる．
- LACPDUを送信する感覚が複数あるものが多い(Fast/SLow etc...)
- LACP activeだとLACPDUを常に送信する．LACP PassiveだとLACPDUを受信した場合のみ送信する．

- role & name
  - Actor: LACP送信インタフェース(送信機器)
  - Partner: LACP 受信インタフェース(対向機器)
  - Collector:  LACP 対向機器
  - Distributer : LACP 送信機器

- info
  Aggregation (集約): Individual or Aggregatable (個別または集約可能)-
                      whether this port can form an aggregation with other ports, or if it
                      can only form an aggregation with itself
                      (このポートが他のポートとの集約を形成できるか、
                      またはそのポート自体との集約のみを形成するか): the port in a one-adapter
                      IEEE 802.3ad Link Aggregation will be marked as Individual, or
                      Aggregatable if there are more than one ports
                      (複数のポートがある場合は、1 アダプター
                      IEEE 802.3ad リンク集約内のポートが Individual (個別) または Aggregatable
                      (集約可能) とマークされます)
   Synchronization (同期): IN_SYNC or OUT_OF_SYNC (IN_SYNC または  OUT_OF_SYNC)-
                  whether the aggregation has determined it has reached synchronization
                  with the partner
                  (集約がパートナーとの同期に到達したと判別したか)
   Collecting (収集): Enabled or Disabled (使用可能または使用不可)-
                  whether the IEEE 802.3ad Link Aggregation is collecting (receiving)
                  packets
                  (IEEE 802.3ad リンク集約がパケットを収集 (受信) するか)
   Distributing (配布): Enabled or Disabled (使用可能または使用不可)-
                  whether the IEEE 802.3ad Link Aggregation is distributing (sending)
                  packets
                  (IEEE 802.3ad リンク集約がパケットを配布 (送信) するか)
   Defaulted (デフォルト): True or False (真または偽)-
                  whether the IEEE 802.3ad Link Aggregation is using default values for the
                  partner's information
                  (IEEE 802.3ad リンク集約がパートナーの情報に対してデフォルト値を
                  使用するか)
   Expired (期限切れ): True or False (真または偽)-
                  whether the IEEE 802.3ad Link Aggregation is operated in expired mode
                  (IEEE 802.3ad リンク集約が期限切れモードで動作するか)
  - ref: https://www.ibm.com/support/knowledgecenter/ja/ssw_aix_72/network/etherchannel_linkaggr_stats.html

## MLAG

## mLACP

### ICCP
