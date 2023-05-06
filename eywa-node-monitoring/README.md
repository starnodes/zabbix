## Eywa node monitoring

Template for Zabbix 6

On the server, open port 8081/tcp for the IP of your Zabbix server

Add the template to your node on the Zabbix server and change the {$NODE_URL} macro as described

Now Zabbix server will notify you if the node stops working or synchronization is not equal to FULLY_SYNCED