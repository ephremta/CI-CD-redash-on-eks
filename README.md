# CI-CD for Redash on Amazon EKS

Deploy Redash on an Amazon EKS environment using GitHub CI/CD. This guide assumes you have already set up an AWS EKS cluster and an RDS instance. Additionally, you need to configure a Redis server within the EKS cluster and establish a policy or role allowing access to RDS database services from EKS.

For more detailed information, refer to the [Redash Setup Documentation](https://redash.io/help/open-source/setup/).

## Steps to Deploy Redash on EKS

#### 1. Clone the Repository

   Clone this repository or create your own fork and copy its contents.

#### 2. Configure GitHub Action Secrets

   Ensure AWS IAM credentials, region settings, and Slack webhook URL are configured in your GitHub repository's secrets.

#### 3. Connect to Your EKS Cluster

   Use `kubectl` to connect to your EKS cluster and update the kubeconfig to the current cluster name:
   
   ```bash
   aws eks --region <region> update-kubeconfig --name <cluster-name>
   ```
#### 4. Create namespace *redash*

Create a namespace named *redash* in your EKS cluster:


   ```
   kubectl create namespace redash
   
   ```
   
#### 5. Configure Redash Secrets: 
  In the redash-secret.yaml file, configure the following secrets in base64-encoded format:

   ```
     REDASH_COOKIE_SECRET: P2Zxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
     REDASH_SECRET_KEY: bXkzxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
     REDASH_REDIS_URL: cmVkaXM6xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
     DATABASE_URL: cG9zdxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
      
   ```
#### 6. Apply Manifest Files:
  Apply the manifest files in the specified order to deploy Redash:
   ```
    kubectl apply -f setup-redash/deployment-setup/redash-serviceaccount.yaml -n redash
    kubectl apply -f setup-redash/deployment-setup/redash-configmap.yaml -n redash
    kubectl apply -f setup-redash/deployment-setup/redash-secret.yaml -n redash
    kubectl apply -f setup-redash/deployment-setup/redash-deployment.yaml -n redash
    kubectl apply -f setup-redash/deployment-setup/redash-service.yaml -n redash
  
  
   ```
#### 7. Initialize Database and Apply Migrations:
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
