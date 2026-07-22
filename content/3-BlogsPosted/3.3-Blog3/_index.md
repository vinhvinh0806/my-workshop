---
title: "Optimizing Costs and Simplifying Operations with Writable Warm Storage in Amazon OpenSearch Service"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your report, including this warning.
{{% /notice %}}

While exploring articles on the **AWS Big Data Blog**, I came across the article **"Cut costs and simplify operations with writable warm storage in Amazon OpenSearch Service."** At first, I thought it was simply another feature update focused on improving storage performance in Amazon OpenSearch Service. However, after reading the article, I realized that AWS is addressing a much more practical challenge faced by organizations managing massive volumes of data: reducing infrastructure costs while still allowing historical data to be updated when necessary.

According to AWS, many workloads such as log analytics, application monitoring, and security event analysis store data ranging from terabytes to petabytes. As datasets continue to grow, keeping everything in the high-performance Hot tier becomes increasingly expensive. On the other hand, moving all historical data into lower-cost storage often limits the ability to modify existing records. Writable Warm Storage is designed to solve this problem by providing a more flexible storage architecture.

## Overview

The article introduces **Writable Warm Storage**, a new capability available in **Amazon OpenSearch Service 3.3** and later. Unlike the traditional UltraWarm tier, which only supports read operations, Writable Warm Storage allows both read and write operations directly within the Warm tier.

Instead of moving an index back to the Hot tier whenever historical records need to be modified, administrators can update data directly inside the Warm tier. According to AWS, this significantly reduces processing time, simplifies operations, and lowers overall infrastructure costs.

From my perspective, this enhancement is more than just another OpenSearch feature. It represents a different approach to managing historical data more efficiently in large-scale analytics environments.

---

## Challenges of Traditional Tiered Storage

Amazon OpenSearch Service organizes data using a tiered storage architecture that balances performance and cost. According to the article, data is typically divided into three storage tiers:

- **Hot** – High-performance storage designed for indexing and real-time search using instance-attached storage.
- **UltraWarm** – A lower-cost storage tier backed by Amazon S3 with local caching for infrequently accessed data.
- **Cold** – The lowest-cost storage tier for archived data that must be restored before it can be queried or modified.

This architecture works well for immutable log data. However, problems arise whenever historical records need to be updated because of late-arriving events or compliance requirements. Since UltraWarm is read-only, even a small update requires moving the entire index back to the Hot tier before writing the changes and moving it back again afterward.

According to AWS, this process involves force merges, snapshot creation, and shard relocation. Updating a single 100 GB index can take approximately **130 minutes** while consuming considerable CPU, memory, and storage resources on the Hot nodes.

In my opinion, this limitation forces many organizations to keep data in the Hot tier much longer than necessary or provision additional infrastructure simply to support occasional updates. As a result, operational costs increase while storage management becomes more complicated.

---

## How Writable Warm Storage Works

To overcome the limitations of UltraWarm, AWS introduced **Writable Warm Storage** based on **OpenSearch Optimized (OI2)** instances.

According to the article, both the Hot tier and the new Writable Warm tier use Amazon S3 as their durable storage layer. Because both tiers share the same storage foundation, moving data between them only requires lightweight segment relocation instead of expensive data migration.

In addition, the Lucene search engine behaves identically across both storage tiers. Writable Warm therefore supports active writes, background merges, and periodic refreshes just like the Hot tier. This allows late-arriving records and historical updates to be written directly into the Warm tier within seconds without moving indexes back to Hot.

**Figure 1. Comparing UltraWarm and Writable Warm Data Flow**

![Comparison between UltraWarm and Writable Warm](/images/writable-warm-storage.png)

*Figure 1. Writable Warm Storage allows data to be written directly into the Warm tier, eliminating the traditional Hot → Warm → Hot migration process and simplifying storage operations.*
---

## Benefits for Cost, Operations, and Flexibility

According to AWS, Writable Warm Storage provides more than just the ability to update historical data. It also delivers significant improvements in infrastructure cost optimization, operational simplicity, and deployment flexibility. These advantages make it an attractive option for organizations managing large-scale OpenSearch clusters.

### Infrastructure Cost Optimization

