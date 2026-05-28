# Single-Winner Voting Methods for a 7-Candidate Field

_A focused research note on ballot formats and tallying rules for single-winner elections with a small candidate field (n = 7), evaluated against social-choice criteria._

The question is narrow: **which aggregation rule should be used to elect one winner from seven candidates?** The 7-candidate setting is small enough that several rules that would be infeasible at parliamentary scale (Kemeny–Young, full pairwise matrices, ranked ballots) become trivially affordable, and large enough that plurality clearly breaks (vote-splitting probability becomes severe).

### Scope and threat model

This note evaluates **aggregation rules**, not the surrounding election infrastructure. The threat model is split accordingly:

**Out of scope** (assumed to be solved at a different layer):

- **Identity and ballot integrity.** One ballot per eligible voter, no Sybil identities, no double-voting, no ballot-stuffing. Handled by authentication, identity binding, and the underlying voting protocol.
- **Ballot secrecy, coercion, vote-buying.** Handled by ballot encryption, receipt-freeness, or coercion-resistant protocols (ZK / homomorphic / MACI-style commitments).

**In scope** (the aggregation rule must be robust to these):

- **Strategic voting.** A voter submits a ballot that misrepresents their true preferences in order to game the outcome — burying a rival, ranking a compromise above a sincere favorite, bullet-voting, min-maxing scores. The strategy-resistance criterion measures how often such manipulation is profitable and how often it backfires.
- **Strategic candidate entry.** A faction enters near-duplicate "clone" candidates to split or steer votes (the spoiler effect under Plurality; vote-teaming under Borda). Clone-independence measures resistance to this.
- **Coalition / bloc behavior.** Coordinated groups exploiting rule-specific weaknesses (the chicken dilemma in Approval, teaming in Borda, center-squeeze in IRV).

In short: the rule must produce a defensible winner _even when voters and candidates are adversarial_, given that authentication and ballot privacy are handled elsewhere. "Honest ballots" are explicitly **not** assumed — sincere voting is the best case, and strategic resistance is precisely about the worst case.

---

## 1. Ballot formats

The ballot is the data structure the voter submits; the tallying method is the function applied to the collection of ballots. Different tallying methods require different ballots.

| Ballot format                  | Voter expresses                                    | Cognitive load (n=7)                     | Information captured                     |
| ------------------------------ | -------------------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| **Plurality / FPTP**           | One favorite candidate                             | Trivial                                  | Lowest — ignores all preferences past #1 |
| **Approval**                   | Subset of "acceptable" candidates                  | Light — 7 yes/no checks                  | Binary partition                         |
| **Score / Range**              | Independent numeric grade per candidate (e.g. 0–5) | Moderate — 7 independent judgements      | Cardinal intensity                       |
| **STAR ballot**                | Same as Score (0–5)                                | Moderate                                 | Cardinal + induced ordinal via runoff    |
| **Ranked (full)**              | Strict total order of all 7                        | Heavier — but well within human capacity | Full ordinal information                 |
| **Ranked (truncated)**         | Ordered list of top-k, rest tied last              | Light–moderate                           | Partial ordinal                          |
| **Pairwise direct**            | 21 head-to-head choices                            | Heaviest — quadratic burden              | Full pairwise (with possible cycles)     |
| **Graded (Majority Judgment)** | Verbal grade per candidate (Excellent…Reject)      | Moderate                                 | Ordinal cardinal, common-language        |

