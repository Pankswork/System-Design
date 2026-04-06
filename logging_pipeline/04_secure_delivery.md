### Rationale
The prompt requires the report links to be accessible for "months or years." Standard S3 pre-signed URLs expire after 7 days, and making an S3 bucket public is a critical security violation. The secure architectural solution is to keep the bucket private and place an API Gateway + Lambda proxy in front of it. The API Gateway can be secured with a WAF, and the proxy safely retrieves the files on demand.

### Prompt
To securely distribute these reports with permanent links without exposing the S3 bucket, write the Terraform for an API Gateway and Lambda proxy.

Constraint 1 (SecOps): The API Gateway must be protected by an AWS WAF rate limiter and only accept TLS 1.2+ connections.
Constraint 2 (Logic): The Lambda proxy will accept a unique report ID from the URL, securely retrieve the requested file from the Private S3 bucket, and return it.
Constraint 3 (Delivery): Write a Python function using Amazon SES to email these secure URLs to the subscribers. Configure DKIM/SPF settings in Terraform to prevent email spoofing.