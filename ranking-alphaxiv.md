<alphaxiv-thinking-title title="Searching for ranking survey methodologies" />

<alphaxiv-thinking-title title="Reading papers on ranking models" />

Ranking surveys and polls typically use specialized statistical models to transform ordinal preferences (e.g., "Item A is better than Item B") into numerical scores. The core challenge in these methodologies is handling the inherent noise and subjectivity of human judgment, as well as the incomplete nature of most survey data where every respondent may not rank every item.

## Key Ranking Models

The most widely used methodologies for analyzing ranking data are based on **probabilistic choice models**. These models assume that each item has an underlying "strength" or "utility," and the probability of one item being ranked above another depends on the difference in these strengths.

*   **Bradley-Terry Model**: The fundamental model for pairwise comparisons. It calculates the probability that item $i$ is preferred over item $j$ as:
    $$p_{ij} = \frac{\pi_i}{\pi_i + \pi_j}$$
    where $\pi_i$ and $\pi_j$ are the positive-valued strength parameters of the items. [Model Foundations](https://alphaxiv.org/abs/2312.13619v1?page=3)
*   **Plackett-Luce Model**: A generalization of Bradley-Terry for ranking more than two items. It models a full ranking as a sequence of choices: the respondent first picks the best item from the set, then the best from the remaining items, and so on.
*   **Thurstone-Mosteller Model**: Similar to Bradley-Terry but assumes that the "sensation" or strength of an item follows a **Normal distribution** rather than a Logistic distribution. While mathematically different, these two models often yield empirically similar ranking results. [Comparison](https://alphaxiv.org/abs/2312.13619v1?page=3)
*   **Mallows Model**: A distance-based model that assumes there is a "true" consensus ranking, and the probability of a respondent's ranking decreases as its distance (e.g., Kendall's tau) from the consensus increases.

---

## Comparison of Methodology Types

Methodologies can be categorized by how they collect and aggregate data:

| Method | Description | Pros | Cons |
| :--- | :--- | :--- | :--- |
| **Forced Ranking** | Respondents rank all items from $1$ to $n$. | Highly granular; eliminates "tie" bias. | High cognitive load for many items. |
| **Pairwise Comparison** | Respondents choose between two items at a time. | Simple for the respondent; very robust. | Requires many comparisons ($n(n-1)/2$). |
| **Best-Worst Scaling** | Respondents pick only the best and worst from a subset. | Efficient; identifies extremes well. | Less info on middle-ranked items. |
| **Ratings-to-Rankings** | Converting Likert scales (1-5) into rankings. | Easy to implement in standard software. | Leads to many ties; loses ordinal nuance. |

---

## Technical Challenges in Ranking

### Handling Heterogeneity
Modern survey methodology often assumes that respondents may belong to different "clusters" with distinct preferences (e.g., different demographics having different tastes). **Mixture Models** are used to identify these latent groups.
> "We simultaneously estimate both the amount and type of heterogeneity among judges... [to] accurately combine information from rankings and incomplete ratings." [Heterogeneity](https://alphaxiv.org/abs/2301.09755v1?page=5)

### Rankings vs. Ratings
Surveys often collect both **rankings** (ordinal) and **ratings** (cardinal). 
*   **Rankings** are scale-free and force respondents to make trade-offs, making them resistant to "response styles" (e.g., some people always giving high scores). 
*   **Ratings** provide more granularity and allow for ties, but are highly subjective. 
Recent "unified" models, such as the **BTL-Binomial**, attempt to combine both data types into a single estimation framework to leverage the strengths of each. [Joint Modeling](https://alphaxiv.org/abs/2301.09755v1?page=1)
