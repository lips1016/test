1. create dockerfile

```
vi dockerfile
```

2. build up the docker image 

```
docker build -t test_build:v1 .
```

##the "." after the build version is import and which is needed
##if don't type in the version, default version is latest
![image](https://github.com/user-attachments/assets/51519d96-860d-4e99-8f14-692de6c9e154)
![image](https://github.com/user-attachments/assets/76eb744b-419c-4124-aaa7-8ba5278dfe31)

after build the image, use the commannd to check the images

```
docker images
```

![image](https://github.com/user-attachments/assets/278284dd-993a-4a9a-9e18-6cf78292585a)
