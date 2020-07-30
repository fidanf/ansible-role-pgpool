PgPool
=========

Installs and configures PgPool-II for Debian/Ubuntu. The default _running mode_ is **streaming replication mode**.

Requirements
------------

Ansible >=2.9

Role Variables
--------------

TODO

Dependencies
------------

None

Example Playbook
----------------

TODO

Configuration checking commands
------------------------------

:warning: PgPool instance must be running

Show all configuration parameters
```bash
pgpool@pgpool01:~$ psql -h localhost -p 9999 -U admin -d postgres -c 'PGPOOL SHOW ALL' 
```

Show pool status
```bash
pgpool@pgpool01:~$ psql -h localhost -p 9999 -U admin -d postgres -c 'show pool_nodes'
 node_id | hostname | port | status | lb_weight |  role   | select_cnt | load_balance_node | replication_delay | replication_state | replication_sync_state | last_status_change
---------+----------+------+--------+-----------+---------+------------+-------------------+-------------------+-------------------+------------------------+---------------------
 0       | pgsql01  | 5432 | up     | 0.333333  | primary | 30         | true              | 0                 |                   |                        | 2020-07-27 14:50:55
 1       | pgsql02  | 5432 | up     | 0.333333  | standby | 13         | false             | 0                 | streaming         | async                  | 2020-07-27 15:26:15
 2       | pgsql03  | 5432 | up     | 0.333333  | standby | 83         | false             | 0                 | streaming         | async                  | 2020-07-27 14:50:55
(3 rows)
```

Feel free to create a .pgpass file for pgpool user in order to skip repetitive password prompts

More details at : https://www.pgpool.net/docs/latest/en/html/sql-commands.html 

:warning: If for any reason PgPool's internal status is no longer in sync with Postgresql Replication' status, issue the following commands :
```bash
root@pgpool01:~$ systemctl stop pgool2
root@pgpool01:~$ rm -f /var/log/pgpool/pgpool_status
root@pgpool01:~$ sudo systemctl restart pgpool2
```

PCP commands
------------

Get pgpool status of backend node ID 0 
```bash
pgpool@pgpool01:~$ pcp_node_info -h 127.0.0.1 -U pgpool -w -v 0
Password:
Hostname               : pgsql01
Port                   : 5432
Status                 : 2
Weight                 : 0.333333
Status Name            : up
Role                   : primary
Replication Delay      : 0
Replication State      :
Replication Sync State :
Last Status Change     : 2020-07-27 14:50:55
```

Same thing against standby node
```bash
pgpool@pgpool01:~$ pcp_node_info -h 127.0.0.1 -U pgpool -w -v 1
Password:
Hostname               : pgsql02
Port                   : 5432
Status                 : 2
Weight                 : 0.333333
Status Name            : up
Role                   : standby
Replication Delay      : 0
Replication State      : streaming
Replication Sync State : async
Last Status Change     : 2020-07-27 14:50:55
```

Attach pgpool node and give it and ID
```bash
pgpool@pgpool01:~$ pcp_attach_node -h 127.0.0.1 -U pgpool -w -n 3
```

Display the parameter values as defined in pgpool.conf
```bash
pgpool@pgpool01:~$ pcp_pool_status -h /var/run/pcp -U pgpool -w
```

More details at : https://www.pgpool.net/docs/latest/en/html/pcp-commands.html 

License
-------

MIT / BSD
