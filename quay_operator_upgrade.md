1. stop the clair & quay container operation

```
-kind: clair
manage: true
override:
  replicas: 0

-kind: quay
manage: true
override:
  replicas: 0

```

2.  check the pods are already stoped
3.  backup the DB after the pods is stopped.

```
oc exec<database name> -- /usr/bin/pg_dump -C example-registry-quay-database  > backup.sql
```

4. copy the backup file to s3 stroage

```
aws --no-verify-ssl --endpoint-url http://192.168.91.136:9000 s3 cp backup.sql s3://quay/datastorage/registry/
```

5. go to the UI page and select the install operator -> subscription -> select the target version

![image](https://github.com/user-attachments/assets/fb706ff8-5c1c-4a89-8bf1-bcffd5597edb)


6. wait for the application update after update the version.

7. check the DB version (if operator has updated to v3.13 and more, psql version should be v15.8)

```
oc rsh <database_name>
psql
\l
select version();
\q

exit
```
8. 
