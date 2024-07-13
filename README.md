# CI-CD-redash-on-eks

Deploy Redash on an EKS environment using GitHub CI/CD. This repository assumes you have already set up an AWS EKS environment and an RDS instance. Additionally, you need to set up a Redis server in the EKS cluster and configure a policy or role on the EKS cluster to access RDS database services. Once these prerequisites are in place, you can easily deploy Redash on your EKS environment.

For more detailed information, refer to the [Redash Setup Documentation](https://redash.io/help/open-source/setup/).

## Steps to Follow

1. **Clone this repo or create your own and copy the content:**
   Clone this repository or create your own fork and copy its contents.

2. **Configure AWS IAM credentials, region, and Slack hook URL in GitHub Action secrets:**
   Ensure you have set up AWS IAM credentials and Slack hook URL in your GitHub repository's secrets.

3. **Connect with your EKS cluster using `kubectl` utilities and update the context to the current cluster name:**

   ```bash
   aws eks --region <region> update-kubeconfig --name <cluster-name>
   ```
4. Create namespace 'redash':
  Create a namespace named 'redash' in your EKS cluster.

```
kubectl create namespace redash

```
   
3. Configure Redash Secrets: 
  In the redash-secret.yaml file, configure the following secrets in base64-encoded format:

```
  REDASH_COOKIE_SECRET: P2Zxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  REDASH_SECRET_KEY: bXkzxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  REDASH_REDIS_URL: cmVkaXM6xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  DATABASE_URL: cG9zdxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   
```
4. Apply Manifest Files:
  Apply the manifest files in the specified order to deploy Redash:
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
