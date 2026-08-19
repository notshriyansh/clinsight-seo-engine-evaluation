# Integration Options

## Estimated Implementation Effort

These are rough engineering estimates for an MVP, assuming one developer who is already familiar with the Clinsight codebase.

| Approach                                     |        MVP Estimate | Main Work                                      |
| -------------------------------------------- | ------------------: | ---------------------------------------------- |
| Direct DataForSEO integration                |            3–5 days | Adapter, API client, caching, error handling   |
| Add basic rank tracking                      | 4–7 additional days | Scheduling, persistence, polling, snapshots    |
| Self-host OpenSEO for evaluation             |              <1 day | Docker setup + DataForSEO credentials          |
| Integrate self-hosted OpenSEO into Clinsight |            2–4 days | Service boundary, auth, data exchange          |
| Production-ready OpenSEO integration         |           1–2 weeks | Auth, monitoring, failure handling, deployment |
| Fork and deeply customize OpenSEO            |            2+ weeks | Codebase integration + ownership/maintenance   |

These numbers are estimates rather than fixed commitments. The actual effort depends on the existing Clinsight backend, authentication model, database, job system, and the exact SEO workflows required.

There are three realistic approaches:

## Option A — Integrate DataForSEO Directly

```text
Clinsight Feature
       |
       v
   SEO Adapter
       |
       v
   DataForSEO
```

**Advantages**
Small provider surface
Clinsight owns the product experience
No dependency on OpenSEO application code
Easier to customize
Easier to control caching and costs
Provider can be replaced later

**Disadvantages**

Clinsight must implement the application workflows
Rank tracking requires considerably more than an API call
Scheduling, polling, persistence and historical analytics must be owned internally
Best Use

This should be the default architecture for new SEO capabilities.

## Option B — Integrate/Self-Host OpenSEO

```text
Clinsight
    |
    v
OpenSEO
    |
    v
DataForSEO
```

**Advantages**
Faster access to existing SEO workflows
Existing application logic can reduce implementation effort
Useful for capabilities where Clinsight does not yet have a mature implementation

**Disadvantages**

Additional application dependency
Deployment and authentication responsibility
Upgrade and maintenance burden
Integration boundaries can become harder to control
DataForSEO costs still remain underneath the system
Best Use

Use selectively when an OpenSEO workflow provides enough functionality that rebuilding it internally is not justified.

## Option C — Fork OpenSEO

A fork gives maximum control over the code but also creates ownership of the fork.

```text
Clinsight
    |
    v
Clinsight-maintained OpenSEO fork
    |
    v
DataForSEO
```

**Advantages**
Maximum customization
Full control over deployment
Ability to remove unnecessary functionality

**Disadvantages**

Long-term merge and upgrade burden
Security maintenance becomes Clinsight's responsibility
More code to understand and operate
Risk of coupling Clinsight's architecture to an external codebase

**Recommendation**

Do not fork initially.

A fork should only be considered after identifying a specific OpenSEO workflow that:

- is strategically important,
- would be expensive to reproduce,
- cannot be consumed cleanly through an API/service boundary, and
- justifies permanent ownership.

**Recommended Integration Strategy**

Start with the smallest useful provider boundary:

```text
interface SEOProvider {
  searchSERP(input: SERPRequest): Promise<SERPResult>;
  getKeywords(input: KeywordRequest): Promise<KeywordResult>;
  getBacklinks(input: BacklinkRequest): Promise<BacklinkResult>;
  getDomainData(input: DomainRequest): Promise<DomainResult>;
  getCompetitors(input: CompetitorRequest): Promise<CompetitorResult>;
}
```

Then put all provider-specific code behind this interface.

This prevents product features from depending directly on DataForSEO request formats.
