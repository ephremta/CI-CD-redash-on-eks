# CI-CD-redash-on-eks
Deploy Redash on an EKS environment using GitHub CI/CD
This repo assumes you have already setup aws eks environment and RDS instance. In addtion , you need to setup redis server in EKS cluster. You need to add also a polciy or role on EKS cluster to access RDS intance database services. Then you can easily setup redash on EKS environment.  


## Steps to follow 

1. **Clone this repo or create your own and copy the content:**
2. ** Configure the aws I am credentials, region and slack hook-url in github action secrets :**
3. ** Connect with your eks cluster using kubectl utilities and update the context to the current cluster name :**
3. ** Create namesapce 'redash' :**
3. **In the redash-secret.yaml file configure the REDASH_REDIS_URL, REDASH_COOKIE_SECRET, REDASH_SECRET, DATABASE_URL  encoded in base 64 format **
you can do like this:

```
  REDASH_COOKIE_SECRET: P2Zxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  REDASH_SECRET_KEY: bXkzxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  REDASH_REDIS_URL: cmVkaXM6xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  DATABASE_URL: cG9zdxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   
```
---