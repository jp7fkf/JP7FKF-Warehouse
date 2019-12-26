# misc

## MTUとMSS
### MTU (Maximun Transfer Unit)
- MTU とは，一回のデータ転送で送信可能な"IPデータグラムの最大値"のこと．
  - つまりIPパケットのヘッダからしっぽまでを一度に送信できる最大データ量．
  - Ethernet環境において，Ethernetフレームが最大 1518bytes．
    - Ethernetヘッダ(14bytes)とFCS(4bytes)を除く1500bytesがMTUサイズとなる．
  - PPPoEの場合，Ethernetフレームにカプセル化する前に，PPPとPPPoEもカプセル化する．
    - PPP Header: 2 bytes
    - PPPoE Header: 6 bytes
    - -> PPPoEな環境のMTUは　1518 - 18 - 8 = 1492 bytes.

### MSS (Maximun Segment Size)
- 　MSSとは，TCPが格納するユーザデータで"受信可能なセグメントサイズの最大値"のこと．
  - IP Header: 20 bytes
  - TCP Header: 20 bytes
  - -> なので，Ethernet環境下では 1500 - 20 - 20 = 1480 bytes.

- ref: [MTU / MSS / RWINとは](https://www.infraexpert.com/info/5adsl.htm)

### Appendix: フレッツのMTU
- フレッツのMTUは 1454 bytes と指定されている．
- 通常のPPPoEでは1492 bytesであるのになぜこれより短くなるのか．

> Bフレッツ等のPPPoE接続は，NTT収容ビルにある機器(Flet's ADSLの場合はDSLAM)までであり，
> その機器から事業者網にある機器(BAS)との間においては，L2TP接続が行われています

- つまり，PPPoE終端であるNTT収容ビルまでは確実にMTU1492なのだが，その先のNTT収容局舎 - ISP事業者のBAS の間がL2TP接続となっている．
  - IP: 20 bytes
  - UDP: 8 bytes
  - L2TP: 12 bytes
  - PPP: 2 bytes
  - 1518 - 18(ethernet) - 20(IP) - 8(UDP) - 16(L2TP) - 2(PPP) = 1454 bytes

- ref: L2TP Header Format
  - [RFC 2661 - Layer Two Tunneling Protocol "L2TP"](https://tools.ietf.org/html/rfc2661)
```
This header is formatted:

    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |T|L|x|x|S|x|O|P|x|x|x|x|  Ver  |          Length (opt)         |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |           Tunnel ID           |           Session ID          |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |             Ns (opt)          |             Nr (opt)          |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |      Offset Size (opt)        |    Offset pad... (opt)
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   Figure 3.1 L2TP Message Header

   The Type (T) bit indicates the type of message. It is set to 0 for a
   data message and 1 for a control message.

   If the Length (L) bit is 1, the Length field is present. This bit
   MUST be set to 1 for control messages.

   The x bits are reserved for future extensions. All reserved bits MUST
   be set to 0 on outgoing messages and ignored on incoming messages.

   If the Sequence (S) bit is set to 1 the Ns and Nr fields are present.
   The S bit MUST be set to 1 for control messages.

   If the Offset (O) bit is 1, the Offset Size field is present. The O
   bit MUST be set to 0 (zero) for control messages.

   If the Priority (P) bit is 1, this data message should receive
   preferential treatment in its local queuing and transmission.  LCP
   echo requests used as a keepalive for the link, for instance, should
   generally be sent with this bit set to 1. Without it, a temporary
   interval of local congestion could result in interference with
   keepalive messages and unnecessary loss of the link. This feature is
   only for use with data messages. The P bit MUST be set to 0 for all
   control messages.

   Ver MUST be 2, indicating the version of the L2TP data message header
   described in this document. The value 1 is reserved to permit
   detection of L2F [RFC2341] packets should they arrive intermixed with
   L2TP packets. Packets received with an unknown Ver field MUST be
   discarded.

   The Length field indicates the total length of the message in octets.

   Tunnel ID indicates the identifier for the control connection. L2TP
   tunnels are named by identifiers that have local significance only.
   That is, the same tunnel will be given different Tunnel IDs by each
   end of the tunnel. Tunnel ID in each message is that of the intended
   recipient, not the sender. Tunnel IDs are selected and exchanged as
   Assigned Tunnel ID AVPs during the creation of a tunnel.

   Session ID indicates the identifier for a session within a tunnel.
   L2TP sessions are named by identifiers that have local significance
   only. That is, the same session will be given different Session IDs
   by each end of the session. Session ID in each message is that of the
   intended recipient, not the sender. Session IDs are selected and
   exchanged as Assigned Session ID AVPs during the creation of a
   session.

   Ns indicates the sequence number for this data or control message,
   beginning at zero and incrementing by one (modulo 2**16) for each
   message sent. See Section 5.8 and 5.4 for more information on using
   this field.

   Nr indicates the sequence number expected in the next control message
   to be received.  Thus, Nr is set to the Ns of the last in-order
   message received plus one (modulo 2**16). In data messages, Nr is
   reserved and, if present (as indicated by the S-bit), MUST be ignored
   upon receipt. See section 5.8 for more information on using this
   field in control messages.

   The Offset Size field, if present, specifies the number of octets
   past the L2TP header at which the payload data is expected to start.
   Actual data within the offset padding is undefined. If the offset
   field is present, the L2TP header ends after the last octet of the
   offset padding.

```

- ref: [Bフレッツやフレッツ光のMTUサイズ（1492ではなく1454の理由）](https://www.infraexpert.com/info/6adsl.htm)

## RWIN (TCP Receive Window)
- 相手側の確認応答 (Ack) を待たずに，一度に送信出来るデータサイズのこと．
  - 通信時の SYN/ACK のコネクションの際に，TCPが受信可能なMSSとRWINを相手に通知する．
  - 回線速度やPCの CPU/Memory などにより最適なサイズが異なる．