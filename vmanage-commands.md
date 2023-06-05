request nms statistics-db status

request nms all status

request nms all diagnostics

tail -f -n 100 /var/log/nms/vmanage-neo4j-out.log

request nms cloud-agent status
request nms cloud-agent-v2 status

Troubleshooting Control Connections
===================
https://www.cisco.com/c/en/us/support/docs/routers/sd-wan/214509-troubleshoot-control-connections.html

show control local-properties

tcpdump vpn 0 interface ge0/1 options "host 203.0.113.147 -n"

show control connections-history

show control connections

Change the Database Password
=====
Example

    For Cisco SD-WAN Release 20.1.1 and earlier:
    request nms configuration-db update-admin-user username neo4j password ******** newusername myusername newpassword mypassword

    For releases from Cisco SD-WAN Release 20.1.2:

    request nms configuration-db update-admin-user

    Enter current user name: neo4j

    Enter current user password: password

    Enter new user name: myusername

    Enter new user password: mypassword
