# CI-CD-redash-on-eks
Deploy Redash on an EKS environment using GitHub CI/CD.
This repo assumes you have already setup aws eks environment and RDS instance. In addtion , you need to setup redis server in EKS cluster. You need to add also a polciy or role on EKS cluster to access RDS intance database services. Then you can easily setup redash on EKS environment.  


## Steps to follow 

1. Clone this repo or create your own and copy the content:
2. Configure the aws I am credentials, region and slack hook-url in github action secrets :
3.Connect with your eks cluster using kubectl utilities and update the context to the current cluster name :
3. Create namesapce 'redash' :
3. In the redash-secret.yaml file configure the REDASH_REDIS_URL, REDASH_COOKIE_SECRET, REDASH_SECRET, DATABASE_URL  encoded in base 64 format 
you can do like this:

```
  REDASH_COOKIE_SECRET: P2Zxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  REDASH_SECRET_KEY: bXkzxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  REDASH_REDIS_URL: cmVkaXM6xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  DATABASE_URL: cG9zdxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   
```
4. Apply the manifest files in the order;
   ```
    kubectl apply -f setup-redash/deployment-setup/redash-serviceaccount.yaml -n redash
    kubectl apply -f setup-redash/deployment-setup/redash-configmap.yaml -n redash
    kubectl apply -f setup-redash/deployment-setup/redash-secret.yaml -n redash
    kubectl apply -f setup-redash/deployment-setup/redash-deployment.yaml -n redash
    kubectl apply -f setup-redash/deployment-setup/redash-service.yaml -n redash
  
  
   ```
5. Initialize Database and Apply Migrations:
To ensure all default Redash tables are created and migrations are applied, follow these steps:
```
# Login to Redash container/pod
kubectl exec -it <pod-name> -- /bin/bash -n redash

# Change to the app directory
cd /app

# Initialize the database schema
./manage.py database create_tables

# Apply Migrations; after initializing the tables, or if there are new migrations to apply:
./manage.py database upgrade
```
 
   

       


   ```