One of the most interesting improvements mentioned in the article is support for **Reserved Instances (RI)** on **OpenSearch Optimized (OI2)** instances. Previously, UltraWarm only supported On-Demand pricing, whereas Writable Warm enables organizations to commit to longer-term usage in exchange for lower costs.

According to AWS, customers can reduce costs by approximately **36%** with a one-year Reserved Instance and up to **48%** with a three-year Reserved Instance. Furthermore, because historical data can move into the Warm tier much earlier instead of remaining in the expensive Hot tier while waiting for potential updates, overall storage costs are also significantly reduced.

In my opinion, this improvement is particularly valuable for organizations that retain large amounts of log or analytics data for extended periods, where storage expenses represent a considerable portion of the total infrastructure budget.

### Simplified Operations

Another important benefit highlighted by AWS is the reduction in operational complexity.

Previously, updating historical data required several maintenance tasks, including force merges, snapshot creation, and index migration between storage tiers. These operations consumed computing resources and often needed to be carefully scheduled to minimize production impact.

With Writable Warm Storage, these complex procedures are replaced by lightweight segment relocation and direct write capability within the Warm tier. This reduces the workload on Hot nodes, simplifies **Index State Management (ISM)** policies, and eliminates many operational steps that administrators previously had to perform manually.

From my perspective, simplifying these management tasks not only saves time but also reduces the likelihood of operational errors during maintenance.

### Greater Deployment Flexibility

According to AWS, Writable Warm Storage also provides more deployment options than UltraWarm.

While UltraWarm supports only two instance types, Writable Warm supports multiple **OI2** instance sizes ranging from **oi2.large** to **oi2.16xlarge**. This allows organizations to choose configurations that better match their workloads, storage capacity, and performance requirements.

I believe this flexibility becomes increasingly important as systems continue to grow because organizations can scale their infrastructure without redesigning the overall storage architecture.

---

## Search Performance and Recommended Use Cases

Besides reducing operational costs, AWS also evaluated the search performance of Writable Warm Storage using the **NYC Taxi** benchmark dataset.

The benchmark results show that Writable Warm delivers performance comparable to or better than UltraWarm in **six out of seven** tested query types. Filtering, range queries, sorting, and time-based aggregations all performed efficiently, demonstrating that adding write capability does not compromise search performance.

AWS also provides guidance on when each storage option should be used.

Writable Warm Storage is recommended for workloads that frequently update historical data, want to leverage Reserved Instances for cost savings, or require a wider selection of instance sizes. UltraWarm, on the other hand, remains suitable for immutable datasets or environments that rely heavily on Cold Storage for long-term archival.

After reading this section, I realized that choosing between Writable Warm and UltraWarm is not about selecting the better technology. Instead, the decision should depend on workload characteristics and operational requirements.

---

## Lessons Learned

After reading the article, I learned several important points:

- Tiered storage helps balance performance and cost when managing very large datasets.
- Writable Warm Storage removes the read-only limitation of UltraWarm by allowing direct writes in the Warm tier.
- Eliminating the Hot-to-Warm migration process significantly reduces update time and operational complexity.
- Reserved Instances on OI2 instances provide substantial infrastructure cost savings for long-term deployments.
- The choice between Writable Warm and UltraWarm should be based on workload requirements and data update frequency rather than performance alone.

---

## Conclusion

After studying this article, I understood that Writable Warm Storage is more than just a new feature in Amazon OpenSearch Service. It represents a significant improvement in the way historical data is managed. By allowing updates directly within the Warm tier, AWS reduces storage costs, simplifies day-to-day operations, and maintains strong search performance without requiring repeated index migrations.

In my opinion, this feature is especially beneficial for organizations operating large-scale log analytics, monitoring, or security platforms. It enables more efficient infrastructure utilization while making historical data updates much faster and easier to manage. For organizations using Amazon OpenSearch Service with continuously growing datasets, Writable Warm Storage is a valuable enhancement worth considering.

---

## References

- AWS Big Data Blog: *Cut costs and simplify operations with writable warm storage in Amazon OpenSearch Service*
- https://aws.amazon.com/blogs/big-data/cut-costs-and-simplify-operations-with-writable-warm-storage-in-amazon-opensearch-service/