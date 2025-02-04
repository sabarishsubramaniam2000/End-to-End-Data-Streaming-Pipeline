# End-to-End Data Streaming Pipeline using Apache Spark and Kafka

## **Overview**
This project is a fully functional end-to-end data streaming pipeline that ingests, processes, and stores real-time user data using a modern data stack. It encompasses the entire pipeline, from data ingestion to processing and storage, leveraging a powerful technology stack that includes **Apache Airflow, Python, Apache Kafka, Apache Zookeeper, Apache Spark, and Cassandra**. The entire system is containerized with Docker to ensure seamless deployment and scalability.

## **Pipeline Components**
1. **Data Ingestion (Apache Airflow)**
   - Fetches user data from `randomuser.me API`.
   - Stores raw data in **PostgreSQL**.
   - Sends structured data to **Kafka** for streaming.

2. **Data Streaming (Apache Kafka & Zookeeper)**
   - Kafka queues incoming user data.
   - Zookeeper manages Kafka brokers.
   - Kafka topic `users_created` stores incoming records.

3. **Data Processing (Apache Spark)**
   - Spark **reads real-time data from Kafka**.
   - Processes and structures the data.
   - Writes processed data to **Apache Cassandra**.

4. **Data Storage**
   - **PostgreSQL**: Stores raw user data before streaming.
   - **Cassandra**: Stores processed user records.

5. **Orchestration (Apache Airflow)**
   - Manages workflow execution.
   - Automates periodic data ingestion.

6. **Containerization (Docker)**
   - **Ensures smooth deployment** across different environments.

---

## **Technologies Used**
- **Apache Airflow** – Workflow automation & scheduling
- **Python** – Data extraction & processing
- **Apache Kafka** – Real-time data streaming
- **Apache Zookeeper** – Kafka broker management
- **Apache Spark** – Real-time data processing
- **Apache Cassandra** – Processed data storage
- **PostgreSQL** – Raw data storage
- **Docker** – Containerization for deployment

---

## **Setup Instructions**

### **Prerequisites**
Ensure you have installed:
- **Docker & Docker Compose**
- **Python 3.x**
- **Apache Kafka & Zookeeper** (Handled inside Docker)
- **Apache Spark**
- **PostgreSQL**
- **Apache Cassandra**

---

### **Steps to Run the Pipeline**

#### **1. Clone the Repository**
```sh
git clone <repository-url>
cd <project-folder>
```

#### **2. Run Docker Compose**
```sh
docker-compose up
```
This starts **Airflow, Kafka, Zookeeper, Spark, PostgreSQL, and Cassandra**.

#### **3. Access Airflow & Trigger DAG**
- Open **Airflow UI** at `http://localhost:8080`
- Enable and trigger the DAG **user_automation**.
- This triggers the **stream_data_from_api** task.

#### **4. Monitor Kafka Streams**
Run the Kafka consumer from **inside the Kafka container**:
```sh
docker exec -it <kafka-container-id> kafka-console-consumer --bootstrap-server broker:29092 --topic users_created --from-beginning
```
Kafka runs on `broker:29092` inside Docker but is accessible on `localhost:9092` from the host.

#### **5. Check Data in PostgreSQL**
- Connect to PostgreSQL:
```sh
docker exec -it <postgres-container-id> psql -U <username> -d <database>
```
- View stored raw data:
```sql
SELECT * FROM raw_users;
```

#### **6. Check Processed Data in Cassandra**
- Connect to Cassandra:
```sh
docker exec -it <cassandra-container-id> cqlsh
```
- Query stored data:
```sql
SELECT * FROM spark_streams.created_users;
```

---

## **Industry Use Cases**
- **Fraud Detection**: Streaming transaction data to detect anomalies.
- **Real-time Analytics**: Processing user activity in web applications.
- **E-commerce & Recommendations**: Analyzing user behavior in real time.

This project showcases **modern real-time data engineering** for large-scale applications. 
