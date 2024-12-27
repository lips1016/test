1. copy

 #copy one file
```
aws --no-verify-ssl --endpoint-url http://192.168.91.136:9000 s3 cp config.yaml s3://quay/datastorage/registry/

```
 ##aws + ssl verify condition + --endpoint-url  + s3 stroage URL:port s3 cp 'target file' s3://<bucket_name>/path/

![image](https://github.com/user-attachments/assets/cfed4b76-34d3-4258-91a0-27e394db8b77)


 #copy whole folder
```
aws --no-verify-ssl --endpoint-url http://192.168.91.136:9000 s3 cp --recursive ./blobs/ s3://quay/datastorage/registry/
```
##aws + ssl verify condition + --endpoint-url  + s3 stroage URL:port s3 cp --recursive 'target file' s3://<bucket_name>/path/
  

2. delete

```
aws --no-verify-ssl --endpoint-url http://192.168.91.136:9000 s3 rm s3://quay/datastorage/registry/config.yaml

```
 ##aws + ssl verify condition + --endpoint-url + s3 stroage URL:port s3 rm s3://<bucket_name>/path/'target file'

![image](https://github.com/user-attachments/assets/73fe8ed0-eb5f-4dd2-b66f-5b6a648ef314)


3. ls

```
aws --no-verify-ssl --endpoint-url http://192.168.91.136:9000 s3 ls
```
 ##aws + ssl verify condition + --endpoint-url + s3 stroage URL:port s3 ls

![image](https://github.com/user-attachments/assets/4507d07c-35ee-401e-83e9-68fc70de4fb2)


