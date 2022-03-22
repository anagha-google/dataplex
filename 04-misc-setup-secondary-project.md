# About
This module should be executed in the **secondary project**. It involves creation of a bucket and copying data into it.


## 1. Declare variables

Repasting the entire set of variables for simplicity-
```
#The base_prefix below, is any keyword that you can prefix to your resources for traceability and for uniqueness
#for those services that require globally unique names. The author has used "zeus" for base prefix

BASE_PREFIX="zeus"  

#Replace with your details
ORG_ID=akhanolkar.altostrat.com                              
ORG_ID_NBR=236589261571
ADMINISTRATOR_UPN_FQN=admin@$ORG_ID 
PROJECT_ID=zeus-dataplex-p
PROJECT_NBR=902769668277
SECONDARY_PROJECT_ID=zeus-dataplex-s
SECONDARY_PROJECT_NBR=159402989543

#Your public IP address, to add to the firewall
YOUR_CIDR=98.222.97.10/32

#General variables
LOCATION=us-central1
ZONE=us-central1-a

UMSA="$BASE_PREFIX-sa"
UMSA_FQN=$UMSA@$PROJECT_ID.iam.gserviceaccount.com

DATA_BUCKET_SECONDARY=$BASE_PREFIX-$SECONDARY_PROJECT_NBR-data-sec
```

## 2. Data setup for the lab

### 2a. Create a bucket for the data
```
gsutil mb -b on -l $LOCATION gs://${DATA_BUCKET_SECONDARY}
```

### 2b. Copy a CSV dataset into the bucket

```
gsutil cp gs://dataplex-demo-sme/bikeshare_trips_csv.csv gs://${DATA_BUCKET_SECONDARY}

```