Empirical research on ranking surveys [(Qualtrics)](https://www.qualtrics.com/articles/strategy-research/the-dos-and-donts-of-ranking-questions/), [(1000minds)](https://www.1000minds.com/decision-making/pairwise-comparison) finds that respondents reliably distinguish only their top 3–4 and bottom 1–2; the middle of a list of 7 is fuzzy. This argues against ballots that demand a strict total order over all 7 (pure Borda, pure Kemeny–Young) and in favor of either (a) **truncated rankings** with safe equality, or (b) **score/grade ballots**, which let voters cluster items they cannot distinguish.

---

## 2. Tallying methods

### 2.1 Plurality / FPTP

Count first-place votes only. At n=7 this is severely vote-split: a faction of 30% can beat six factions of ~12% each, even if the 30% candidate is despised by 70% of voters. Universally rejected by the voting-theory literature for multi-candidate fields ([electowiki](https://electowiki.org/wiki/First-past-the-post)).

### 2.2 Approval voting

Each ballot is a set; the candidate in the most sets wins. Simple, monotonic, clone-independent, satisfies IIA and participation. Main weakness: the **chicken dilemma** — voters of two similar candidates must guess whether to approve only their favorite or also the ally. Champion: [Center for Election Science](https://electionscience.org/library/approval-voting/).

### 2.3 Score / Range voting

Sum (or average) of per-candidate scores. Satisfies IIA, monotonicity, participation, clone independence. Vulnerable to **min-max strategy** (rational voters score everyone 0 or max, collapsing back to approval). Highest utilitarian efficiency in simulations ([rangevoting.org](https://rangevoting.org/)).

### 2.4 STAR voting (Score Then Automatic Runoff)

Two stages: (1) compute total scores, take the top two; (2) the candidate preferred on more ballots wins the runoff. Adopted by the [Equal Vote Coalition](https://www.equal.vote/criteria) and used for internal party elections in Oregon. Fails Condorcet, monotonicity (rarely), later-no-harm, and participation; but [simulations](https://www.equal.vote/strategic-star) show a strategic-success-to-backfire ratio near 1:1, vs ~3:1 for IRV and ~18:1 for plurality.

**Why the strategic resistance is structural, not coincidental.** The two stages create opposing pressures on any strategic voter:

- **Score phase rewards exaggeration.** To maximize a favorite's chance of reaching the runoff, give them 5 and rivals 0 (bullet voting / min-max — the strategy that collapses pure Score voting back to Approval).
- **Runoff phase rewards honesty.** Among the final two, only the _relative_ score matters — 5-vs-4 counts the same as 5-vs-0. Any compression you applied in the score phase gives you zero leverage in the runoff if your favorite didn't make it.

The optimal strategy depends on predicting which two candidates will reach the runoff — exactly the information strategists don't have. Honest scoring is close to optimal under uncertainty, and dishonest scoring frequently **backfires**: bullet-voting your favorite can knock an acceptable compromise out of the runoff and hand the win to your least-preferred candidate. STAR also passes the **majority criterion in the runoff** — a true majority winner among the top two always wins — and Condorcet failures are rare in practice, requiring the Condorcet winner to fail to reach the top two by total score.

### 2.5 Instant Runoff Voting (IRV / Ranked Choice)

Eliminate the candidate with the fewest first-place votes, transfer their ballots to the next non-eliminated preference, repeat until one candidate has a majority. Familiar, "feels democratic," satisfies later-no-harm and Condorcet-loser. Critically, **fails monotonicity, participation, and the Condorcet criterion** — the 2009 Burlington VT mayoral election and the 2022 Alaska special U.S. House election are real-world cases where IRV elected someone other than the Condorcet winner, and Burlington exhibited a documented non-monotonic outcome ([Miller, UMBC](https://userpages.umbc.edu/~nmiller/MONFAILURE.R2.NRM.pdf), [Graham-Squire & Zayatz arXiv:2303.00108](https://arxiv.org/html/2303.00108v2)). Estimated lower bound for monotonicity failure in competitive 3-candidate IRV elections: ~15%.

### 2.6 Condorcet methods (winner is the candidate who beats all others head-to-head, with a tiebreaker for cycles)

All Condorcet methods take the same input: ranked ballots converted to a **pairwise matrix** M, where `M[A][B]` is the count of voters ranking A above B. A pairwise "victory" A→B has margin `M[A][B] − M[B][A]`. All Condorcet methods elect the **Condorcet winner** when one exists (a candidate who beats every other head-to-head). They differ only in how they resolve a **Condorcet cycle** (A beats B, B beats C, C beats A — no candidate beats all others). In real elections a Condorcet winner exists ~95% of the time, so cycle-resolution differences are mostly theoretical — but they matter when they don't.

#### Ranked Pairs (Tideman)

Sort every pairwise victory from strongest margin to weakest. Walk down the list; "lock in" each victory as a directed edge **unless it would create a cycle** with edges already locked, in which case skip it. The result is a directed acyclic graph; the winner is the source (the candidate with no incoming locked edges). Intuition: _respect the strongest pairwise preferences first; when forced into a cycle, drop its weakest link_. Satisfies Condorcet, Condorcet-loser, monotonicity, clone independence, reversal symmetry, Smith, Pareto. A 2022 paper in _Constitutional Political Economy_ [(Munger 2022)](https://link.springer.com/article/10.1007/s10602-022-09382-w) calls Ranked Pairs "the best Condorcet-compatible method."

#### Schulze (beatpath)

Define the **strength of a path** from X to Y as its weakest link (the smallest margin on any edge in the path). For each ordered pair (A, B), compute the strongest path from A to B over all possible paths through intermediate candidates. A beats B in the Schulze sense if the strongest A→B path is stronger than the strongest B→A path. The Schulze winner beats every other this way. Computed via a Floyd–Warshall-style transitive closure on the pairwise matrix. Intuition: _indirect victories count — a strong "A beats X, X beats B" chain is evidence about A vs B even with no direct contest_. Same criterion compliance as Ranked Pairs. Most-deployed Condorcet method in the world — Wikimedia, Debian, KDE, FSF Europe, several Pirate Parties, Ubuntu, Gentoo ([arXiv:1804.02973](https://arxiv.org/pdf/1804.02973)).

#### Kemeny–Young

Consider every total ordering of the n candidates (n! options). Score each ordering by summing, over every pair (X, Y) where the ordering ranks X above Y, the number of voters who agreed (i.e., `M[X][Y]`). The ordering with the highest score is the **Kemeny ranking**; its first place is the Kemeny–Young winner. Equivalently: the ranking that minimizes total pairwise disagreement with voter ballots (the **median ranking** under Kendall-tau distance). NP-hard in general ([Wikipedia](https://en.wikipedia.org/wiki/Kemeny%E2%80%93Young_method)) but at n=7 there are only 5,040 orderings — brute force runs in microseconds. Strongest information-theoretic justification of any Condorcet method (maximum-likelihood interpretation under a noise model). Uniquely among common Condorcet methods, satisfies **reinforcement / consistency** (merging two electorates that agree on a winner returns the same winner) and **local IIA**. But **fails clone independence** — adding a clone changes which orderings exist and shifts their scores.

#### How they differ in practice

- **When a Condorcet winner exists**, all three agree, always (~95% of real elections).
- **On small cycles**, Ranked Pairs and Schulze almost always agree; documented divergences are rare and pathological.
- **Kemeny–Young is the most likely to diverge**, because its global optimization can prefer an ordering that no sequential rule would lock in. It is also the only one that natively produces a **full social ranking** as primary output (the others give a winner; rankings are derived secondarily).
- **Clone independence** is the most consequential practical difference: Ranked Pairs and Schulze are clone-proof, Kemeny–Young is not (see §3.1).
- **Explainability** ranks roughly Ranked Pairs > Kemeny–Young > Schulze: "lock the strongest victories, skip cycles" is the most intuitive cycle-resolution rule; "the ordering that best matches all the votes" is conceptually clean but the enumeration is opaque; "strongest beat-path" requires the path-strength definition to land before anything else makes sense.

#### Minimax, Copeland (for completeness)

- **Minimax (Simpson–Kramer)** — winner is the candidate whose worst pairwise loss is smallest. Simplest Condorcet completion. Fails clone independence and Condorcet-loser ([equal.vote](https://www.equal.vote/minimax)).
- **Copeland** — count net pairwise wins. Simple, fails clone independence, often produces ties at small n.

### 2.7 Borda count and variants

- **Borda** — points = (rank − 1) inverted, so with 7 candidates the top rank scores 6 and the bottom 0. Intuitive, monotonic, satisfies Condorcet-loser and reversal symmetry; **fails Condorcet winner and clone independence (badly)**.
- **Nanson** — iteratively eliminate any candidate with below-average Borda score. Restores Condorcet-winner compliance but fails monotonicity.
- **Baldwin** — iteratively eliminate the lowest-Borda candidate. Condorcet-compliant but fails monotonicity.

**Why Borda's strategic resistance is rated "poor."** Three structural problems compound:

1. **Burying.** Ranking your favorite's strongest rival 7th instead of 2nd swings 5 points per ballot against them. There is no offsetting penalty for dishonesty. The dominant strategy is _favorite first, strongest rival last_ regardless of true preferences.
2. **Vote-teaming via clones.** Adding a clone of a candidate shifts down every candidate ranked below the clone pair, transferring Borda points to the clone's faction. Saari's canonical example: 3 voters prefer A>B>C, 2 prefer B>C>A, 2 prefer C>A>B; Borda elects A (8 points vs B's 7 and C's 6). Now add a clone C′ that every voter ranks immediately below C — suddenly C wins (13 vs B's 12 vs A's 11) without any underlying preference change. C's faction manufactured a win by fielding a sacrificial decoy. (See §3.1 for the general clone-independence concept.)
3. **Later-no-harm failure with a large penalty.** Every candidate you rank gives them points that can beat your favorite, so even non-strategic voters are pushed toward truncation — degrading the information content of ballots.

Donald Saari (Borda's most prominent academic defender) acknowledges these vulnerabilities openly: Borda is theoretically elegant under honest voting and highly manipulable under strategic voting. Simulation studies (Chamberlin & Featherston; Lepelley & Mbih; Green-Armytage) consistently rank Borda among the most manipulable common methods, comparable to plurality under coordinated voting despite being far better than plurality under honest voting. In any adversarial setting where similar candidates can enter, **the clone-teaming pathology is the disqualifying flaw**.

### 2.8 Bucklin

Tally first-place votes; if no majority, add second-place; repeat. Satisfies majority, mutual majority, monotonicity; fails Condorcet, later-no-harm, participation. Historical novelty (used in early-20th-century U.S. cities).

### 2.9 Coombs

Like IRV but eliminate the candidate with the **most last-place votes**. Satisfies Condorcet-loser, mutual majority; fails Condorcet-winner, monotonicity, participation, clone independence.

### 2.10 Majority Judgment (Balinski & Laraki 2010)

Median-grade rule on verbal-grade ballots. Highly strategy-resistant per the authors' analysis ([Wikipedia](https://en.wikipedia.org/wiki/Majority_judgment)). Critics ([rangevoting.org](https://rangevoting.org/MedianVrange.html)) point out: fails majority and Condorcet, can produce "tyranny of the median" outcomes, ties are common and the tie-break (closest-to-median) is delicate. Verbal grades are excellent for explainability; deliberation around grades is concrete.

---

## 3. Criteria comparison table

Compiled from [electowiki's Table of voting method criteria](https://electowiki.org/wiki/Table_of_voting_method_criteria) and [Wikipedia's _Comparison of voting rules_](https://en.wikipedia.org/wiki/Comparison_of_voting_rules). "Y" = satisfies, "N" = fails, "~" = depends on variant / probabilistically rare failure.

| Method            | Condorcet winner | Condorcet loser | Majority | Monotonic | IIA (Arrow) | Clone-indep. | Later-no-harm | Participation | Reversal sym. |    Strat. resistance    |
| ----------------- | :--------------: | :-------------: | :------: | :-------: | :---------: | :----------: | :-----------: | :-----------: | :-----------: | :---------------------: |
| Plurality (FPTP)  |        N         |        N        |    Y     |     Y     |      N      |      N       |       Y       |       Y       |       N       |        Very poor        |
| Approval          |        N         |        N        |    Y     |     Y     |     Y\*     |      Y       |       N       |       Y       |       Y       | Good (chicken dilemma)  |
| Score / Range     |        N         |        N        |    N     |     Y     |     Y\*     |      Y       |       N       |       Y       |       Y       |    Medium (min-max)     |
| **STAR**          |        N         |        Y        |    N     |     ~     |      N      |     Y\*      |       N       |       N       |       Y       |      **Very good**      |
| IRV               |        N         |        Y        |    Y     |   **N**   |      N      |      Y       |       Y       |     **N**     |       N       |         Medium          |
| Bucklin           |        N         |        N        |    Y     |     Y     |      N      |      N       |       N       |       N       |       Y       |         Medium          |
| Coombs            |        N         |        Y        |    Y     |     N     |      N      |      N       |       N       |       N       |       Y       |         Medium          |
| Borda             |        N         |        Y        |    N     |     Y     |      N      |    **N**     |       N       |       Y       |     **Y**     |   **Poor** (burying)    |
| Nanson            |        Y         |        Y        |    Y     |     N     |      N      |      N       |       N       |       N       |       Y       |         Medium          |
| Baldwin           |        Y         |        Y        |    Y     |     N     |      N      |      N       |       N       |       N       |       Y       |         Medium          |
| Minimax           |        Y         |        N        |    Y     |     Y     |      N      |      N       |       N       |       N       |       N       |         Medium          |
| Copeland          |        Y         |        Y        |    Y     |     Y     |      N      |      N       |       N       |       N       |       Y       |         Medium          |
| **Schulze**       |      **Y**       |      **Y**      |  **Y**   |   **Y**   |      N      |    **Y**     |       N       |       N       |     **Y**     |        **Good**         |
| **Ranked Pairs**  |      **Y**       |      **Y**      |  **Y**   |   **Y**   |      N      |    **Y**     |       N       |       N       |     **Y**     |        **Good**         |
| Kemeny–Young      |        Y         |        Y        |    Y     |     Y     |      N      |      N       |       N       |       N       |       Y       |          Good           |
| Majority Judgment |        N         |        N        |    N     |     ~     |     Y\*     |      Y       |       N       |       Y       |       N       | Very good (per authors) |

\* Approval, Score, and Majority Judgment satisfy IIA only with respect to ordinal preferences; with normalized scoring, adding a new option can rescale grades.

**Theoretical impossibilities to keep in mind.** Arrow's theorem ⇒ no rank-based method can satisfy IIA together with universal domain, non-dictatorship, and Pareto. Gibbard–Satterthwaite ⇒ every non-dictatorial deterministic single-winner rule is strategically manipulable in some profile. Equal Vote Coalition's argument ([equal.vote/q_and_a](https://www.equal.vote/q_and_a_common_cause)): **Later-No-Harm and Favorite-Betrayal-resistance are mutually exclusive** — any method satisfying both reduces to one ignoring later preferences, which is the property that creates the spoiler effect.

### 3.1 Independence of clones — what it means and why it matters

**Clone.** A candidate that _every voter_ ranks immediately adjacent to another candidate (the original), with no other candidate between them. A "clone set" is treated by voters as a block, though individuals may have preferences within the block. Typical real-world cases: two candidates from the same party or faction with near-identical platforms; a primary candidate plus a near-identical running mate; two near-duplicate options on a multi-option ballot. Voters who favor the underlying position rank them adjacent; voters who oppose it rank them both at the bottom.

**The criterion** (Tideman 1987): _adding or removing a clone of an existing candidate must not change which non-clone candidate wins._ A method that satisfies this is "clone-proof." A method that violates it is vulnerable to **strategic candidate entry** — factions can run decoys to shift outcomes.

**Two failure modes** (cloning can hurt _or_ help the original, depending on method):

- **Vote-splitting — Plurality fails this way.** Without clones: A gets 45%, B gets 55% → B wins. B runs a near-identical ally B′: A gets 45%, B gets 30%, B′ gets 25% → A wins. B's faction was split. The classic "spoiler" / "Nader" story.
- **Vote-teaming — Borda fails this way (the opposite failure).** Nothing about voter preferences over the originals changes, but the clone shifts other candidates down a slot on some ballots and transfers points to the clone's faction. See the §2.7 Saari example: cloning C flips the winner from A to C without changing anyone's underlying preferences.

**Why Schulze and Ranked Pairs are clone-proof.** Both work on the pairwise matrix. When a clone is added, the clones split each other's pairwise margins against outsiders, but the clone-set as a whole produces the same pairwise relationship with every non-clone candidate. The cycle-resolution rules (lock strongest margins / strongest beat-path) ignore intra-clone shuffling. A clone may win _instead of its original_, but the non-clone winner never changes.

**Why it is operationally important.** Wherever candidacies are not gatekept tightly — open primaries, internal organization elections, multi-option ballots, anything where two factions of a coalition can both stand — clone-independence is a real strategic vector, not just a theoretical curiosity. A rule that responds non-monotonically to candidate entry creates direct incentives for slate-stuffing or sacrificial decoys. In settings with tight gatekeeping (e.g. two-party general elections after a strict primary), clone-independence matters less because clones are simply not present.

**Quick scoreboard.** Clone-proof: Schulze, Ranked Pairs, IRV, STAR, Score, Approval. Not clone-proof: Plurality, Borda, Minimax, Copeland, Kemeny–Young. (Same data as the "Clone-indep." column in §3's table.)

---

## 4. Practical considerations for 7 candidates

### 4.1 Cognitive load

For an election with 7 well-described candidates, full ranking is feasible but tiring. Score/STAR ballots let voters express "I have no opinion on these three" by giving them the same score, which honest rankings cannot. Pairwise (21 comparisons) is decision-quality but high-friction.

### 4.2 Tallying complexity at n = 7

At n = 7, computational cost is essentially irrelevant on modern hardware. Every method considered here — including brute-force Kemeny–Young (5,040 permutations × 21 pairs ≈ 100 k operations) — finishes in milliseconds for any plausible electorate. Schulze's Floyd–Warshall, Ranked Pairs' sort-and-lock, IRV's ≤6 elimination rounds, and Borda's weighted sum are all trivial. The historical objection to Kemeny–Young on computational grounds is moot in the single-winner small-n regime.

### 4.3 Tie-breaking

At small n with thousands of ballots, exact ties are unlikely but possible. A deterministic, ballot-independent tiebreaker is needed (lexicographic candidate ID, prior round of voting, random seed published in advance). Schulze and Ranked Pairs have well-documented tiebreak procedures (TBRC: Tie-Breaking Ranking of the Candidates derived from a random ballot). Majority Judgment ties are common and require the published "majority gauge" sub-rule.

### 4.4 Auditability and explainability

The explanation a non-expert voter must accept:

- **Approval**: "Whoever the most people said yes to wins." ★★★★★
- **Score / STAR**: "Add up the stars. STAR: then between the top two, whoever more voters scored higher wins." ★★★★
- **IRV**: "Eliminate the last-place candidate and re-count, repeat." ★★★★
- **Ranked Pairs**: "List every head-to-head match strongest-margin first; lock in winners unless that would create a cycle." ★★★ (concrete, but cycle-check is unfamiliar)
- **Schulze**: "Whoever has the strongest chain of victories." ★★ (rigorous explanation requires the beatpath concept)
- **Majority Judgment**: "Whoever has the highest median grade." ★★★★
- **Borda**: "First place is worth 6 points, second 5, etc.; highest total wins." ★★★★★
- **Kemeny–Young**: "The overall ranking that disagrees with the fewest voter-pair-judgments wins." ★★★

Borda is the most explainable but the most strategically broken; STAR is the best simplicity/correctness trade-off. Ranked Pairs is more explainable than Schulze for the same criterion compliance.

---

## 5. Recommendation

The choice reduces to two things voters and organisers actually experience: **(a) the ballot format**, and **(b) the social-choice properties of the rule** (Condorcet compliance, monotonicity, strategic resistance, explainability). At n = 7, computational cost is uniform across methods. Three frontrunners emerge.

### Rank 1 — **STAR voting**

**Why.** STAR's 0–5 ballot is the most ergonomic for a 7-candidate field: voters score independently, may treat indifferents identically, and don't need to manufacture distinctions they don't feel. Empirically, STAR has the lowest strategic-payoff-to-backfire ratio across Monte-Carlo simulations of any major method ([Equal Vote](https://www.equal.vote/strategic-star)) — roughly 1:1, versus ~3:1 for IRV and ~18:1 for plurality. The automatic-runoff step recovers most of the discrimination that pure Score loses to min-max strategy.

**Costs.** Fails Condorcet (rarely impactful in real elections, since the failure requires the Condorcet winner to fail to reach the top two by score), monotonicity (in pathological constructed examples), and later-no-harm.

### Rank 2 — **Ranked Pairs (Tideman)**

**Why.** If Condorcet correctness is prized — and the normative claim "beats every other candidate head-to-head" is hard to argue against — Ranked Pairs offers strong overall criterion compliance (clone independence, monotonicity, reversal symmetry, Smith) and is widely considered the "best" Condorcet method ([Munger 2022, _Constitutional Political Economy_](https://link.springer.com/article/10.1007/s10602-022-09382-w)). It is more intuitively explainable than Schulze: process head-to-head matches in margin order, locking each in if it doesn't contradict earlier locks.

**Costs.** Requires a ranked ballot (heavier than scoring for the voter). Fails participation and later-no-harm (consequences of Condorcet compliance per Moulin's theorem). Tiebreaker details require care.

### Rank 3 — **Schulze**

**Why.** Same criterion profile as Ranked Pairs in almost every case (clone-free, monotonic, reversal-symmetric, Smith-efficient, Condorcet). The most-deployed Condorcet method in the world — Wikimedia, Debian, KDE, FSF-E, Pirate Parties, Ubuntu, Gentoo — so a deep well of implementations, documentation, and battle-tested edge cases is available.

**Costs.** Beatpath explanation is harder to convey to lay voters than Ranked Pairs. Schulze and Ranked Pairs return the same winner in the overwhelming majority of real elections, so the choice between them is mostly about explainability and implementer familiarity.

### Runners-up

- **Kemeny–Young** is a defensible alternative to Ranked Pairs / Schulze. It offers the strongest theoretical justification of any Condorcet completion (median ranking, maximum-likelihood interpretation, satisfies reversal symmetry and Smith). It ranks just behind because (i) its winner coincides with Ranked Pairs / Schulze in the vast majority of profiles, (ii) "the ordering minimizing pairwise disagreements" is harder to explain to lay voters than Ranked Pairs' margin-locking story, and (iii) it fails clone independence.
- **IRV** is disqualified on **social-choice grounds**: documented monotonicity failures (Burlington 2009) and Condorcet-winner failures (Alaska 2022) are real, recent events, and the estimated lower bound on monotonicity failure in competitive 3-candidate IRV elections is ~15%. Where the absence of "raising your favorite hurt them" outcomes matters for legitimacy, IRV's failure modes are too embarrassing.
- **Approval voting** deserves an honorable mention as the simplest method that is not actively broken. If voter UX must be radically minimal, Approval is the right choice. It does not handle the chicken-dilemma case as gracefully as STAR.
- **Avoid plain Borda** — the literature on burying and clone attacks is unanimous ([Wikipedia — Borda](https://en.wikipedia.org/wiki/Borda_count)).
- **Majority Judgment** is plausible where verbal grades and deliberation about quality are valued; it satisfies IIA-on-grades but has known median-tyranny pathologies and fails the majority criterion.

### Closing note

If a single recommendation is required: **STAR** for the combination of UX, strategy resistance, and explainability. Because STAR ballots can be reinterpreted as ordinal preferences (rank-by-score, with ties allowed), the same ballots can be re-tallied with Ranked Pairs, Schulze, or Kemeny–Young as a parallel audit artifact with zero additional voter burden. If STAR's winner and the Condorcet winner ever diverge, the divergence is transparent and the choice can be revisited.

This recommendation assumes **secret ballots**. If ballots are immediately public — every voter can see every cast ballot in real time — both the threat model and the ranking change. See §6.

---

## 6. Public-ballot setting

The recommendation above implicitly assumes secret ballots (or at least ballots not visible until the voting period closes). If ballots are **immediately public** — visible to anyone, in real time, as soon as they are cast — the threat model expands and the relative attractiveness of aggregation rules shifts.

### 6.1 New threats introduced by public ballots (rule-agnostic)

Public ballots reintroduce categories of attack that the secret-ballot tradition (Australian ballot, 1856) was invented to defeat. None of these are fixable by choosing a different aggregation rule.

1. **Coercion and vote-buying become enforceable.** An employer, landlord, party boss, spouse, or coercive group can demand a specific vote and verify compliance. Vote-buying contracts become executable ("pay on proof of vote"). This is the single strongest historical argument for ballot secrecy.
2. **Bandwagon and cascade effects.** Visible running totals influence later voters. Even small early leads can compound. Documented empirically in any real-time public-tally system.
3. **Last-mover advantage.** Late voters have strictly more information than early ones. A coordinated late-voting bloc can act as a decisive swing faction.
4. **Chilling effects on preference expression.** Even without explicit coercion, voters self-censor under social and professional pressure — voting what is _safe to be seen voting for_, not what they prefer.
5. **Perfect-information strategic voting.** Strategy stops being a guess about other voters' behavior. It becomes a solvable game, often with a determinable optimal play.

### 6.2 How each method's strategy resistance changes

The key distinction is whether a method's strategy resistance derives from voters' **uncertainty** about each other (which public ballots eliminate) or from **structural** properties of the rule (which they do not).

| Method                     | Effect of public ballots on strategy resistance                                                                                                                                                                                                                                                                                                                                                  |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Plurality**              | Severely worse. Bandwagon dominates; late voters last-mover-coordinate on the top two.                                                                                                                                                                                                                                                                                                           |
| **Approval**               | Chicken dilemma becomes a full-information chicken game — late voters dominate.                                                                                                                                                                                                                                                                                                                  |
| **Score / Range**          | Already collapses to Approval under strategy; public ballots accelerate the collapse.                                                                                                                                                                                                                                                                                                            |
| **STAR**                   | **Partially weakened.** STAR's structural defense relies on voters not knowing which two candidates will reach the top-two runoff. Under public ballots that uncertainty evaporates: once the top two pull away in real time, score-bulleting becomes safe and burying becomes targetable. The runoff phase itself (no further voter input) is unaffected, but the score-phase game gets easier. |
| **IRV**                    | Worse. Tactical truncation and ordering manipulation become easier with visibility into who is about to be eliminated. Center-squeeze becomes exploitable in real time.                                                                                                                                                                                                                          |
| **Borda**                  | Catastrophic. Favorite-first, strongest-rival-last was already optimal under uncertainty; with full information, burying is perfectly targeted and clone-teaming attacks can be coordinated live.                                                                                                                                                                                                |
| **Schulze / Ranked Pairs** | **Largely holds.** Condorcet winners are intrinsically hard to dislodge by strategic voting — overturning A's pairwise victory over B requires coordinated burying by a near-majority faction, which is the same threshold whether ballots are public or private. Burying is more efficient with information, but the criterion-level robustness survives.                                       |
| **Kemeny–Young**           | Similar to Schulze / Ranked Pairs for the winner; the full ranking is more manipulable.                                                                                                                                                                                                                                                                                                          |
| **Majority Judgment**      | **Most robust.** The median is intrinsically strategy-resistant: a single ballot only moves the outcome if it crosses the median grade. Public information does not change this — it is a property of the statistic, not of voter uncertainty. Balinski & Laraki's case for MJ is at its strongest in transparent-ballot settings.                                                               |

### 6.3 Revised ranking under public ballots

The headline ranking changes. STAR's lead in the secret-ballot setting came largely from its near-1:1 strategic-backfire ratio in simulations — but those simulations assume voters cannot see each other's ballots. With that assumption gone, STAR's structural defense weakens.

A more defensible ranking when ballots are immediately public:

1. **Schulze or Ranked Pairs** — structural Condorcet robustness does not depend on voter uncertainty. Manipulation thresholds remain at "majority faction must coordinate burying."
2. **Majority Judgment** — moves up substantially. The median statistic is the rare aggregator that is intrinsically strategy-resistant; public ballots barely weaken it.
3. **STAR** — still better than IRV, Borda, or Plurality, but its score-phase strategy game becomes much easier with full information.

### 6.4 The bigger question

Whether ballots _should_ be public is a more fundamental question than which aggregation rule to use. The coercion, vote-buying, and chilling-effects problems in §6.1 are **not solved by any tally rule** — they are solved (if at all) by ballot secrecy, or by deciding that the election context is one in which those threats do not apply (small high-trust groups, internal organizations, deliberative bodies whose members are publicly accountable for their votes, transparency-prioritised governance settings).

A useful middle ground that several systems adopt is **commit–reveal voting**: voters publish a cryptographic commitment to their ballot during the voting period; ballots are revealed only after the period closes, simultaneously. This eliminates bandwagon, last-mover advantage, and most cascade effects while preserving end-state auditability. It does **not** solve coercion or vote-buying if reveals are linkable to identity — those still require ballot secrecy or coercion-resistant cryptography. If the design constraint is "tally must be publicly auditable" rather than "ballots must be publicly visible during voting," commit–reveal captures the auditability benefit without the strategic-game and cascade costs.

### 6.5 Commit–reveal on Cardano, and the forced-reveal problem

The commit phase is straightforward in Plutus/Aiken: the commit datum stores `H(ballot || nonce)`, the reveal transaction supplies the preimage, and the validator checks the hash using cheap built-in primitives (Blake2b, SHA-256, Keccak). The harder problem is **forced reveal** — preventing voters from selectively withholding their reveal once they observe how other reveals are trending, which would reintroduce the very last-mover advantage commit–reveal is meant to eliminate. Cardano-feasible mitigations, ordered by strength:

- **Bond / slashing** (weak). Collateral locked at commit time is forfeited (burned or sent to a treasury) on non-reveal. Discourages but does not prevent selective abstention — a sufficiently motivated voter can pay the bond.
- **Default-ballot via relayer** (medium). After the reveal window, any third party can submit a transaction that defaults an unrevealed commit to a null ballot, incentivized by a bounty paid from the forfeited bond. Non-reveal becomes equivalent to casting the default ballot. The voter still has a tactical choice between "my committed vote" and "the default," but the full last-mover advantage is removed.
- **Threshold encryption with a trustee committee** (strong). Ballots are encrypted to a t-of-n public key held by trustees (DReps, SPOs, or a dedicated election committee). After the voting window closes, trustees publish their decryption shares and anyone reconstructs the plaintext ballots. Voters do not control reveal. Requires an honest threshold of trustees and an off-chain coordination protocol; the chain holds the ciphertexts and verifies the shares.
- **Timelock encryption (Drand `tlock`)** (strongest). Ballots are encrypted under a Drand "League of Entropy" public key whose decryption material is broadcast at a specific future round. After that round anyone can decrypt; before it, no one can. The voter has no reveal-time choice — decryption is a function of time, not voter action — and no trustee committee is required beyond Drand itself. The chain stores ciphertexts as datums; the tally is performed off-chain after the target Drand round publishes. §6.6 examines the on-chain feasibility of this in detail.

For genuinely forced reveal, **Drand timelock encryption is the cleanest option available today** and is the recommended primary mechanism on Cardano, with classic commit–reveal retained as a recoverable fallback in case of beacon outage. Threshold encryption with a trustee committee is the next-best alternative and avoids the external dependency on Drand, at the cost of a governance question about trustee selection.

### 6.6 Is Drand `tlock` on Aiken actually feasible? A detailed look

The §6.5 recommendation deserves scrutiny, because Cardano's pairing builtins have a specific limitation that determines what kind of tlock construction is possible.

**Available primitives.** Plutus V3 (CIP-0381) exposes BLS12-381 builtins that Aiken surfaces via `aiken/crypto/bls12_381`: `G1`/`G2` `add`, `scalar_mul`, `neg`, `equal`, `compress`, `uncompress`, `hash_to_group` (RFC 9380, configurable DST); and the pairing as `miller_loop : G1 → G2 → MlResult`, `mul_ml_result`, and `final_verify : MlResult → MlResult → Bool`.

**The limitation that matters.** Plutus exposes the pairing only as an _equality oracle_. You can check `e(A,B) == e(C,D)` via `final_verify`, but you cannot obtain the GT field element value of `e(A,B)`. This is fine for BLS signature verification but a problem for IBE decryption.

**Why it matters for `tlock`.** Drand's tlock is Boneh–Franklin IBE on BLS12-381. Decryption with the round signature `σ_R` looks like

```
M = V ⊕ H'( e(σ_R, U) )
```

where `H'` is a KDF over the GT element. _Computing the mask requires the GT element value_, which Plutus cannot return. There is no clever rearrangement that lets `final_verify` rescue this: the KDF input is the pairing output itself, not an equation between pairings.

**Consequence for validator design.**

| Goal                                                                                               | Feasible in Aiken today?                                                                          |
| -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Voters submit time-locked ballots; tally is computed off-chain after the Drand round               | **Yes**, trivially — the validator just stores ciphertexts as datums; no BLS ops needed on-chain. |
| Hash-commit on-chain with tlock as a forced-reveal backup                                          | **Yes** — Aiken verifies a cheap Blake2b hash on reveal; tlock handles silent voters off-chain.   |
| Validator verifies on-chain that a revealed plaintext is the genuine tlock decryption of its datum | **No** — requires extracting the GT element to recompute the KDF, which Plutus does not expose.   |
| Use Drand beacons that are not on BLS12-381 (e.g. the legacy BN254 default beacon)                 | **No** — wrong curve; Plutus has no BN254 builtins. Must target a BLS12-381 beacon (quicknet).    |

The crucial observation is that **on-chain validation of decryption is not needed** for an election whose tally is off-chain anyway. The forced-reveal property comes from the cryptography, not from the validator: once Drand publishes `σ_R`, every ciphertext targeted at round `R` is decryptable by anyone (auditors included), and the voter has no opportunity to back out. The validator's only job is to make sure each eligible voter submitted exactly one ciphertext during the commit window.

**Recommended pattern.** Combine a Blake2b hash commitment with a tlock ciphertext for the same `(ballot, salt)` pair:

- _Happy path_: the voter reveals `(ballot, salt)` on-chain after the deadline; Aiken verifies the hash. Cheap, no BLS work on-chain.
- _Sad path_: the voter goes silent; the off-chain tallier decrypts the tlock ciphertext using `σ_R` and audits that it matches the hash. Forced reveal is guaranteed cryptographically; the validator never needs to verify decryption.

**Practical details that must line up.** Even in the off-chain-tally model, several Drand-specific parameters have to match or the system breaks:

1. **Curve.** Target a BLS12-381 beacon (Drand's `quicknet` is current). The legacy default beacon is BN254 and is unusable on Cardano.
2. **Group placement.** `quicknet` uses signatures on G1 and public key on G2; tlock places the round identity `H(round)` on G1 and `U = g₂^r` on G2. Plutus supports `hash_to_group` on both groups, so this is fine.
3. **Hash-to-curve DST.** Plutus's `hash_to_group` follows RFC 9380 with a caller-supplied DST. The DST passed when reconstructing a round identity off-chain (and any time the validator touches `hash_to_group`) must exactly match Drand's per-network DST (e.g. `BLS_SIG_BLS12381G1_XMD:SHA-256_SSWU_RO_NUL_` for quicknet). Easy to get wrong, easy to test.
4. **Serialization.** Drand publishes compressed group elements (48 bytes G1, 96 bytes G2); Plutus has `uncompress`. Comfortably within transaction limits.
5. **Round timing.** Drand `quicknet` produces a beacon every 3 seconds since genesis. Pick `R = (deadline_unix − genesis) / period`. Cardano slots and Drand rounds are independent — the deadline is wall-clock-based.
6. **Beacon availability.** If the League of Entropy fails to publish `σ_R`, ciphertexts for that round cannot be decrypted and the election cannot be tallied. Mitigations: keep the target round close, optionally encrypt to a chain of consecutive rounds, and retain the hash-commit path so honest voters can still reveal manually.
7. **Trust assumption.** tlock's confidentiality before round `R` depends on the LoE threshold (currently ~9 nodes, threshold 6). Early collusion implies early decryption. Worth disclosing to voters; not a flaw, but not "cryptographically unconditional" either.
8. **Griefing via malformed ciphertexts.** A voter can post bytes that do not decrypt to a valid ballot. The validator can cheaply enforce well-formedness (call `uncompress` on the ciphertext components, which includes subgroup checks) to make on-curve membership a precondition of acceptance; semantic validity of the plaintext is policed off-chain by the tally rule.
9. **Costs.** Per ciphertext on-chain: zero pairings, at most a few `uncompress` calls. Per-voter datum: roughly 150–250 bytes. Comfortable within Cardano execution budgets.

**Verdict.** Drand-based timelock voting is genuinely feasible on Aiken **for the off-chain-tally model an election needs**. It is _not_ feasible to have a validator decide on-chain whether a particular reveal is the honest tlock decryption of its commitment — that would require a GT-element extraction primitive Plutus does not provide. For single-winner elections this is not a real limitation; the cryptographic forced-reveal property holds regardless, and the off-chain tallier (along with any auditor) can verify decryptions independently.

---

## Sources

- [electowiki — Table of voting method criteria](https://electowiki.org/wiki/Table_of_voting_method_criteria)
- [electowiki — STAR voting](https://electowiki.miraheze.org/wiki/STAR_voting)
- [electowiki — Tactical voting](https://electowiki.org/wiki/Tactical_voting)
- [Wikipedia — Comparison of voting rules](https://en.wikipedia.org/wiki/Comparison_of_voting_rules)
- [Wikipedia — Schulze method](https://en.wikipedia.org/wiki/Schulze_method)
- [Wikipedia — Kemeny–Young method](https://en.wikipedia.org/wiki/Kemeny%E2%80%93Young_method)
- [Wikipedia — Majority judgment](https://en.wikipedia.org/wiki/Majority_judgment)
- [Wikipedia — Borda count](https://en.wikipedia.org/wiki/Borda_count)
- [Wikipedia — Instant-runoff voting](https://en.wikipedia.org/wiki/Instant-runoff_voting)
- [Equal Vote Coalition — STAR criteria](https://www.equal.vote/criteria)
- [Equal Vote Coalition — Strategic STAR](https://www.equal.vote/strategic-star)
- [Equal Vote Coalition — Q&A with Common Cause](https://www.equal.vote/q_and_a_common_cause)
- [STAR Voting — Majority FAQ](https://www.starvoting.org/majority)
- [Center for Election Science — Approval voting](https://electionscience.org/library/approval-voting/)
- [Schulze, _The Schulze Method of Voting_ (arXiv:1804.02973)](https://arxiv.org/pdf/1804.02973)
- [Munger, _The best Condorcet-compatible election method: Ranked Pairs_ (Constitutional Political Economy, 2022)](https://link.springer.com/article/10.1007/s10602-022-09382-w)
- [Miller, _Monotonicity Failure in IRV Elections with Three Candidates_ (UMBC)](https://userpages.umbc.edu/~nmiller/MONFAILURE.R2.NRM.pdf)
- [Graham-Squire & Zayatz, _RCV and the Center Squeeze in Alaska 2022_ (arXiv:2303.00108)](https://arxiv.org/html/2303.00108v2)
- [rangevoting.org — Schulze beatpath explanation](https://www.rangevoting.org/SchulzeExplan.html)
- [rangevoting.org — Median vs range](https://rangevoting.org/MedianVrange.html)
- [CIVS — Condorcet Internet Voting Service completion algorithms](https://civs1.civs.us/rp.html)
- [Mathematics & Democracy Institute — Empirical analysis of RCV methods](https://mathematics-democracy-institute.org/empirical-analysis-of-ranked-choice-voting-methods/)
- [arXiv:2501.00618 — Borda count variations on RCV data](https://arxiv.org/html/2501.00618v1)
- [Springer SCW — Borda variations (2025)](https://link.springer.com/article/10.1007/s00355-025-01638-2)
- [Wang & Cuff (Princeton) — Condorcet and strategic voting (PDF)](https://www.princeton.edu/~cuff/publications/wang_strategic_voting.pdf)
- [arXiv:2411.18790 — Fast Schulze voting using Quickselect](https://arxiv.org/pdf/2411.18790)
- [arXiv:1911.08774 — Fast self-tally voting protocol](https://arxiv.org/pdf/1911.08774)
- [FairVote — Comparing single-winner voting methods](https://fairvote.org/resources/electoral-systems/comparing-voting-methods/)
- [Balinski & Laraki, _Majority Judgment_, MIT Press](https://mitpress.mit.edu/9780262545716/majority-judgment/)
