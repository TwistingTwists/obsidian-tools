---
tags:
  - interview
---

### Basics

- incident is a series of issues => timeline

### What do support engineers do?

- **Route** customer complaints 
	- (e.g., “Payment delays? Check Redis cache metrics”).
- **Monitor** system performance to prevent issues.
- Troubleshoot technical issues by analyzing **logs** and metrics.
- **Document** solutions and update the knowledge base.
- **Escalate** complex problems to engineering teams.
- Provide training and support documentation to customers.

### What does the support engineer need?

- Discovery - 
	- HOW = What alerts have been triggered in our monitoring systems?
	- Timeline  - first issue
- list of issues - by severity 
	- secondary effects of those issues / if any
	- Quantify: **numbers**, **timeframes**, and **percentages**
- impact - 
	- category = technical  / business  / user 
- scope of issues - api / app / region / users with certain criteria
- Action
	- mitigation = Are there workarounds? Should they restart a service?
	- links to resources - runbook / past incident reports / monitoring dashboards
- Post-incident verification
	- which things to monitor - api error rate down? / logs etc 


### Hygiene: 
- Scannable message



---
## Bad Examples

Here are **5 more nuanced bad examples** of technical writing, focusing on mistakes a **mid-level engineer** might make (beyond basic errors, these reflect oversights in deeper context, prioritization, or institutional knowledge):

### **1. Surface-Level Root Cause Without Systemic Flaws**  
**Bad Example:**  
> *“Root Cause: The API failed because the authentication service was restarted accidentally.”*  

**Why It’s Bad:**  
- **Stops at the immediate trigger**, ignoring systemic gaps (e.g., lack of graceful shutdown, missing circuit breakers).  
- Mid-level engineers often fixate on the “what” but miss the “why” (e.g., *why* was the restart accidental? No safeguards?).  
- Support engineers can’t address underlying risks without this context.  

**Common Mistake:**  
Failing to highlight **preventable systemic issues** (e.g., “Root Cause: No health checks for dependent services during deployment, leading to cascading failures when auth service restarted”).  

---

### **2. Overloading with Internal Jargon/Slang**  
**Bad Example:**  
> *“The flibber widget in the quantum-sync module timed out, causing a grumble in the flux capacitor.”*  

**Why It’s Bad:**  
- Uses **team-specific nicknames** (“flibber widget”) or **overly cute metaphors** (“grumble”) instead of standard terms.  
- Mid-level engineers familiar with the system might forget that support teams or new hires won’t know internal slang.  
- Creates confusion during escalations or handoffs.  

**Common Mistake:**  
Assuming **shared tribal knowledge**. Always clarify:  
> *“The data synchronization component (internally called ‘quantum-sync’) failed due to a timeout in its request queue (‘flibber widget’).”*  

---

### **3. Unprioritized Timeline with Noise**  
**Bad Example:**  
> *“Timeline:  
> 09:00: Team standup.  
> 09:30: Deployed v2.1.0.  
> 10:15: Noticed errors in logs.  
> 11:00: Lunch break.  
> 12:30: Rolled back deployment.”*  

**Why It’s Bad:**  
- Includes **irrelevant events** (standup, lunch) and omits **key technical milestones** (e.g., when alerts fired, mitigation steps tried).  
- Mid-level engineers often document linearly without curating for **actionable insights**.  
- Distracts from critical moments (e.g., rollback delay analysis).  

**Common Mistake:**  
Failing to filter timelines for **diagnostic relevance**:  
> *“10:00: v2.1.0 deployed.  
> 10:12: Alert ‘API_ERROR_RATE > 25%’ triggered.  
> 10:30: Identified authentication latency spike.  
> 10:45: Rolled back to v2.0.3.  
> 11:00: Confirmed error rate normalized.”*  

---

### **4. Ignoring Post-Resolution Verification**  
**Bad Example:**  
> *“Resolution: Restarted the service. Impact resolved.”*  

**Why It’s Bad:**  
- **No proof the fix worked** (e.g., metrics, logs, user testing).  
- Mid-level engineers might assume that “no alerts = resolved,” but transient fixes can mask lingering issues (e.g., memory leaks).  
- Support engineers can’t validate recovery or answer customer questions about stability.  

**Common Mistake:**  
Skipping **verification steps**:  
> *“Resolution: Restarted the service and confirmed recovery via:  
> - Metric: `SERVICE_HEALTH` returned to 100%.  
> - Test: Executed 50 synthetic transactions successfully.  
> - Logs: No new errors in last 30 minutes.”*  

---

### **5. Failing to Link to Institutional Knowledge**  
**Bad Example:**  
> *“We followed standard procedures to mitigate the incident.”*  

**Why It’s Bad:**  
- **Vaguely references “standard procedures”** without naming runbooks, playbooks, or docs.  
- Mid-level engineers might assume everyone knows where to find SOPs, but support teams often juggle multiple systems.  
- Wastes time hunting for resources during critical moments.  

**Common Mistake:**  
Not **embedding references**:  
> *“Mitigation: Applied ‘High CPU Load’ runbook (Confluence link).  
> Key steps: Scaled worker nodes from 5 → 10, cleared stuck queues (see CLI command log).”*  

### **6. Bad vs. Good Impact Statement**  
**Bad:**  
> *“There was a major outage. Users couldn’t use the app. We fixed it quickly.”*  

**Mistakes:**  
- Vague (“major,” “quickly”).  
- No scope, metrics, or user symptoms.  
- Zero actionable data.  

**Good:**  
> **Impact Breakdown:**  
> - **Scope:** Users in US region on Android devices (v12+).  
> - **Severity:** 90% of login attempts failed (15:00–16:30 UTC).  
> - **User Symptoms:** “Invalid credentials” errors despite correct passwords.  
> - **Business Impact:** 50k failed logins, 10% drop in active hourly users.  
> - **Root Cause:** OAuth token validation timeout (30s → 5s in config).  

---

### **Key Nuances for Mid-Level Engineers**  
1. **Dig deeper than the trigger**: Explain *why* the incident was possible, not just *what* happened.  
2. **Standardize terminology**: Avoid internal slang; clarify if unavoidable.  
3. **Curate timelines**: Remove noise, highlight diagnostic milestones.  
4. **Prove resolution**: Share metrics, tests, and logs that confirm stability.  
5. **Link to institutional knowledge**: Assume nothing is “common knowledge.”  

 