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
4. **Actual scredit  scoring **


## To run the API inside the cluster 
```
kubectl port-forward services/deapiv21  -n decisionengineapi 30517:89
navigate to the port-forwarded url /(http://127.0.0.1:5001/)
This API has alos an external IP found at: **http://'{}'**
```

# Test input data 
This API has TwO endpoints:
1. score - this endpoint allow the consumer to get the actual decision engine response including credit score, loan assignment variables , screening and overwrting responses
2. configure - this endpoint enables end users to configure decision engine specific configuration varibales and rules such as screening rules, overwritting rules, parameter ranages and score weights based on financial institute needs and business decisions. 

In order to test the first endpoint(evaluation), we can use the input json below.
```
{
    "api_request_data": {
        "client_name": "abebe",
        "source": "coop",
        "loan_id": "9302nd",
        "borrower_id": "6263a6d4a78b7e3e4e6adec5",
        "loan_type": "MSME",
        "loan_subtype": "small"
    },


}






