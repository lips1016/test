```
vi /etc/docker/daemon.json  ##add the config make the url through the udp to process
```

```
{
  "insecure-registries" : ["quay.apps-crc.testing:<port>"]  ##target hostname with port/ quay.apps-crc.testing:443 -> default use 443 port

}
```

sudo systemctl restart docker




## if cannot run successfully use below method

1. open CRC portal and go to openshift-ingress project

2. open openshift-ingress porject secert tab and open route-cert-default

![image](https://github.com/user-attachments/assets/f9020ea1-ced5-45a2-83b0-841df499c181)

3. copy the cert info

4. create the cert and using the cert to curl the target host
```
vi ca.crt
```
curl --cacert ca.crt https://quay.apps-crc.testing

5. add the cert to the server
```
vi /etc/ssl/certs/ca-certificates.crt
```

curl  https://quay.apps-crc.testing ##curl the host after add the cert

5.5 remove the docker insecure registry setting if before setting is not successfully.

6. restart docker service and minio service to renew the setting
7. docker login for docker push file to minio/quay

![image](https://github.com/user-attachments/assets/e124f68b-b84e-4577-ada7-13fac6ae7699)

8. docker tag and docker build
```
docker tag hello-world:latest quay.apps-crc.testing/admin/test:v1
```

9.  push successfully

 ![image](https://github.com/user-attachments/assets/766f1ef9-d6dc-4944-ae1b-0bdc605c48d7)

10.  minio/quay can show the image has been pushed

![image](https://github.com/user-attachments/assets/52fce2f0-911d-47cb-8792-f485cb491e39)



    


