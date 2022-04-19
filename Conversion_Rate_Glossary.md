# Conversion Rate and Related Terms Glossary

A set of terms and definitions that I think are useful for the Conversion Rate.

**Contribution Tiers**: Equivalent to funnel "layers"

**Types of Conversion Rates**
- Hierarchical Conversion Rates: Multiple Conversion Rates in a related set, forming a pyramid shape, where each Conversion Rate represents % of conversion to a higher level. These can also be seen as multiple related standalone conversion rates. 
    - Example: Group A, D0, D1, D2.
- Standalone Conversion Rates: One number, consisting of a denominator representing "new" contributors and the numerator representing "sustained" contributors. Different Conversion Rates can be obtained due to the data being examined combined with filters. 
    - Examples:
        - Conversion Rates at the repo, project or community levels
        - Conversion Rates with respect to different intervals / different time periods
        - Conversion Rates with respect to different developer roles (D0, D1, D2 for example)
        - Conversion Rates with respect to different types of contributions (technical, non-technical). As mentioned on https://chaoss.community/metric-types-of-contributions/, it would be better to recognize different types of contributions independently.

**Definitions of Activity-Based Roles in Conversion Rate**: New and sustained are the most relevant here, as they are directly mentioned in the metric's question. Thanks to Elizabeth Barron for suggesting standardizing these definitions - they are kept consistent as possible with other CHAOSS metrics. 
-  Roles in "Onion" definition (these can be used to generate Conversion Rates from a different perspective or qualitatively show insight in Conversion Rates)
    - Casual - Contributing last 5% of activity
    - Regular - Contributing next 15% of activity
    - Core - Either, contributing 80% of the activity OR >=1 contribution per month for 3 months in a row (as defined in wg-DEI)
- Roles in "number of contributions/time period" definition
    - New - First contribution was made within interval of time (consistent with new contributors and https://chaoss.community/metric-occasional-contributors/) 
    - Occasional/Fly By -  Less than n contributions per 365 days AND were new in specified time period
    - Sustained -  ≥ n contributions per 365 days AND were new in specified time period
    - Roles in "leadership" definition by privilege permission on repositories as leadership is defined in CHAOSS https://handbook.chaoss.community/community-handbook/about/path-to-leadership 
        - Github's Repository roles for organizations: Read, Triage, Write, Maintain, Admin. Or, mine this information from trace data. https://docs.github.com/en/organizations/managing-access-to-your-organizations-repositories/repository-roles-for-an-organization 
        - GitLab Roles: Guest, Reporter, Developer, Maintainer, Owner: https://docs.gitlab.com/ee/user/permissions.html 
    - Non-technical leadership roles: https://chaoss.community/metric-inclusive-leadership/ 

**Analytical Unit**:
    - Conversion Rate can be calculated at the repository, project, or community level. It can be computed for a single repo/project/community but ultimately, we seek to compute Conversion Rate for pairwise comparison between projects/communities. Therefore, a set of one or more repositories will be the analytical unit (invariant) considered. By adjusting the set of repositories used for data collection, different conversion rates may be obtained. 
    - Choosing Repo for Data Collection
    - In my approach: for the initial testing of the Conversion Rate Metric Model, I will start with a single active repository to use as data (e.g. Augur), then once the code is stable, analyze a larger collection of repositories as the analytical unit and compare across multiple projects - for example Tensorflow vs. Pytorch. 
