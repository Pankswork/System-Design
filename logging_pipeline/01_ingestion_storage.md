### Rationale
To handle the 100,000 logs/sec traffic spike within an $800/month budget, we cannot use always-on clusters like Apache Kafka (MSK). Amazon Kinesis Firehose is the perfect serverless shock-absorber. However, to prevent runaway storage and query costs, the data must be converted to Parquet format before hitting S3. For Day-2 operability and security, we must include a Dead Letter Queue (DLQ) for failed conversions and S3 Object Lock to guarantee log immutability for forensic integrity.

### Prompt
Act as a Principal AWS DevSecOps Engineer. Write the Terraform code for a Kinesis Firehose delivery stream and an S3 bucket to ingest 100,000 DNS logs/sec. 

Constraint 1 (DataOps/FinOps): Configure Firehose for inline JSON to Apache Parquet conversion using the AWS Glue Schema Registry to handle future schema evolution. Ensure source agents batch logs to bypass the 5KB billing penalty. 
Constraint 2 (Observability): Configure a Dead Letter Queue (DLQ) S3 bucket. Any logs that fail Parquet conversion must be routed here to prevent silent data loss.
Constraint 3 (SecOps/FinOps): Enable S3 Object Lock in compliance mode, enforce KMS encryption, and implement an S3 Lifecycle Rule to transition logs to Glacier Deep Archive after 90 days. Apply strict Cost Allocation Tags (`Project: DNS-Log-Pipeline`).