
# About
This module covers the data setup for the and includes steps to be executed in the primary project.


## 1. Declare variables 

Declare variables in cloud shell, in the primary project's scope (in the author's case, its zeus-dataplex-p).

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

SPARK_GCE_NM=$BASE_PREFIX-gce
PERSISTENT_HISTORY_SERVER_NM=$BASE_PREFIX-sphs

SPARK_GCE_BUCKET=$SPARK_GCE_NM-$PROJECT_ID-s
SPARK_GCE_BUCKET_FQN=gs://$SPARK_GCE_BUCKET
SPARK_GCE_TEMP_BUCKET=$SPARK_GCE_NM-$PROJECT_ID-t
SPARK_GCE_TEMP_BUCKET_FQN=gs://$SPARK_GCE_TEMP_BUCKET
PERSISTENT_HISTORY_SERVER_BUCKET_FQN=gs://$PERSISTENT_HISTORY_SERVER_NM-$PROJECT_NBR

DATAPROC_METASTORE_SERVICE_NM=$BASE_PREFIX-dpms

VPC_PROJ_ID=$PROJECT_ID        
VPC_PROJ_ID=$PROJECT_NBR  

VPC_NM=$BASE_PREFIX-vpc
SPARK_GCE_SUBNET_NM=$SPARK_GCE_NM-snet
SPARK_CATCH_ALL_SUBNET_NM=$BASE_PREFIX-misc-snet

DATA_BUCKET=$BASE_PREFIX-$PROJECT_NBR-data
BQ_DATASET_NM=${BASE_PREFIX}_austin_bikeshare
HIVE_WAREHOUSE_BUCKET=$BASE_PREFIX-$PROJECT_NBR-hive-warehouse

DATA_BUCKET_SECONDARY=$BASE_PREFIX-$PROJECT_NBR-data-sec
```

## 2. Data setup for the lab

#### 2.a. Create a bucket for the data

```
gsutil mb -b on -l $LOCATION gs://${DATA_BUCKET}
```

#### 2.b. Copy the lab dataset into the data bucket

```
gsutil cp gs://dataplex-demo-sme/bikeshare_trips_parquet.parquet gs://${DATA_BUCKET}
```


#### 2.c. Create a BigQuery dataset 

```
bq --location=$LOCATION mk --dataset $BQ_DATASET_NM
```

#### 2.d. Load the bikeshare dataset into BigQuery

```
bq load --source_format=PARQUET $BQ_DATASET_NM.bikeshare_trips_bq gs://${DATA_BUCKET}/bikeshare_trips_parquet.parquet
```

#### 2.e. Create a bucket for the Hive warehouse

```
gsutil mb -b on -l $LOCATION gs://${HIVE_WAREHOUSE_BUCKET}
```

#### 2.f. Copy data into the Hive warehouse

```
gsutil -m cp -r gs://dataplex-demo-sme/hive-warehouse/ gs://${HIVE_WAREHOUSE_BUCKET}
```
