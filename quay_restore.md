1. Restore the backed up Quay configuration and the randomly generated keys:

```
oc create -f ./config-bundle.yaml
```

```
oc create -f ./managed-secret-keys.yaml
```

## if show the error message Error from server (AlreadyExists): error when creating "./config-bundle.yaml": secrets "config-bundle-secret" already exists

![image](https://github.com/user-attachments/assets/25e9fafd-0418-400b-8b45-3deee49e741d)
![image](https://github.com/user-attachments/assets/85d28f8a-3b7a-4e14-9984-417ea358afbf)


please use below command to delete the existing file and re run the create command
```
oc delete Secret config-bundle-secret -n quay-test
oc delete secret example-registry-quay-registry-managed-secret-keys -n quay-test

```
![image](https://github.com/user-attachments/assets/279e7d30-ad49-4ead-9ffc-8c2644494a40)

2. Restore the QuayRegistry custom resource

```
oc create -f ./quay-registry.yaml
```

3. Scale down the Quay operator & namespace

```
oc scale --replicas=0 deployment $(oc get deployment -n quay-test |awk '/^quay-operator/ {print $1}') -n quay-test
oc scale --replicas=0 deployment $(oc get deployment -n quay-test -l quay-component=quay -o jsonpath='{.items[0].metadata.name}') -n quay-test

```

![image](https://github.com/user-attachments/assets/03ac5d56-639c-4a2e-9d2b-513a23648255)


4. find out the database pod
```
oc get pod
oc get pod -l quay-component=postgres -n quay-test -o jsonpath='{.items[0].metadata.name}'
```

##example-registry-quay-database-79f8764fc6-b9ghj


5. copy the database backup to the database pod

```
oc cp ./backup.sql -n quay-test example-registry-quay-database-79f8764fc6-b9ghj:/tmp/backup.sql
```


6. go into the pod

```
oc rsh -n quay-test example-registry-quay-database-79f8764fc6-b9ghj
```

7. use psql to drop db

```
psql
\l
DROP DATABASE "example-registry-quay-database";
\q
```

![image](https://github.com/user-attachments/assets/f5eb92f5-541f-4aa4-9ef0-b65ea88d4a74)
![image](https://github.com/user-attachments/assets/6be1efbe-2fbf-4edc-9925-a875edf414d1)

8. use psql to restore db

```
psql < /tmp/backup.sql
exit
```

9. Scale up the quay operator and namespace

```
oc scale --replicas=1 deployment $(oc get deployment -n quay-test |awk '/^quay-operator/ {print $1}') -n quay-test
oc scale --replicas=1 deployment $(oc get deployment -n quay-test -l quay-component=quay -o jsonpath='{.items[0].metadata.name}') -n quay-test
```

```
## check the status is that up
oc get quayregistry -n quay-test example-registry -o yaml
```



