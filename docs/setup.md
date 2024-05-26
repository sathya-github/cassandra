# Cassandra 


## To make the loopback interfaces available for Cassandra

    sudo ifconfig lo0 alias 127.0.0.2
    sudo ifconfig lo0 alias 127.0.0.3
    sudo ifconfig lo0 alias 127.0.0.4
    sudo ifconfig lo0 alias 127.0.0.5
    sudo ifconfig lo0 alias 127.0.0.6


## Create a Cluster through CCM

    ccm create easybuy -v 4.1.5 -n 5 -s --pwd-auth


## CCM Commands

    ccm list
    ccm status
    ccm stop
    ccm start
    ccm remove
    ccm node1 show

## cqlsh connection

To override the default host/port for connecting to Cassandra from CQLSH. Default port 9042

    CQLSH_HOST
    CQLSH_PORT

To login to Cassandra

    cqlsh -u cassandra -p cassandra

To set a specific version of Python. CQLSH seems to work with Python 3.9 and not with 3.12

    CQLSH_PYTHON=python3.9


## Roles
    create role appuser with password = 'password' and login = true and superuser = true;
    create role dummy with password = 'password1' and login = true;

### List all the roles
    list roles;

### Drop role
    drop role dummy;    