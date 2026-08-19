# Cost Analysis

## The Cost Model

The important cost is not simply the price of one API request.

For Clinsight, the relevant equation is closer to:

```text
Total SEO Cost
=
Provider Usage
+
Infrastructure
+
Storage
+
Engineering/Maintenance
+
Operational Overhead
```

The provider component is driven by:

- Number of keywords
- Number of devices
- Tracking frequency
- SERP depth
- Standard vs. priority vs. live execution
- Other paid SEO operations

**Example: Moderate Rank Tracking**

Assume:

- 100 keywords
- 2 devices
- Weekly tracking
- 10-result SERPs

```text
100 × 2 × 52
= 10,400 SERP checks/year
```

**Example: Larger Workload**

Assume:

- 1,000 keywords
- 2 devices
- Daily tracking
- 100-result depth

```text
1,000 × 2 × 365
= 730,000 keyword/device checks

100 results ≈ 10 SERP pages

730,000 × 10
= 7.3M SERP pages/year
```

At this point, execution mode, depth, caching, batching and workload design become important cost controls.

## Cost Optimization

### 1. Use the Cheapest Suitable Execution Mode

Scheduled jobs do not necessarily need live execution.

### 2. Batch Work:

Avoid sending individual requests sequentially when the provider supports batching.

### 3. Cache Stable Data:

Suitable candidates include:

- Keyword research
- Keyword ideas
- Competitor discovery
- Content research
- Some domain information

Do not blindly cache time-sensitive rank snapshots.

**4. Add Cost Ceilings**

**5. Track Provider Spend Internally**

Every provider call should produce enough metadata to answer:

- Which customer generated the cost?
- Which feature caused it?
- Which provider endpoint was used?
- Was the result cached?
- How much did it cost?

This allows Clinsight to price and optimize its own product independently of provider pricing.

## Main Cost Conclusion

- For moderate workloads, raw SERP API costs can be small compared with engineering and infrastructure costs.

- At larger scale, however, inefficient tracking frequency, SERP depth and execution mode can multiply provider spend quickly.

- Therefore the best optimization is not simply choosing the cheapest provider. It is controlling when fresh data is actually necessary.
