# SchoolData
Plays for managing your data safely

Note: The following commands assume you're dealing with a hardened droplet created using `School`


## Setup master and slaves

Run:

```
ansible-playbook datas.yml -i inventory/digital_ocean.py
```

This will setup a master and 1 or more slaves (with hot_standby).

## Add a new slave to the cluster

Simply update inventory and run the above playbook

Optional `--limit=slave` will not run anything against the master db

## Verify replication

```
ansible-playbook verify_replication.yml -i inventory
```

Will:

1. create a database on master
2. for-each slave: verify database exists
3. remove database on master

## Changing the master (coming soon)

Update inventory to reflect your desired new configuration (you will need to add an `[oldmaster]` host block:

then run:

```
ansible-playbook promote.yml -i inventory
```

Will:

1. Stop `oldmaster`
2. Stop `master` (new master)
3. Update postgres.conf (on both)
4. Update recovery.conf on all slaves


## Postgres commands:

Some postgres commands worth knowing:

**Stop, start, restart and status:**

```
systemctl status|stop|start
```