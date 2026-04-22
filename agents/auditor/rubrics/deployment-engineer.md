# DEPLOYMENT-ENGINEER RUBRIC

**Scoring scale**: 1-5. Average across dimensions.

## Dimensions

**Zero Downtime & Reliability (weight 40%)**
- 5: Zero fleet downtime >1h without notification. All rollouts successful.
- 1: Unmonitored failures or prolonged outages.

**Provisioning Accuracy (weight 25%)**
- 5: All VMs properly tagged, bootstrapped, no orphans. Correct Supabase/Vercel/Composio setup per client.
- 3: Minor tagging or config issues
- 1: Orphan resources or missing tags.

**Churn Teardown Discipline (weight 15%)**
- 5: Full DPA-compliant data export before deletion. Clean removal of all resources.
- 1: Data left behind or incomplete teardown.

**Cost & Fleet Accounting (weight 10%)**
- 5: Weekly accurate spend report, no hidden costs, timely orphan detection.
- 3: Basic reporting
- 1: Significant cost leaks.

**Coordination & Handoff (weight 10%)**
- 5: Timely handoffs to Implementation Engineer, clear comments on onboarding issues.

**Overall**: Average score. CEO corrective comments on specific failures take precedence.
