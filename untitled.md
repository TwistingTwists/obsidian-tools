

When breaking down **impact** in incident documentation (or any technical writing), granularity ensures clarity, prioritization, and actionable next steps. For a support engineer audience, this helps them triage issues, communicate with stakeholders, and anticipate follow-up questions. Here’s how to structure impact with granularity, using an infrastructure incident as an example:

---

### **1. Break Impact into Categories**  
Organize impact into logical buckets to avoid ambiguity. For example:  

| **Category**          | **Example**                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| **Scope**             | EU region, Payment service APIs, Users on iOS v15+                          |
| **Severity**          | 40% API error rate, 100% downtime for 30 minutes                            |
| **User Impact**       | Failed checkout flows, Delayed order confirmations                         |
| **Business Impact**   | SLA violation (99.9% → 95%), $X lost revenue                                |
| **Long-Term Risks**   | Customer trust erosion, Compliance audit flags                              |

**Why this works:**  
- Separates technical metrics (severity) from user/business outcomes.  
- Helps support engineers prioritize communication (e.g., high-severity issues first).  

---

### **2. Quantify Where Possible**  
Use **numbers**, **timeframes**, and **percentages** instead of vague terms.  

**Bad Example:**  
> *“Some users had trouble logging in.”*  

**Good Example:**  
> **User Impact:**  
> - 25% of authentication requests failed (2,000 users affected).  
> - Downtime: 10:00–10:45 UTC (45 minutes).  
> - Peak failure rate: 500 errors/minute at 10:15 UTC.  

**Why this works:**  
- Specific metrics let support engineers correlate user reports with incident timelines.  
- Quantified data helps prioritize post-incident actions (e.g., compensating affected users).  

---

### **3. Map Impact to Systems/Components**  
Link the impact to specific infrastructure components, services, or dependencies.  

**Bad Example:**  
> *“The database was slow.”*  

**Good Example:**  
> **System Impact:**  
> - **MySQL Cluster (eu-primary):** Connection pool exhausted (200 → 500 concurrent connections).  
> - **Payment Service:** API latency spiked to 15s (95th percentile).  
> - **Redis Cache:** Cache-hit ratio dropped from 80% → 30%, increasing DB load.  

**Why this works:**  
- Identifies *exactly* where the support engineer should look for residual issues.  
- Helps them route customer complaints (e.g., “Payment delays? Check Redis cache metrics”).  

---

### **4. Highlight User-Facing Symptoms**  
Describe how the impact manifested for end-users, not just systems.  

**Bad Example:**  
> *“Users experienced errors.”*  

**Good Example:**  
> **User-Facing Symptoms:**  
> - Checkout page: “Payment failed” errors (HTTP 500).  
> - Mobile app: Sessions expired prematurely (1h → 10m).  
> - Email notifications delayed by ~30 minutes.  

**Why this works:**  
- Support engineers can match user reports to known issues.  
- Provides clear language for customer communications (e.g., “You may have seen payment errors between 10:00–10:45 UTC”).  

---

### **5. Include Secondary/Cascading Effects**  
Document downstream or indirect impacts that aren’t immediately obvious.  

**Bad Example:**  
> *“The API was down.”*  

**Good Example:**  
> **Cascading Impact:**  
> - API failures → queued requests overloaded message brokers (Kafka lag: 1.2M messages).  
> - Monitoring alerts silenced due to alert-storm throttling.  
> - Post-resolution: CDN cached error pages for 10 minutes (required manual purge).  

**Why this works:**  
- Prepares support engineers for “ripple effect” issues (e.g., backlogged queues).  
- Explains why some systems might still behave oddly post-resolution.  

---

### **Key Takeaways**  
1. **Categorize impact** (scope, severity, user, business).  
2. **Quantify** with numbers, timeframes, and percentages.  
3. **Link to systems** (e.g., MySQL, Redis) and **user symptoms** (e.g., “Payment failed”).  
4. **Anticipate cascading effects** (e.g., message broker lag).  
5. **Avoid vague terms** like “some,” “major,” or “quickly.”  

By breaking down impact with this granularity, you empower support engineers to:  
- Triage user reports faster.  
- Communicate confidently with stakeholders.  
- Debug residual or related issues.