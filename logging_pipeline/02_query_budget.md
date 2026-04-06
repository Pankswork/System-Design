### Rationale
Using Amazon Athena allows us to query S3 directly without paying for a standing database. However, without FinOps guardrails, a poorly written query could scan the entire S3 bucket and destroy the $800 budget. We must enforce strict data-scan limits via Athena Workgroups and deploy AWS Cost Anomaly Detection as code to catch billing spikes before the monthly invoice arrives.

### Prompt
Write the AWS Glue and Athena configuration, along with proactive operational billing alerts in Terraform.

Constraint 1 (FinOps): Write the Terraform for an AWS Budget hard-capped at $800/month, and enable AWS Cost Anomaly Detection to trigger an SNS alert to the engineering team if the daily spend exceeds $26.
Constraint 2 (SecOps/Cost): Create a dedicated Athena Workgroup enforcing query encryption and a strict 10GB data-scan limit per query.
Constraint 3 (Logic): Write the optimized SQL query pointing to the Parquet S3 bucket to calculate the highest resolutions and counts, explicitly limiting the `WHERE` clause to scan only the last 24-hour partition.