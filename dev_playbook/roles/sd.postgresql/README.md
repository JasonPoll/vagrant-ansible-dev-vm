## SD - Postgresql : borrowed from ANXS - PostgreSQL 

Credit where it's due: https://github.com/ANXS/postgresql

Ansible role which installs and configures PostgreSQL, extensions, databases and users.



#### Variables

```yaml
# Basic settings
postgresql_version: 13
postgresql_encoding: "UTF-8"
postgresql_locale: "en_US.UTF-8"
postgresql_ctype: "en_US.UTF-8"

postgresql_admin_user: "postgres"
postgresql_default_auth_method: "peer"

postgresql_service_enabled: false # should the service be enabled, default is true

postgresql_cluster_name: "main"
postgresql_cluster_reset: false

# List of databases to be created (optional)
# Note: for more flexibility with extensions use the postgresql_database_extensions setting.
postgresql_databases:
  - name: foobar
    owner: baz          # optional; specify the owner of the database
    hstore: yes         # flag to install the hstore extension on this database (yes/no)
    uuid_ossp: yes      # flag to install the uuid-ossp extension on this database (yes/no)
    citext: yes         # flag to install the citext extension on this database (yes/no)
    encoding: "UTF-8"   # override global {{ postgresql_encoding }} variable per database
    state: "present"    # optional; one of 'present', 'absent', 'dump', 'restore'
    lc_collate: "en_GB.UTF-8"   # override global {{ postgresql_locale }} variable per database
    lc_ctype: "en_GB.UTF-8"     # override global {{ postgresql_ctype }} variable per database

# List of database extensions to be created (optional)
postgresql_database_extensions:
  - db: foobar
    extensions:
      - hstore
      - citext

# List of users to be created (optional)
postgresql_users:
  - name: baz
    pass: pass
    encrypted: yes  # if password should be encrypted, postgresql >= 10 does only accepts encrypted passwords
    state: "present"    # optional; one of 'present', 'absent'

# List of schemas to be created (optional)
postgresql_database_schemas:
  - database: foobar           # database name
    schema: acme               # schema name
    state: present

  - database: foobar           # database name
    schema: acme_baz           # schema name
    owner: baz                 # owner name
    state: present

# List of user privileges to be applied (optional)
postgresql_user_privileges:
  - name: baz                   # user name
    db: foobar                  # database
    priv: "ALL"                 # privilege string format: example: INSERT,UPDATE/table:SELECT/anothertable:ALL
    role_attr_flags: "CREATEDB" # role attribute flags
```

There's a lot more knobs and bolts to set, which you can find in the [defaults/main.yml](./defaults/main.yml)


#### License

Licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.
