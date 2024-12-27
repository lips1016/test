1. backup the registry from operator

```
oc get quayregistry example-registry -n quay-test -o yaml > quay-registry.yaml
```

## delete 5 condition which is under metadata  
  metadata.creationTimestamp
  metadata.finalizers
  metadata.generation
  metadata.resourceVersion
  metadata.uid

2. backup the managed keys secret

```
oc get secret -n quay-test example-registry-quay-registry-managed-secret-keys -o yaml > managed-secret-keys.yaml
```

## delete owner reference


3. backup the quay configuration (config_bundle_secert)

```
oc get secret -n quay-test  $(oc get quayregistry example-registry -n quay-test  -o jsonpath='{.spec.configBundleSecret}') -o yaml > config-bundle.yaml
```

4. backup the quay-app config

```
oc get pod
oc exec -it example-registry-quay-app-776d4bf5c7-8jk9t -- cat /conf/stack/config.yaml > quay-config.yaml
```
## get pod name and copy the name

5. scale down the application (can mannual in OCP portal : deployment edit the replica_set = 0)

```
oc get deployment
oc scale --replicas=0 deployment example-registry-quay-app -n quay-test

#scale down the namespace
$ oc scale --replicas=0 deployment $(oc get deployment -n <quay-namespace> -l quay-component=quay -o jsonpath='{.items[0].metadata.name}') -n <quay-namespace>

```

##check the pod status
```
watch oc get pod -n quay-test
```

6. found out the database pod name

```
oc get pod
oc get pod -l quay-component=postgres -n quay-test -o jsonpath='{.items[0].metadata.name}'
```

###will response the pod name and copy it.


7. obtain the database name

```
oc -n quay-test rsh $(oc get pod -l app=quay -o NAME -n quay-test |head -n 1) cat /conf/stack/config.yaml|awk -F"/" '/^DB_URI/ {print $4}'example-registry-quay-database-7ff4d67996-pdjrk
```
8. bakcup database (pg_dump)

```
oc exec example-registry-quay-database-7ff4d67996-pdjrk -- /usr/bin/pg_dump -C example-registry-quay-database  > backup.sql
```

9. set the s3 cli credentials (marking the s3 storge accesskey & secertkey)

```
aws configure
access_key_id: IadI3mtsP6ODXGFrAXxO
secert_key: d30mdG6Pc4DMCOfuSZJL5fX7VN6osmSU5g9o51ZA
Default region name [None]: us-east-1
Default output format [None]: 
```
![image](https://github.com/user-attachments/assets/fad6397a-998e-49e2-9a68-09f190cfc2f8)

 ##file will auto genereate ./.aws/credentials

10. testing the connection

```
aws --no-verify-ssl --endpoint-url http://192.168.91.136:9000 s3 ls
```

![image](https://github.com/user-attachments/assets/72926326-bc02-44ad-a63e-ac5cb3f0f017)

##since minio is non-secure which is using http. need to add the condition --no-verify-ssl

11. s3 sync file to minio stroage

```
aws --no-verify-ssl --endpoint-url http://192.168.91.136:9000 s3 cp backup.sql s3://quay/datastorage/registry/
```
![image](https://github.com/user-attachments/assets/5ac00afe-7cb7-4b33-9f46-a161e990eec9)

![image](https://github.com/user-attachments/assets/d4179bcc-5642-4095-aa3d-b973c24417c7)


12. 





