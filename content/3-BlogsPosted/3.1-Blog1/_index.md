---
title: "Blog 1"
date: 2026-07-22
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# CUT COSTS AND SIMPLIFY OPERATIONS WITH WRITABLE WARM STORAGE IN AMAZON OPENSEARCH SERVICE

When managing large-scale search and analytics workloads (such as Log Management, SIEM, and Monitoring) on AWS, handling petabytes of data presents a constant challenge between cost and performance. Storing all data in high-performance storage leads to unsustainable costs, while moving data to cheaper storage tiers often restricts essential write capabilities, complicating day-to-day operations.

Although Amazon OpenSearch Service provides a tiered storage architecture, traditional warm storage limitations have historically created operational bottlenecks. The introduction of **Writable Warm Storage** effectively resolves this challenge.

Key points to know:

* **Eliminates costly round-trip data migration:** Allows direct writes and updates for late-arriving data or compliance modifications directly within the Warm tier, removing the need to migrate data back to the Hot tier.
* **Full feature parity with Hot storage:** Supports active writing, background segment merging, and periodic refreshes powered by the unified Lucene engine.
* **Reduces costs by up to 48%:** Shortens mandatory retention periods in the Hot tier, frees up 35% of Hot disk capacity previously reserved for snapshot/merge operations, and supports Reserved Instances (RI) pricing on OpenSearch Optimized (OI2) instances.
* **Simplifies operational management:** Offers flexible instance sizing ranging from `oi2.large` to `oi2.16xlarge`, while removing the legacy UltraWarm restriction of 10 concurrent migration queues.
* **Delivers high query performance:** Achieves query latency comparable to or faster than UltraWarm across 6 out of 7 benchmark test scenarios (including filtering, sorting, and time-series aggregations).

---

### 1. Bottlenecks of Traditional Tiered Storage

OpenSearch Service typically manages data across three tiers:
1. **Hot Tier:** Highest performance using attached SSDs for real-time indexing and search. Most expensive.
2. **UltraWarm Tier:** Cost-effective storage backed by Amazon S3 and local caching, optimized for infrequently accessed data.
3. **Cold Tier:** Fully detached storage providing extremely low-cost retention for rarely accessed data.

While this model works seamlessly for immutable log data, issues arise when handling late-arriving data or compliance updates. Because the traditional UltraWarm tier is **read-only**, updating even a single historical record requires an expensive round-trip process:

**Warm Tier** &rarr; **Migrate back to Hot** &rarr; **Update / Write Data** &rarr; **Migrate to Warm**

This workflow requires force-merges, snapshots, and segment relocations. Re-indexing a 100 GB index can take up to **130 minutes** while heavily consuming Hot node CPU and RAM resources, forcing teams to over-provision capacity or retain data in the Hot tier longer than necessary.

---

### 2. Breakthrough with Writable Warm Storage (OI2 Instances)

To eliminate these complex cycles, AWS introduced **Writable Warm Storage** powered by the **OpenSearch Optimized (OI2)** instance family. Because both the Hot tier and the new Writable Warm tier share Amazon S3-backed persistent storage, tier transitions become lightweight segment relocations rather than heavy data copy operations.

* **Direct Writes and Updates:** Late-arriving data is indexed directly into the Warm tier within seconds.
* **Uninterrupted Hot Tier Performance:** Eliminates forced merges and mandatory snapshots, freeing up to 35% of previously reserved Hot storage.
* **No-replica Option:** For warm data where brief recovery times are acceptable, replica copies can be disabled to further minimize S3 storage costs.

---

### Links & References

* 🔗 **Original AWS Blog Post:** [Cut costs and simplify operations with writable warm storage in Amazon OpenSearch Service](https://aws.amazon.com/blogs/big-data/cut-costs-and-simplify-operations-with-writable-warm-storage-in-amazon-opensearch-service/)
* 📚 **Documentation Guide:** [Amazon OpenSearch Service Developer Guide - Managing storage tiers](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/managedomains-opensearch-storage-tiers.html)