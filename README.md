# VPC Flow Logs Cost Estimation
VPC Flow Logs cost estimation depends on **destination**, **log ingestion volume**, and **storage duration**. Costs are typically calculated per gigabyte (GB) of log data ingested and stored, with volume discounts applied at higher tiers. Here’s a breakdown based on AWS 2025 pricing:

***

**VPC Flow Logs to CloudWatch Logs:**
- Ingestion:
  - First 10TB/month: $0.50/GB
  - Next 20TB/month: $0.25/GB
  - Next 20TB/month: $0.10/GB
  - Over 50TB/month: $0.05/GB
- Archival Storage (compressed): $0.03/GB

**Example:** 72TB logs/month to CloudWatch Logs =
- Total ingestion: $13,414.40
- Storage (30TB compressed): $921.60
- **Total monthly: $14,336**

***

**VPC Flow Logs to S3 (optional Apache Parquet):**
- Ingestion:
  - First 10TB/month: $0.25/GB
  - Next 20TB/month: $0.15/GB
  - Next 20TB/month: $0.075/GB
  - Over 50TB/month: $0.05/GB
- Optional Parquet conversion: $0.03/GB
- S3 Storage: $0.023/GB (standard storage, regional differences may apply)

**Example:** 72TB logs/month to S3 =
- Total ingestion: $8,294.40
- Optional Parquet conversion: $2,211.84
- Storage (6.5TB compressed): $153.01
- **Total monthly: $10,659.25**

***

**Cost Control Tips:**
- **Log volume** is determined by network activity, VPC size, and chosen log format (e.g., all, accept, reject).
- Tag flow logs for granular chargebacks and use AWS Cost Explorer to analyze and forecast costs.[3][4]
- Choose log destinations wisely: S3 is generally cheaper for very large data volumes; CloudWatch is better for search/analyze workflows.
- Retention periods and log formats impact storage costs.
- Use compression and efficient formats (like Apache Parquet) to reduce storage cost.

**Summary Table:**

| Destination   | Ingestion Cost (per GB)  | Storage Cost (per GB) | Notes                    |
|---------------|-------------------------|-----------------------|--------------------------|
| CloudWatch    | $0.50–$0.05 (by tier)   | $0.03 (compressed)    | Lower cost at volume; analytics support |
| S3            | $0.25–$0.05 (by tier)   | $0.023 (standard)     | S3 cost regional; Parquet adds cost    |

***

You can use the AWS Pricing Calculator to estimate your costs for custom data volumes and retention policies.

For precise estimates, track data volume in your environment and apply current AWS regional rates for the relevant services.

---
