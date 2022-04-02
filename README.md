# docker-airflow-spark
Docker with Airflow + Postgres + Spark cluster + JDK (spark-submit support) + Jupyter Notebooks

## 📦 The Containers

* **airflow-webserver**: Airflow webserver and scheduler, with spark-submit support.
    * image: docker-airflow2:latest (custom, Airflow version 2.2.4)
      * Based on python:3.7-stretch, [puckel/docker-airflow](https://github.com/puckel/docker-airflow) and [cordon-thiago/airflow-spark](https://github.com/cordon-thiago/airflow-spark/)
    * port: 8080

* **postgres**: Postgres database, used by Airflow.
    * image: postgres:13.6
    * port: 5432

* **spark-master**: Spark Master.
    * image: bitnami/spark:3.2.1
    * port: 8081

* **spark-worker[-N]**: Spark workers (default number: 1). Modify docker-compose.yml file to add more.
    * image: bitnami/spark:3.2.1

* **jupyter-spark**: Jupyter notebook with pyspark support.
    * image: jupyter/pyspark-notebook:spark-3.2.1
    * port: 8888

## 🛠 Setup

### Clone project

    $ git clone https://github.com/pyjaime/docker-airflow-spark

### Build airflow Docker

    $ cd docker-airflow-spark/airflow/
    $ docker build --rm -t docker-airflow2:latest .
  
### Setup the sandbox
The sandbox will contain the folders where data will be persisted from the containers, and some test files.
We will create the folder easily:
  
    $ cd docker-airflow-spark/
    $ cp -R sandbox-test/. ../sandbox/

### Launch containers

    $ cd docker-airflow-spark/
    $ docker-compose -f docker-compose.yml up -d

### Check accesses

* Airflow: http://localhost:8080
* Spark Master: http://localhost:8081
* Jupyter Notebook: http://localhost:8888 (follow the instructions to get a token)
  
## 👣 Additional steps
  
### Create a test user for Airflow
  
    $ docker-compose run airflow-webserver airflow users create --role Admin --username admin \
      --email admin --firstname admin --lastname admin --password admin

### Edit connection from Airflow to Spark

* Go to Airflow UI > Admin > Edit connections
* Edit spark_default entry:
  * Connection Type: Spark
  * Host: spark://spark
  * Port: 7077 

### Test spark-submit from Airflow
  
Go to the Airflow UI and run the test_spark_submit_operator DAG :)

## 🎉 A big thank you

THANK YOU [Thiago Cordon](https://github.com/cordon-thiago) 
