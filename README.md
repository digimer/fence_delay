# fence_delay

This is a fence agent that always fails after a given delay. It is meant to give other fence devices time to recover before a fence is attempted again.

Example use case;

A cluster has two fence devices;

1. IPMI 
2. PDUs

In a situation where both fence methods fail on the initial try, after the PDU fails, the cluster will again try the IPMI method. The problem is that the IPMI BMC doesn't get enough time to boot again. As such, it will never have a chance to succeed. By adding this fence agent as a third method, the IPMI BMC has enough time to boot and come fully inline before it is tried again.

1. fence_ipmilan ... -o restart
2. fence_apc_pdu ... -o off; fence_apc_pdu ... -o on
3. fence_delay --delay 60 -o restart

In this scenario, the IPMI BMC gets 60 seconds to boot before the cluster will try to use it.
