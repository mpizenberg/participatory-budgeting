# Ranking Surveys: A Detailed Report on Methodologies

Ranking surveys are designed to elicit ordered preferences across a set of options. Unlike rating scales — where respondents independently score each item and may rate everything as "important" — ranking forces trade-offs and reveals what truly matters most to respondents ([Qualtrics](https://www.qualtrics.com/articles/strategy-research/rating-or-ranking-choosing-the-best-question-type-for-your-data/), [OpinionX](https://www.opinionx.co/blog/choosing-a-survey-ranking-method)). This report surveys the major methodologies, their mechanics, their statistical underpinnings, and their trade-offs.

---

## 1. Direct (Standard) Ranking

**Mechanic.** Respondents arrange a list of items from most to least preferred — typically via drag-and-drop or by assigning each item a rank number (1 = most preferred). Each rank is occupied by exactly one item ([SurveyPlanet](https://blog.surveyplanet.com/ranking-survey-questions-examples-to-use-in-2025), [Suzy](https://www.suzy.com/blog/rating-vs-ranking-scales)).

**Strengths.** Familiar, fast, low setup cost.

**Weaknesses.** Reliability degrades sharply beyond ~5–7 items; respondents tend to be confident only about their top and bottom choices and to randomize the middle. Asking respondents to rank everything "in one fell swoop" produces less reliable and valid data than designs based on pairwise judgments ([1000minds](https://www.1000minds.com/decision-making/pairwise-comparison), [Qualtrics — Dos & Don'ts](https://www.qualtrics.com/articles/strategy-research/the-dos-and-donts-of-ranking-questions/)). Long lists also display poorly on mobile, and matrix-grid usage has fallen from ~50% (2015) to ~19% (2020) for this reason ([OpinionX](https://www.opinionx.co/blog/choosing-a-survey-ranking-method)).

---

## 2. Paired (Pairwise) Comparison

**Mechanic.** A list of N items is broken into a series of two-option head-to-head votes. Each comparison records a winner; an aggregate score is derived from the proportion of pairs an option wins ([OpinionX — Paired Comparison](https://www.opinionx.co/blog/paired-comparison), [Sage Encyclopedia](https://methods.sagepub.com/ency/edvol/encyclopedia-of-survey-research-methods/chpt/paired-comparison-technique)).

**Strengths.** Forces explicit trade-offs on each judgment, mitigates central-tendency and halo bias, and yields nuanced relative preferences ([Formplus](https://www.formpl.us/blog/paired-comparison-scale-in-surveys-purpose-implementation-analysis)).

**Weaknesses.** A full design requires N(N−1)/2 comparisons, which scales quadratically. Balanced incomplete block designs are typically used to keep respondent burden manageable.

**Statistical model.** Results are commonly analyzed with the **Bradley–Terry model**, which assumes each item has a latent strength πᵢ and that P(i beats j) = πᵢ / (πᵢ + πⱼ). Parameters are usually fit by maximum likelihood. The model was introduced by Bradley and Terry (1952), though Zermelo had studied an equivalent in the 1920s. It is widely used today to rank sports competitors, products, journals, and AI model outputs (notably as the basis for reward models in RLHF) ([Wikipedia — Bradley–Terry](https://en.wikipedia.org/wiki/Bradley%E2%80%93Terry_model), [Stanford lecture notes](https://web.stanford.edu/class/archive/stats/stats200/stats200.1172/Lecture24.pdf)). It generalizes to the **Plackett–Luce model** for full or partial rankings.

---

## 3. Best–Worst Scaling (MaxDiff)

**Mechanic.** Respondents are shown sets of 3–6 items and asked to pick the _best_ and _worst_ in each set. Across many such sets (designed via balanced incomplete block designs), each item appears multiple times and against varied competitors. Latent utilities are estimated from the choice patterns ([Qualtrics — MaxDiff intro](https://www.qualtrics.com/articles/strategy-research/an-introduction-to-maxdiff/), [Displayr](https://www.displayr.com/what-is-maxdiff/)).

**Strengths.** Eliminates "everything is important" responses, immune to scale-use bias and scale-meaning bias, gathers preference data faster than full pairwise comparison while preserving statistical rigor ([Pollfish](https://www.pollfish.com/resources/blog/pollfish-school/what-is-a-maxdiff-analysis-the-new-best-worst-scaling-feature/), [SurveyMonkey](https://www.surveymonkey.com/market-research/resources/best-worst-scaling/)).

**Weaknesses.** More complex to design and analyze; requires multinomial-logit-style estimation. It produces only relative — not absolute — importance.

**History.** Developed by Jordan Louviere and colleagues at the University of Sydney's Centre for the Study of Choice ([Displayr](https://www.displayr.com/what-is-maxdiff/)).

---

## 4. Conjoint Analysis

**Mechanic.** Items are described as bundles of attribute levels (e.g., a phone defined by brand × price × battery life). Respondents express preferences over full profiles, from which part-worth utilities for each attribute level are derived ([Wikipedia](https://en.wikipedia.org/wiki/Conjoint_analysis), [Qualtrics — Conjoint types](https://www.qualtrics.com/articles/strategy-research/types-of-conjoint/)).

**Common variants.**

- **Choice-Based Conjoint (CBC):** the dominant format — respondents choose a preferred profile from 2–6 alternatives per task.
- **Full-Profile / Ratings-Based:** respondents rate or rank complete profiles.
- **Adaptive Conjoint Analysis (ACA):** the survey adapts to a respondent's prior answers, focusing on attributes most relevant to them.
- **MaxDiff Conjoint:** best/worst on profiles instead of items.

([Conjointly](https://conjointly.com/guides/what-is-conjoint-analysis/), [HBS Online](https://online.hbs.edu/blog/post/what-is-conjoint-analysis), [Sawtooth](https://sawtoothsoftware.com/conjoint-analysis))

**Strengths.** Reveals not just _which_ option is preferred but _why_ — the relative weight of each attribute. Used widely for pricing research, feature prioritization, and demand forecasting, including in healthcare to elicit patient preferences ([PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC8879380/)).

**Weaknesses.** Design-heavy; requires careful selection of attributes and levels, plus larger sample sizes for stable estimates.

---

## 5. Constant Sum / Points Allocation

**Mechanic.** Respondents distribute a fixed number of points (commonly 100) across options to reflect the _magnitude_ of relative preference, not just the order ([OpinionX — Constant Sum](https://www.opinionx.co/blog/constant-sum), [QuestionPro](https://www.questionpro.com/features/constant-sum.html)).

**Strengths.** Yields ratio-level data: a 40/30/20/10 allocation says option A is twice as preferred as C, not merely "ranked higher." Forces trade-offs because the budget is fixed ([SurveySparrow](https://surveysparrow.com/what-is-constant-sum/), [Drive Research](https://www.driveresearch.com/market-research-company-blog/what-is-a-constant-sum-scale-in-market-research/)).

**Weaknesses.** Mentally demanding — respondents must keep a running total in their heads. Quality drops with more than ~5–7 options. Strong primacy effects unless rows are randomized.

---

## 6. Ranked-Choice Voting Methods

When ranking surveys are used to elect a single winner or aggregate group preferences, the choice of _aggregation rule_ matters as much as the ballot format. The main families:

- **Plurality / First-Past-the-Post:** count only first choices. Vulnerable to vote splitting.
- **Instant-Runoff Voting (IRV) / Single Transferable Vote:** iteratively eliminate the lowest first-choice candidate and reallocate ballots until one option exceeds 50%.
- **Borda Count:** assign descending points to ranks (e.g., N−1 for 1st, 0 for last) and sum. Intuitive but unusually vulnerable to tactical voting; under fully strategic voting it tends to a near-tie ([Wikipedia — Borda count](https://en.wikipedia.org/wiki/Borda_count)).
- **Condorcet Methods:** elect the candidate (if any) who beats every other in pairwise contests. Multiple completion rules (Schulze, Ranked Pairs, Minimax) handle the cyclic case.

A 2024–25 empirical study of ~4,000 real-world ranked-ballot elections (including ~2,000 political elections from the U.S., Australia, and Scotland) found that **IRV and Condorcet methods perform best** — they are least susceptible to spoiler effects, mostly resistant to strategic manipulation, and unlikely to elect fringe candidates ([Mathematics & Democracy Institute](https://mathematics-democracy-institute.org/empirical-analysis-of-ranked-choice-voting-methods/), [arXiv preprint](https://arxiv.org/html/2501.00618v1), [Springer — Soc. Choice & Welfare](https://link.springer.com/article/10.1007/s00355-025-01638-2)). On strategic resistance specifically, Princeton work argues Condorcet methods are less susceptible to strategic voting than alternatives ([Princeton — Wang & Cuff](https://www.princeton.edu/~cuff/publications/wang_strategic_voting.pdf)).

This matters for survey designers: identical ballots can yield different "winners" depending on the aggregation rule.

---

## 7. Q-Methodology (Q-Sort)

**Mechanic.** A hybrid qualitative–quantitative method developed by William Stephenson in the 1930s. Participants sort a curated set of statements (the "Q-sample") into a forced quasi-normal distribution from "most agree" to "most disagree." The resulting Q-sorts are then factor-analyzed across _people_ (rather than items) to identify shared viewpoints ([Wikipedia](https://en.wikipedia.org/wiki/Q_methodology), [qmethod.org](https://qmethod.org/), [Better Evaluation](https://www.betterevaluation.org/methods-approaches/methods/q-methodology)).

**Strengths.** Reveals subjective viewpoints and discourses, not population frequencies. Used in nursing, public health, education, and policy research ([ERIC PDF — Damio](https://files.eric.ed.gov/fulltext/EJ1207820.pdf), [Wiley — Rost 2021](https://onlinelibrary.wiley.com/doi/full/10.1002/capr.12367)).

**Weaknesses.** Small-N by design; not generalizable to populations. Statement curation introduces researcher influence.

---

## 8. Forced-Choice Ranking (in Personality / Rater Contexts)

A specialization where respondents pick "most" and "least" from a small set of items balanced to be similarly socially desirable. Used heavily in personality assessment and 360-degree feedback to reduce response style biases (acquiescence, halo, central tendency). Modern IRT-based scoring (e.g., Thurstonian IRT, forced-choice models) recovers normative scores from such ipsative data ([Sage — Hung & Huang 2022](https://journals.sagepub.com/doi/full/10.3102/10769986221104207), [OpinionX — Forced Choice](https://www.opinionx.co/blog/forced-choice-ranking)).

---

## 9. Ranking vs. Rating: When to Use Which

| Goal                                            | Recommended approach                  |
| ----------------------------------------------- | ------------------------------------- |
| General attitudes / satisfaction                | Rating scale (Likert, NPS)            |
| Setting priorities, allocating scarce resources | Ranking (forced trade-off)            |
| Measuring magnitude of preference               | Constant sum                          |
| Few items (≤7), simple study                    | Direct ranking                        |
| Many items (>7), need rigor                     | MaxDiff or pairwise comparison        |
| Multi-attribute products                        | Conjoint analysis                     |
| Group decision / election                       | RCV with IRV or Condorcet aggregation |
| Mapping shared viewpoints in a population       | Q-sort                                |

Sources: [Qualtrics](https://www.qualtrics.com/blog/rating-or-ranking-choosing-the-best-question-type-for-your-data/), [Suzy](https://www.suzy.com/blog/rating-vs-ranking-scales), [OnePulse](https://www.onepulse.com/ranking-scale-vs-rating-scale/), [Attest](https://www.askattest.com/blog/articles/survey-rating-scales).

---

## 10. Common Biases and Mitigations

- **Primacy effect:** tendency to pick the first acceptable option. Stronger with longer lists, lower cognitive ability, older respondents.
- **Recency effect:** tendency to pick the last item, especially with auditory presentation.
- **Survey fatigue:** beyond ~10–15 minutes, response quality collapses; respondents satisfice or randomize.
- **Mitigations:** randomize option order between respondents; cap list length at 5–7 for direct ranking; switch to MaxDiff or pairwise for longer lists; pre-test for length and clarity ([SurveyPlanet — order](https://blog.surveyplanet.com/survey-question-order-does-it-matter), [Qualtrics — fatigue](https://basecamp.qualtrics.com/minimizing-survey-fatigue-and-bias), [SurveyMonkey — order bias](https://www.surveymonkey.com/curiosity/eliminate-order-bias-to-improve-your-survey-responses/), [GESIS guidelines (PDF)](https://www.gesis.org/fileadmin/admin/Dateikatalog/pdf/guidelines/response_biases_standardized_surveys_bogner_landrock_2016.pdf), [PMC — Catalog of Biases](https://pmc.ncbi.nlm.nih.gov/articles/PMC1323316/)).

A formal comparison study by Smyth, Olson & Burke (2018) directly compared ranking question formats in mail surveys and documented format-specific data quality differences ([Sage Journals](https://journals.sagepub.com/doi/abs/10.1177/1470785318767286)).

---

## 11. Choosing a Methodology — Decision Heuristic

1. **How many items?** ≤7 → direct ranking is fine. >7 → pairwise / MaxDiff.
2. **Do items have attributes?** Yes → conjoint. No → MaxDiff or pairwise.
3. **Need magnitude, not just order?** Constant sum.
4. **Aggregating to a single group choice?** Choose IRV or a Condorcet method; avoid Borda where strategic voting is plausible.
5. **Studying _viewpoints_ rather than _averages_?** Q-sort.
6. **Limited sample / mobile-first?** MaxDiff (faster than full pairwise, mobile-friendly).

---

## Sources

- [OpinionX — Choosing a survey ranking method](https://www.opinionx.co/blog/choosing-a-survey-ranking-method)
- [OpinionX — Paired Comparison guide](https://www.opinionx.co/blog/paired-comparison)
- [OpinionX — Constant Sum](https://www.opinionx.co/blog/constant-sum)
- [OpinionX — Forced Choice Ranking](https://www.opinionx.co/blog/forced-choice-ranking)
- [OpinionX — MaxDiff Analysis](https://www.opinionx.co/blog/maxdiff-analysis)
- [OpinionX — 13 Types of Conjoint Analysis](https://www.opinionx.co/blog/conjoint-analysis-types)
- [Qualtrics — Rating vs. ranking](https://www.qualtrics.com/blog/rating-or-ranking-choosing-the-best-question-type-for-your-data/)
- [Qualtrics — Ranking dos and don'ts](https://www.qualtrics.com/articles/strategy-research/the-dos-and-donts-of-ranking-questions/)
- [Qualtrics — MaxDiff introduction](https://www.qualtrics.com/articles/strategy-research/an-introduction-to-maxdiff/)
- [Qualtrics — Types of Conjoint](https://www.qualtrics.com/articles/strategy-research/types-of-conjoint/)
- [Qualtrics — Minimizing survey fatigue and bias](https://basecamp.qualtrics.com/minimizing-survey-fatigue-and-bias)
- [Displayr — What is MaxDiff?](https://www.displayr.com/what-is-maxdiff/)
- [Pollfish — Best-Worst Scaling](https://www.pollfish.com/resources/blog/pollfish-school/what-is-a-maxdiff-analysis-the-new-best-worst-scaling-feature/)
- [SurveyMonkey — Best-Worst Scaling](https://www.surveymonkey.com/market-research/resources/best-worst-scaling/)
- [Conjointly — What is Conjoint Analysis](https://conjointly.com/guides/what-is-conjoint-analysis/)
- [Conjointly — Constant sum](https://conjointly.com/guides/constant-sum-question/)
- [Sawtooth Software — Conjoint Analysis](https://sawtoothsoftware.com/conjoint-analysis)
- [HBS Online — Conjoint Analysis](https://online.hbs.edu/blog/post/what-is-conjoint-analysis)
- [PMC — Conjoint Analysis in Patient Preference Research](https://pmc.ncbi.nlm.nih.gov/articles/PMC8879380/)
- [Wikipedia — Conjoint analysis](https://en.wikipedia.org/wiki/Conjoint_analysis)
- [Wikipedia — Bradley–Terry model](https://en.wikipedia.org/wiki/Bradley%E2%80%93Terry_model)
- [Stanford — Bradley-Terry lecture notes (PDF)](https://web.stanford.edu/class/archive/stats/stats200/stats200.1172/Lecture24.pdf)
- [Wikipedia — Borda count](https://en.wikipedia.org/wiki/Borda_count)
- [Wikipedia — Ranked voting](https://en.wikipedia.org/wiki/Ranked_voting)
- [Mathematics & Democracy Institute — Empirical RCV analysis](https://mathematics-democracy-institute.org/empirical-analysis-of-ranked-choice-voting-methods/)
- [arXiv — Borda count variations on RCV data](https://arxiv.org/html/2501.00618v1)
- [Springer — Social Choice & Welfare (Borda variations)](https://link.springer.com/article/10.1007/s00355-025-01638-2)
- [Princeton — Condorcet & strategic voting (PDF)](https://www.princeton.edu/~cuff/publications/wang_strategic_voting.pdf)
- [Wikipedia — Q methodology](https://en.wikipedia.org/wiki/Q_methodology)
- [qmethod.org](https://qmethod.org/)
- [Better Evaluation — Q methodology](https://www.betterevaluation.org/methods-approaches/methods/q-methodology)
- [ERIC — Damio, Q methodology overview (PDF)](https://files.eric.ed.gov/fulltext/EJ1207820.pdf)
- [Wiley — Rost 2021, Q-sort in psychotherapy research](https://onlinelibrary.wiley.com/doi/full/10.1002/capr.12367)
- [Sage — Hung & Huang 2022, Forced-Choice Ranking Models](https://journals.sagepub.com/doi/full/10.3102/10769986221104207)
- [Sage — Smyth, Olson & Burke 2018, Comparing ranking question formats](https://journals.sagepub.com/doi/abs/10.1177/1470785318767286)
- [1000minds — Pairwise comparison](https://www.1000minds.com/decision-making/pairwise-comparison)
- [Sage Encyclopedia — Paired Comparison Technique](https://methods.sagepub.com/ency/edvol/encyclopedia-of-survey-research-methods/chpt/paired-comparison-technique)
- [Formplus — Paired Comparison Scale](https://www.formpl.us/blog/paired-comparison-scale-in-surveys-purpose-implementation-analysis)
- [QuestionPro — Constant Sum](https://www.questionpro.com/features/constant-sum.html)
- [SurveySparrow — Constant Sum](https://surveysparrow.com/what-is-constant-sum/)
- [Drive Research — Constant Sum Scale](https://www.driveresearch.com/market-research-company-blog/what-is-a-constant-sum-scale-in-market-research/)
- [Suzy — Ranking vs. Rating](https://www.suzy.com/blog/rating-vs-ranking-scales)
- [OnePulse — Ranking Scale vs. Rating Scale](https://www.onepulse.com/ranking-scale-vs-rating-scale/)
- [Attest — Survey rating scales](https://www.askattest.com/blog/articles/survey-rating-scales)
- [SurveyPlanet — Question order](https://blog.surveyplanet.com/survey-question-order-does-it-matter)
- [SurveyMonkey — Order bias](https://www.surveymonkey.com/curiosity/eliminate-order-bias-to-improve-your-survey-responses/)
- [GESIS — Response biases in standardised surveys (PDF)](https://www.gesis.org/fileadmin/admin/Dateikatalog/pdf/guidelines/response_biases_standardized_surveys_bogner_landrock_2016.pdf)
- [PMC — A Catalog of Biases in Questionnaires](https://pmc.ncbi.nlm.nih.gov/articles/PMC1323316/)
