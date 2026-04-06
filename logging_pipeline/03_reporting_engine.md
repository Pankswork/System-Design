### Rationale
A daily report requires event-driven compute. An always-on container (Fargate) would waste budget, so a scheduled Lambda function is the correct choice. From a DevSecOps perspective, the Lambda's IAM role must be strictly scoped to prevent lateral movement if compromised. Furthermore, we must ensure the Python code is secure and tested before deployment by injecting SAST and LocalStack integration tests into the prompt instructions.

### Prompt
Act as a Senior Python Developer. Write a Python Lambda function (triggered daily via EventBridge) that executes the Athena query using `boto3`. 

Constraint 1 (SecOps/Observability): Define an IAM execution role utilizing strict least privilege (no `s3:*` wildcards). Configure an EventBridge DLQ for the Lambda function to capture failed invocations after 3 retries.
Constraint 2 (Logic): The script must wait for the Athena execution, parse the data, generate a PDF, HTML, and JSON file, and save them into a completely Private S3 Bucket. 
Constraint 3 (QA Automation): Generate a GitHub Actions workflow that runs SAST (`bandit`) on the Python code AND sets up a LocalStack container environment to run automated integration tests before deployment.