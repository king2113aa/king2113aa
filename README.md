#!/bin/bash
sudo apt-get install iptables-persistent -y

for url in `sed '/^$/d; /#/d' domains.txt`; do
   echo "unblocking $url"
   iptables -D OUTPUT -p tcp -m string --string "$url" --algo kmp -j REJECT
done

IPTABLES_DIR="/etc/iptables/"
IPTABLES_V4_RULES="/etc/iptables/rules.v4"
[ ! -d "$IPTABLES_DIR" ] && mkdir $IPTABLES_DIR
rm -f "$IPTABLES_V4_RULES"

sudo bash -c "iptables-save > $IPTABLES_V4_RULES"
echo "persisted rules to $IPTABLES_V4_RULES"
