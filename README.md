# cme_identitysharing

## Usage

- have a Scale Set functional at Azure, OCI, AWS etc.
- have appropriate CME configuration so scale set is managed
- have connectivity between sharing gateway and scale set configured
 - using Ports tcp/28581 and tcp/15105 (Outgoing/Incoming)
 - in default, the main addresses (i.e. cluster objects main IP - often internet facing IP)
  - if main IP is not the IP designated to be used, can be overridden via GUIDBEdit
   - search for "ia_control_connections_ip" at sharing gateway (class name "gateway_ckp") object and enter IP as value (then save and install policy)

- On Management Server:

```
mkdir /scripts
curl_cli -k 'https://raw.githubusercontent.com/leinadred/CP_Indentity-Sharing-with-CME/refs/heads/master/cme_identitysharing.bash' -O /scripts/cme_identitysharing.bash
chmod a+x /scripts/cme_identitysharing.bash
autoprov-cfg set management -cs '/scripts/cme_identitysharing.bash'
autoprov-cfg set template -tn '<cme_template_name>' -cp 'IDSHARING <policy_package_name_of_receiving_device> <name_of_sharing_device> <policy_package_name_of_sharing_device>`
```
! In case the sharing gateway is a cluster, you will have to change one line:

```	GW_JSON_SH=$(mgmt_cli --session-id $SID show simple-gateway name $IDSHARINGGW -f json)```

to

```GW_JSON_SH=$(mgmt_cli --session-id $SID show simple-cluster name $IDSHARINGGW -f json)```

Additionally Identity Awareness has to be added on receiving cme managed gateway. 

```autoprov-cfg set template -tn <cme_template_name> -ia```

After this all new scale set instances are set up to get identities from sharing gateway.
