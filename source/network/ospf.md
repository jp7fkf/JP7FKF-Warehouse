# OSPF

## [OSPF attribute  Further information](https://www.forwardingflows.net/troubleshooting-ospf-neighbor-mismatch-effectively-junos/)
  - Unique router-id
  - Area ID
  - Area flags (Stub/NSSA flag)
  - Authentication type (MD5 (type 2), Simple (type 1) or None (type 0))
  - Authentication password (The authentication secret)
  - Hello/Dead interval
  - L3 MTU (The MTU announced on OSPF DBD packets)
Compatible network type (p2p, broadcastetc... The network type must be consistent on the network segment. You’d have a broken graph if one OSPF node is expecting to   - have a DR and other OSPF nodes aren’t.)
  - Network Mask (Since the network mask is coupled with the underlying graph, it has to match.)

## ospf ecmp
- https://rtodto.net/junos-ospf-equal-cost-path/
  - このcode blockの配色見やすい