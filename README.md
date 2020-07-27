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

More details at : https://www.pgpool.net/docs/latest/en/html/sql-commands.html 

PCP commands
------------------------------

Get pgpool status using backend node ID
```bash
pgpool@pgpool01:~$ pcp_node_info -h 127.0.0.1 -U pgpool -p 9898 --verbose 0
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

Example output with standby node
```bash
pgpool@pgpool01:~$ pcp_node_info -h 127.0.0.1 -U pgpool -p 9898 --verbose 1
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
pgpool@pgpool01:~$ pcp_attach_node -h 127.0.0.1 -U pgpool -p 9898 -n 1
```

More details at : https://www.pgpool.net/docs/latest/en/html/pcp-commands.html 

License
----------

MIT / BSD
