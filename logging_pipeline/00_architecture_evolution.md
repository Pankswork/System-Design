# Architectural Evolution & Trade-off Analysis
**Project:** Highly Scalable DNS Log Processing Pipeline
**Constraint Focus:** $800/Month Hard Budget & 100k/sec Traffic Spikes

## 1. The Layered Engineering Approach
Designing a pipeline to handle extreme traffic spikes on a micro-budget requires moving beyond basic infrastructure. I approached this system design by layering specific engineering domains to guarantee resilience, security, and cost-efficiency.

* **Layer 1: Basic Flow (The Naive Approach)**
  * *Initial Thought:* Route logs directly to a Lambda function, store in S3, and query.
  * *Rejection:* Triggering Lambda 100,000 times per second during the spike would exhaust the $800 budget in hours. 
* **Layer 2: Senior SRE (Scalability & Buffer)**
  * *Upgrade:* Introduced **Amazon Kinesis Firehose** as the ingestion front-door. Firehose acts as a serverless shock-absorber, natively buffering the 100k/sec spike without requiring expensive always-on clusters like Apache Kafka (MSK).
* **Layer 3: FinOps (Cost Defenses)**
  * *Upgrade:* Configured Firehose to perform inline JSON-to-Parquet conversion. Parquet is a compressed columnar format, reducing Athena query scan sizes by up to 90%. Added S3 Lifecycle policies (Glacier Deep Archive) and hard-capped AWS Budgets.
* **Layer 4: DevSecOps (Zero Trust)**
  * *Upgrade:* Enforced S3 Object Lock (WORM compliance) to guarantee log immutability against tampering. Replaced public S3 URLs with an API Gateway + Lambda Proxy protected by an AWS WAF to secure report delivery.
* **Layer 5: DataOps & Observability (Day-2 Operations)**
  * *Upgrade:* Integrated the AWS Glue Schema Registry to handle future log format changes without breaking the pipeline. Implemented Dead Letter Queues (DLQs) for both Firehose and EventBridge to catch and store failed log conversions, preventing silent data loss.

---

## 2. Pros & Cons (Budget-Driven Trade-offs)

Operating under a strict $800 monthly limit required deliberate architectural compromises. I prioritized scalability and data retention over real-time processing.

### The Pros:
* **Infinite Scalability:** The serverless ingestion layer (Firehose) scales automatically to handle massive, instantaneous spikes without manual intervention or pre-provisioning.
* **Zero Idle Compute:** By avoiding EC2, Fargate, or RDS, the system costs approach $0.00 when traffic is at baseline, reserving budget exclusively for data processing.
* **Forensic Integrity:** The integration of S3 Object Lock ensures this pipeline meets stringent cybersecurity compliance standards for immutable audit trails.

### The Cons (Trade-offs for the $800 Budget):
* **High Latency (Not Real-Time):** Firehose buffers data for up to 60 seconds, and the reporting Lambda runs on a 24-hour batch schedule. If the SecOps team required a *real-time* anomaly detection dashboard, we would need Kinesis Data Streams feeding into an Amazon OpenSearch (Elasticsearch) cluster, which would immediately exceed the $800 budget.
* **Query Performance:** While Amazon Athena is highly cost-effective, it is a serverless query engine that scans files in S3. It will not return results in milliseconds like a dedicated, clustered relational database or data warehouse (e.g., Amazon Redshift) would. 

---

### Rationale
This file serves as the foundational design document (ADR) detailing the cross-domain engineering principles and financial trade-offs made to architect a production-ready system under strict constraints.

### Prompt
Act as a Principal Cloud Architect. Acknowledge receipt of this design document and prepare to receive the subsequent sequence of infrastructure prompts.