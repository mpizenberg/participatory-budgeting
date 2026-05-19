# Grant Funding: State of the Art in Literature and Practice (2025–2026)

A survey of grant-funding mechanisms — what works, what fails, what is being tried, and what tooling exists to implement it. The shared backdrop: traditional peer-reviewed grants are widely criticized for low hit rates (often <20%), enormous applicant overhead, conservatism, and bias ([Oxford — peer review flaws][peer-review-flaws], [F1000 — bias, burden, conservatism][bias-burden-conservatism]). The 2025–2026 frontier is a _portfolio_ of mechanisms, each attacking a different failure mode.

---

## 1. Landscape

### 1.1 Failure-mode map

| Failure mode of traditional grants            | Mechanism that addresses it                    |
| --------------------------------------------- | ---------------------------------------------- |
| Reviewer overhead, false precision in ranking | Modified lottery                               |
| Slow decisions, application burden            | Fast grants, micro-ARPAs                       |
| Risk aversion, no portfolio management        | ARPA / FRO models                              |
| Hard to pre-judge value                       | Prizes, AMCs, RetroPGF                         |
| Public-goods underprovision                   | Quadratic funding, RetroPGF                    |
| Sustaining maintainers, not just creators     | Direct streaming (Drips, GitHub Sponsors, STF) |

### 1.2 Push vs. pull: the canonical taxonomy

A _grant_ is a non-repayable transfer of resources from a funder to a recipient, conditional on the recipient pursuing a stated purpose. Grants sit alongside three other instruments in the standard taxonomy of public-goods finance:

- **Grants** ("push" funding) — pay for _inputs_ (a research plan, a staffed team), released before outputs exist.
- **Contracts** — pay for _deliverables_ specified in advance; the funder owns the work product.
- **Prizes and inducement awards** ("pull" funding) — pay for _outputs_ only, after a target is met ([Luminary Labs][luminary-grants-prizes], [Rethink Priorities][rethink-prizes]).
- **Advance Market Commitments (AMCs)** — a hybrid: a binding commitment to _purchase_ a future product at a guaranteed price if it meets pre-set specs ([Wikipedia — AMC][amc-wiki], [Belfer Center][belfer-amc]).

Grants dominate because the funder usually cannot specify the deliverable precisely ex ante — they want the recipient's judgment about _what_ to produce. Azoulay & Li (NBER w26889) argue grants dominate when work is exploratory and produces large spillovers, prizes when the target is well defined, contracts when the funder knows exactly what they want ([NBER][nber-azoulay]).

### 1.3 Mechanisms in brief

- **Peer-reviewed grants** — the default for ~50 years across NIH, NSF, ERC, and most national funders.
- **Modified lotteries** — peer review filters to a "fundable" pool, winners drawn randomly. Adopted by SNSF (Switzerland), NZ Health Research Council, Volkswagen Foundation, FWF (Austria) ([SNSF Bayesian + lottery][snsf-lottery], [Nature — Swiss funder draws lots][swiss-funder-draws-lots], [DORA — lotteries][dora-lotteries]).
- **Fast / lean grants** — short application, small panel, decisions in days. Fast Grants (~$50M for COVID research) is the proof point; Emergent Ventures continues the pattern; FAS's "micro-ARPA" proposal generalizes it ([FAS micro-ARPA][micro-arpa]).
- **ARPA-style program managers** — DARPA, ARPA-E, ARPA-H, UK ARIA. PM-driven portfolios with active management rather than passive peer review ([ARPA-H programs][arpa-h-programs]).
- **Focused Research Organizations (FROs)** — time-limited startup-like nonprofits for mid-scale science (open datasets, infrastructure). Championed by Convergent Research.
- **Prizes & AMCs** — pay on outcomes. X-Prize / DARPA Grand Challenges and the 2009 pneumococcal AMC are the canonical cases; Frontier Climate's >$1B CDR commitments are a current example.
- **Quadratic funding (QF)** — matching pool amplifies many small donations more than few large ones. Largest deployment: Gitcoin Grants ([Gitcoin QF][qf-gitcoin]).
- **Retroactive Public Goods Funding (RetroPGF)** — pay after demonstrated impact. Optimism has distributed >$100M across multiple rounds ([Optimism RetroPGF intro][retropgf-origin]).
- **DAO grant programs (milestone-based)** — Arbitrum, Uniswap, Ethereum Foundation, ENS, Cardano Catalyst.
- **DeSci / IP-NFT grants** — VitaDAO, Molecule-mediated DAOs; tokenized research IP with upside claim.
- **Futarchy / prediction-market funding** — conditional markets pick highest-expected-value option (early stage).
- **Direct & continuous funding** — GitHub Sponsors, Open Collective, Drips, Liberapay, Germany's Sovereign Tech Fund.

---

## 2. Peer review: deep dive

The dominant model — institutionalized by NIH, NSF, ERC, Wellcome, and most foundations — is: open call → written application → external peer review → panel deliberation → ranked funding line. NIH allocated ~80% of its ~$39B budget through this in 2019; in 2015 NSF engaged 16,255 scientists to evaluate 51,588 proposals at ~3.9 hours each ([PMC — troubles with peer review][pmc-troubles]). The ERC distributed €1.8B for medical research in 2017 through a two-stage variant ([PMC — peer review realist synthesis][pmc-realist]).

### 2.1 What it does well

- **Domain calibration** — expert panels can spot proposals that misstate prior art or methodology.
- **Legitimacy** — applicants and the public broadly accept "experts judged it."
- **Defensible audit trail** — scores, justifications, and rebuttals create something funders can defend politically.

### 2.2 Failure modes

- **Low reliability.** Independent panels reach materially different conclusions on the same proposals. Inter-rater agreement is only modestly above chance for proposals in the "fundable" middle band — exactly where decisions get made ([NHMRC analysis via mBio][mbio-lottery], [PMC — grant peer review in health][grant-peer-review-health]). An NHMRC analysis found that **59% of funded grants could have missed funding through random variability in scoring alone**.
- **Weak predictive validity.** Peer review scores are at best a weak predictor of subsequent research output / citations. Top-ranked rejected proposals often outperform funded ones ([PMC — ranking vs rating][ranking-vs-rating]).
- **Conservatism bias.** Novel proposals score systematically lower, even controlling for feasibility. Panels focus on faults over potential ([F1000][bias-burden-conservatism], [Oxford][peer-review-flaws]).
- **Demographic bias.** Higher scores correlate with prior funding and h-index; lower scores correlate with female applicants and applied-science researchers. The NSF and Villum Foundation both demonstrated in controlled studies that blinding reviewers to applicant identity changed funding outcomes — proposals that would otherwise have been rejected were funded ([PMC — troubles with peer review][pmc-troubles]).
- **Massive overhead.** Application + review burden is estimated at billions of researcher-hours annually. At 15–20% hit rates, most of that work is wasted from an allocation standpoint. Some authors treat both writing and reviewing low-payline proposals as "questionable research practices" because the expected return on the writing effort is dominated by noise in review ([PMC — QRP][pmc-qrp]).
- **Strategic behavior.** Proposals optimize for reviewer legibility rather than scientific merit.

### 2.3 Current frontier

- **Modified lotteries** — SNSF, NZ HRC, Volkswagen Foundation, FWF. Bayesian ranking identifies the band where reviewer disagreement makes ranking meaningless, then randomizes within it ([SNSF][snsf-lottery], [Nature Comm 2025 — lottery before peer review][lottery-before-peer-review], [Research on Research blog 2025][ror-randomizing-2025]).
- **Internal review only** (ARPA-H, DARPA, ARIA model) — empirical evidence that _external_ peer review improves on internal expert PMs is weak. Several metascience researchers argue this is the highest-leverage reform ([Maximum Progress — clearest metascience reform][clearest-metascience-reform]).
- **Editorial preregistration + grant review** — a Nov 2025 arXiv proposal couples journal acceptance to grant funding to align incentives ([arXiv 2511.01439][preregistration-grant-review]).
- **Two-stage processes** (LOI → invited full proposal) to cut applicant burden — used by ERC, NSF, Wellcome.
- **Blinded review and narrative CVs** — to reduce identity-based shortcuts.
- **Collective allocation.** Bollen et al. propose every funded researcher receive a baseline and be required to redistribute a fraction to peers; aggregated transfers replace a central panel ([PMC — From funding agencies to scientific agency][pmc-bollen]).
- **Bibliometric ex-post evaluation.** Standard but unstable as a single metric; consensus is to use multiple indicators ([Springer — Measuring impact][springer-impact], [HSLS LibGuide][hsls-impact]).

### 2.4 Applicant attitudes to reform

Surveys of researchers show majority _opposition_ to pure randomness but majority _acceptance_ of hybrid peer-review-plus-lottery designs ([Research Integrity & Peer Review 2019][rpr-acceptability], [Science & Public Policy 2022][spp-survey]).

### 2.5 Honest assessment

Peer review works for the top ~5% and bottom ~30% of proposals; it adds noise in the middle, and that's where lotteries belong. As of early 2025, ~a dozen funders worldwide have implemented some form of funding by lottery ([Oxford — lottery taxonomy][oxford-lottery-taxonomy]); SNSF extended randomization to _all tiebreaker cases across its entire ~CHF 880M grant portfolio_ in late 2021.

---

## 3. Pull mechanisms: prizes and AMCs

### 3.1 Inducement prizes

Inducement prizes pay only on success. The **Ansari X Prize** (2004, $10M) attracted 26 teams that collectively spent ~$100M — a 10× R&D multiplier — and the winning vehicle became the basis for Virgin Galactic ([Rethink Priorities][rethink-prizes]). DARPA's Grand Challenges seeded the autonomous-vehicle industry through prizes that paid no one in 2004 and $2M in 2005.

Empirical evidence is mixed: prizes shine when the target is _crisp and verifiable_ and underperform when the goal is fuzzy or success criteria can be gamed ([SSIR — Prizes and Challenges][ssir-prizes]).

### 3.2 Advance Market Commitments

The canonical AMC is the 2009 pneumococcal vaccine commitment: five donor countries and the Gates Foundation pledged $1.5B to top-up payments per dose conditional on efficacy and pricing terms. By 2020, ~150M children had been immunized. Operation Warp Speed used AMC-like structures for COVID vaccines ([Wikipedia][amc-wiki], [Belfer Center][belfer-amc]).

AMCs are a _purchase_ commitment, not a grant — but they sit in the same ecosystem because they solve the same problem grants do (under-provision of public goods) with a different incentive structure. Frontier Climate's >$1B carbon-removal AMC is the current poster child outside health.

### 3.3 Bounties

The web3 cousin of prizes. A bounty is a small, narrowly-scoped pull payment ("find a bug in this contract", "implement this feature spec"). Bounty platforms (Gitcoin Bounties, ImmuneFi for security) are the standard way DAOs fund discrete tasks too small for a grant.

---

## 4. ARPA-style and FRO models

A distinct school in metascience holds that the most important reform is not mechanism design around peer review but _abandoning_ external peer review for high-risk work in favor of empowered program managers.

- **DARPA / ARPA-E / ARPA-H / UK ARIA.** A small number of PMs hold portfolios; they identify problems, recruit performers, set milestones, and kill underperforming projects. External review is light or absent. ARPA-H's program list is a useful index of the model in action ([ARPA-H][arpa-h-programs]).
- **Fast Grants** (Patrick Collison & Tyler Cowen, 2020) — ~$50M deployed to COVID research with 48-hour decisions. Demonstrated that a small panel of trusted scientists can decide faster and at least as well as conventional review for time-critical work.
- **Emergent Ventures** — Cowen's continuing program; small grants, rapid decisions, broad mandate.
- **Micro-ARPAs** — a Federation of American Scientists proposal generalizing the pattern: small, agile, mission-focused programs run by accountable PMs ([FAS][micro-arpa]).
- **Focused Research Organizations (FROs)** — Convergent Research's model: a time-limited (~5-year) nonprofit company built around a specific mid-scale scientific deliverable (e.g., an open dataset, a measurement tool). Sits between a lab grant and a startup.

---

## 5. Quadratic funding: deep dive

A matching pool amplifies many small donations more than a few large ones — formally, the match for a project scales with the square of the sum of square-roots of individual contributions:

$$M_p = \left( \sum_i \sqrt{c_{p,i}} \right)^2$$

Theoretical justification: Buterin, Hitzig & Weyl (2018) show QF approximates a Lindahl-optimal allocation for public goods _given honest, non-colluding contributors with verified identities_ ([Liberal Radicalism][liberal-radicalism]). Theoretical extensions: Pasquini on matching-funds requirements ([arXiv 2010.01193][arxiv-qf-pasquini]); optimal allocation under limited funds ([arXiv 2207.14775][arxiv-qf-optimal]).

### 5.1 What it does well

- **Aggregates preference signal** from a community without putting any single person in charge.
- **Surfaces socially validated, well-understood work.** Gitcoin retrospectives (GG21, GG23, GG24) report QF reliably highlights mid-stage user-facing infrastructure — dashboards, explorers, analytics, SDKs ([GG24 interop retro][gg24-interop], [GG23 retro][gg23-retro]).
- **Discovery.** New entrants with broad community support can outperform incumbents with one big donor.
- **Scale.** Gitcoin has distributed >$60M to thousands of projects through QF rounds; EIP-1559 funding is a frequently cited example where QF legitimized critical infrastructure work ([Gitcoin][qf-gitcoin]).

### 5.2 Failure modes

- **Sybil attacks are the central problem.** Splitting $100 across 10 fake accounts produces a much higher match than $100 from one account. Most engineering effort since 2019 has been anti-Sybil (Gitcoin Passport, BrightID, Proof of Humanity, biometric attestation). None are fully solved; all create UX friction and exclude legitimate participants ([Dora — QF v2 anti-Sybil][qf-v2-antisybil]).
- **Collusion is harder than Sybil resistance.** Real humans coordinating a bribe ("I'll send $1 to your project if you send $1 to mine") look identical to genuine community support. Buterin/Weyl explicitly flag this as the deepest open problem ([Liberal Radicalism][liberal-radicalism]).
- **Plutocracy in disguise.** When matching pools are large, a wealthy actor can fund 1000 small donations through accomplices and dominate the round.
- **Underperforms for invisible work.** The GG24 retrospective found QF "underperformed for standards, OIF tooling gaps, and invisible coordination layers" — anything community members don't directly _see_ gets underfunded ([GG24][gg24-interop]).
- **Attention dynamics.** Marketing, Twitter presence, and round timing dominate outcomes.
- **Round cadence.** Discrete rounds create boom/bust funding rather than predictable income.

### 5.3 Frontier variants

- **COCM (Connection-Oriented Cluster Matching)** — discounts contributions from already-connected donors to reward independent signal. Used in Gitcoin's Climate Solutions rounds.
- **Pairwise QF / pairwise-bounded matching** — caps match based on pairwise coordination between donor pairs.
- **Tunable QF (TQF)** — round operator weights certain donor classes (e.g., domain experts) higher.
- **Threshold funding** — projects need to clear a minimum to receive anything.
- **MACI (Minimum Anti-Collusion Infrastructure)** — encrypted votes that make bribery unverifiable, so bribes can't be enforced.

### 5.4 Honest assessment

QF is a real innovation for a narrow case: many small, _visible_ public goods with a verified community. It is not a general-purpose grant mechanism. Outside crypto, deployments have been rare because (a) Sybil resistance is hard without web3 identity primitives and (b) somebody still has to fund the matching pool.

---

## 6. Retroactive Public Goods Funding: deep dive

Pay for impact _after_ it's been demonstrated, rather than predicting which proposals will produce impact. Originally formalized by Buterin for Optimism. The thesis: "It's easier to agree on what _was_ useful than what _will be_" ([Optimism RetroPGF intro][retropgf-origin]).

### 6.1 What it does well

- **Eliminates pre-judgment error.** No reviewer has to predict the future. Truly novel work that wouldn't have scored well prospectively can win retroactively.
- **Aligns funder/builder incentives.** Builders ship first, get paid later — closer to how markets work.
- **Captures emergent value.** A library that became critical only after others built on it can be recognized.
- **Scale demonstrated.** Optimism has distributed >$100M across multiple rounds to libp2p, IPFS, Protocol Guild, Ethereum tooling, with ~$1.3B in reserve ([Unchained — what is RetroPGF][unchained-retropgf]).
- **Impact-certificate markets.** Hypercerts allow prospective funders to be reimbursed by retro funders later — effectively a futures market for impact.

### 6.2 Failure modes (well-documented after 5 Optimism rounds)

- **The "results oracle" problem is unsolved.** Somebody still has to decide what counted as impact. Optimism uses "badgeholders"; same political dynamics as peer review re-emerge — bias, capture, lobbying, fatigue ([arXiv 2508.16285 — social choice analysis][social-choice-retropgf]).
- **Badgeholder overload.** Hundreds to thousands of projects per round, insufficient information to evaluate; badgeholders skim.
- **Subjective visibility dominates measurable impact.** Round 4 was widely criticized for projects with strong social presence winning over projects with measurable but quiet contributions ([Unchained — name change criticism][retro-funding-name-criticism]).
- **Voting mechanism vulnerabilities.** Quadratic, mean, and median voting schemes each have distinct failure modes; none is dominant ([arXiv 2505.16068 — voting vulnerabilities][voting-vulnerabilities-retro]).
- **Plutocratic capture.** When voting power is token-weighted, the round becomes a function of whales' preferences. Optimism mitigates this with one-badgeholder-one-vote, but badgeholder selection is itself political.
- **Bribery / collusion.** Same MACI-style problems as QF, plus the additional risk that badgeholders themselves get bribed.
- **No retroactive incentive without trust in future rounds.** Builders need to believe future retro rounds will happen. Optimism's 2025 shift away from regular rounds toward "ongoing impact evaluation" reflects exactly this credibility problem ([Optimism — Retro Funding 2025][retro-funding-2025]).
- **Name change** from "Retroactive Public Goods Funding" to "Retro Funding" dropped the "public goods" framing, signaling drift toward rewarding ecosystem marketing rather than commons ([Unchained][retro-funding-name-criticism]).
- **Insufficient evidence of ecosystem causation.** Optimism's own 2025 retrospective: "lacks sufficient evidence that rewards have caused significant ecosystem growth" ([Optimism][retro-funding-2025]).

### 6.3 Frontier

- **Continuous retro funding** (AutoRF, Drips-style) — replace discrete rounds with streaming based on rolling impact metrics, removing campaign dynamics.
- **Programmatic results oracles** — Open Source Observer is building an open-data layer to measure project impact mechanically (commits, downloads, dependents, on-chain usage) so allocation can be partially automated ([OSO retro blog][oso-retro]).
- **Hypercerts** — standardize impact claims so retro funders share evidence across rounds ([Hypercerts][hypercerts]).
- **Hybrid prospective + retroactive** — prospective seed grants give certainty; retro tops up the winners.

### 6.4 Honest assessment

Retroactive funding genuinely solves the "we can't predict winners" problem, but it just moves the hard judgment call from prediction to evaluation. Evaluation is _somewhat_ easier — but only somewhat, and the mechanism still requires either trusted human judges (with all peer review's pathologies) or a credible automated impact oracle (which nobody has built). Its biggest contribution may be conceptual: making "fund what worked" a legitimate alternative to "fund what you think will work."

---

## 7. DAO grant programs, Cardano Catalyst, DeSci, futarchy

### 7.1 DAO grant programs (ex-ante, milestone-based)

The dominant disbursement model in web3 grants is **milestone-based release**: a project commits to deliverables, each milestone unlocks a tranche, a reviewer approves before the next tranche is sent ([Gitcoin — Milestone-Based Funding][gitcoin-milestone], [Lampros Tech][lampros-milestone]). Major programs:

- **Arbitrum Foundation Grant Program** and the community-governed **Arbitrum DAO Grants** (Questbook-administered "domain allocator" model — $250K to $1M per domain across seasons) ([Arbitrum Foundation][arbitrum-foundation-grants]).
- **Uniswap-Arbitrum Grant Program (UAGP)** — $50K–$250K grants for cross-ecosystem builders.
- **Uniswap Foundation** — ~$40M/year grants budget ([Bitget Academy][bitget-daos]).
- **Ethereum Foundation Ecosystem Support Program** — rolling-application grants ([ethereum.org][ethereum-grants]).
- **ENS Public Goods Working Group** — $1.5–3M annually.

The pattern: open application portal, small standing committee or rotating reviewers, KYC/legal compliance, milestone-based payment in the native token. Closer to a corporate developer-relations budget than to academic grantmaking.

### 7.2 Cardano's Project Catalyst

The largest sustained community-voted treasury in crypto. Catalyst distributes ADA from the Cardano treasury through recurring "funds":

1. Community submits proposals against published _challenges_.
2. Community reviewers (paid) score proposals.
3. ADA holders vote on funded proposals (stake-weighted).
4. Winning proposals receive milestone-gated ADA.

Cumulative distribution: **>$150M across ~2,000 funded proposals** ([Project Catalyst][projectcatalyst-site], [Cardano Developer Portal][cardano-dev-portal]). The 2025 transition moves stewardship from IOG to the Cardano Foundation; Fund15 and Fund16 are paused pending handover ([CryptoSlate][cryptoslate-catalyst]).

Catalyst stacks several mechanisms surveyed elsewhere: stake-weighted voting (with the legitimacy issues that brings), community review pools, milestone-based disbursement, and pre-bucketed challenge categories.

### 7.3 DeSci: token-mediated grants for science

- **VitaDAO** — funds longevity research; raised ~$10M; token-holders vote on funded projects. Pfizer Ventures participated ([Yellow Research][yellow-desci], [Molecule — democratizing][molecule-democratizing]).
- **AthenaDAO** (women's health), **ValleyDAO** (synbio/climate), **HairDAO**, **CryoDAO**.
- **Molecule** — infrastructure: tokenizes research IP via _IP-NFTs_; DAOs fund work in exchange for a stake in downstream commercialization rights ([Stanford Law — DeSci II][stanford-desci]).

DeSci funding is small in absolute dollars relative to NIH but novel in mechanism: it transforms the "grant" into an investment-like instrument with an upside claim, and allows non-credentialed, non-affiliated funders to back basic research.

### 7.4 Futarchy and prediction-market funding

Hanson's 2013 proposal — "vote on values, bet on beliefs" — has had a recent revival in DeSci. The idea: stakeholders define a metric (citations, replications, patient outcomes); conditional prediction markets estimate the metric's expected value under each candidate funding decision; the highest-expected-value option is funded. A 2025 _Frontiers in Blockchain_ paper analyzes embedding futarchy in DeSci DAOs ([Frontiers — Futarchy in DeSci][frontiers-futarchy]). Live deployments remain rare; the mechanism is still a research-stage proposal.

---

## 8. Direct and continuous funding

Where grants are episodic, direct funding pays maintainers continuously. The shift reflects an old critique: grants fund _creation_ but starve _maintenance_.

- **Open Collective** — transparent collective budgets; widely used by OSS projects ([GitHub][open-collective]).
- **Drips** — continuous "streaming" funding to OSS dependencies; no platform fees ([Drips][drips]).
- **Liberapay** — recurrent donations, OSS, nonprofit-run ([Liberapay][liberapay]).
- **GitHub Sponsors, thanks.dev, tea.xyz** — direct maintainer support, mostly proprietary.
- **Sovereign Tech Fund (Germany)** — government-funded support for critical OSS infrastructure; notable institutional model ([STF][stf]).

---

## 9. Cross-cutting comparison

| Dimension          | Peer review                      | QF                            | RetroPGF                           |
| ------------------ | -------------------------------- | ----------------------------- | ---------------------------------- |
| Who decides        | Expert panel                     | Crowd (with matching)         | Curated judges + impact oracle     |
| Main bias          | Conservatism, prestige           | Visibility, marketing         | Visibility, badgeholder politics   |
| Main attack vector | Networking / prestige laundering | Sybils, collusion             | Bribery, capture, lobbying         |
| Burden falls on    | Applicants + reviewers           | Community + Sybil-defense ops | Builders (defer payment) + judges  |
| Strength           | Domain expertise                 | Surfaces consensus            | Captures emergent value            |
| Weakness           | Slow, biased middle              | Invisible work loses          | Requires patient builders + judges |

**Pattern 1:** None of these mechanisms eliminates the need for human judgment about quality — they shift _when, by whom, and under what aggregation rule_ the judgment happens. Mechanism choice mostly determines which biases and which gaming strategies dominate, not whether biases exist.

**Pattern 2:** The most credible empirical results in all three areas are about _failure modes_, not successes. Peer review's problems are rigorously measured; QF's Sybil/collusion vulnerabilities are formally analyzed; RetroPGF rounds have generated multiple critical post-mortems. The mechanisms' _success_ claims tend to be qualitative.

**Convergence point (mid-2026):** hybrid stacks. SNSF: peer review + lottery in the disagreement band. Gitcoin GG24: QF + retro + direct grants per category. ARPA-H: PM-led with light external review. The pure-mechanism era is ending; the design question is which mix fits which kind of work.

### Practice patterns now near-standard

- **Milestone-gated disbursement** — dominant in both web3 (Arbitrum, Optimism, Catalyst) and increasingly in traditional grants ([Allied VC][allied-milestone]).
- **Two-stage review** — short outline → full application; ~5–10× reduction in reviewer hours.
- **Domain-specialized allocator pools** — sub-panels with topical expertise allocate a pre-budgeted slice (Arbitrum/Questbook; NSF program officers).
- **Open-ranking transparency** — public scores, reviewer comments, decisions (Catalyst, Gitcoin, Optimism, Wellcome).
- **AI-assisted application and triage** — the 2025 Elsevier survey found 41% of researchers used AI to help draft proposals; in 2025 only 5% of European Commission proposals fell below the quality threshold, down from 20% in 2018 ([Nature 2026 — agentic AI and grants][nature-agentic-ai]).

---

## 10. Open-source tooling

### Traditional grant management (call → application → review → award)

| Tool                                                 | What it is                                                           | Notes                               |
| ---------------------------------------------------- | -------------------------------------------------------------------- | ----------------------------------- |
| [Gandhi][gandhi]                                     | Self-hostable grant management system (Node + RethinkDB + Redis)     | Alpha but in real-world use         |
| [CiviCRM][civicrm] (with CiviGrant)                  | OSS CRM for nonprofits with grants module                            | Mature; self-host or partner-hosted |
| [Open Journal Systems][ojs] / [OpenReview] / Janeway | Submission + peer-review pipelines, often repurposed for grant calls | OJS used by 44k+ journals           |
| ProjeQtOr                                            | OSS project management, repurposable for grant tracking              | Generic; needs configuration        |

The OSS gap in this category is real — most groups customize CiviCRM or build on OJS / OpenReview. Most production funders run proprietary tools (Submittable, Fluxx, SmartSimple).

### Crypto-native / public-goods mechanisms (mostly in maintenance or winding down)

| Tool                                 | Mechanism                                                          | Status                                                                                                |
| ------------------------------------ | ------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| [Allo Protocol][allo]                | Generalized capital allocation primitives (QF, direct, RFP, retro) | Maintenance mode May 2025; contracts open and forkable                                                |
| [Gitcoin Grants Stack][grants-stack] | Full-stack app built on Allo                                       | OSS, forkable; archived                                                                               |
| [EasyRetroPGF][easy-retropgf]        | Lightweight RetroPGF toolkit for EVM chains                        | Used by Celo RPGF                                                                                     |
| [Drips][drips]                       | Continuous "streaming" funding to OSS dependencies                 | No platform fees                                                                                      |
| [Hypercerts][hypercerts]             | Standard for tradeable impact certificates                         | Early but actively developed                                                                          |
| [Octant][octant]                     | Community-voted grants funded by staking yield                     | OSS reference implementation; not currently active                                                    |
| [Open Source Observer][oso]          | Impact metrics pipeline for allocation decisions                   | Often paired with RetroPGF rounds; business shifting toward data (see 2026-03-04 note in repo readme) |

**Caveat:** Most of these projects are either tied to specific blockchains, in maintenance mode, dead, or shifting business priorities. They are reference implementations more than ready-to-deploy tools for general use.

### Direct contributor / maintainer funding

| Tool                                 | Notes                                                                                            |
| ------------------------------------ | ------------------------------------------------------------------------------------------------ |
| [Open Collective][open-collective]   | Transparent collective budgets; widely used by OSS projects                                      |
| [Liberapay][liberapay]               | Recurrent donations, OSS, nonprofit-run                                                          |
| GitHub Sponsors, thanks.dev, tea.xyz | Direct maintainer support, mostly proprietary platforms                                          |
| [Sovereign Tech Fund][stf]           | Government-funded support for critical OSS infrastructure (Germany); notable institutional model |

---

## 11. Open problems and active frontiers

1. **Sybil resistance for one-person-one-vote mechanisms.** RetroPGF, QF, and Catalyst all degrade under sybil attack. World ID, Proof of Humanity, Gitcoin Passport, and ZK-based personhood proofs are competing solutions, none fully solved.
2. **Aggregating evidence of impact.** RetroPGF's hardest problem is _measurement_ — what counts as impact in an ecosystem with no revenue? Citation-style metrics, on-chain-usage metrics, OSO-style mechanical pipelines, and badgeholder qualitative review are all in active use.
3. **Reducing application cost.** The "writing tax" is widely recognized. Approaches: shorter applications, narrative CVs, application reuse, fewer rejections by triage, and lotteries above a quality threshold.
4. **Pull-vs-push pricing.** Theoretical work continues on when AMC-like or prize-like instruments dominate grants for a given problem class ([NBER w26889][nber-azoulay]).
5. **Composability of mechanisms.** A real program is usually a stack: a _call structure_ (RFP, open, rolling), a _review mechanism_ (peer / panel / community / market), an _aggregation rule_ (rank, score, vote, lottery), and a _disbursement schedule_ (lump-sum, milestone, retroactive). Most innovation is in recombining these primitives.

---

## 12. Choosing a mechanism

| Goal                                                 | Likely best instrument                                  | Examples                                 |
| ---------------------------------------------------- | ------------------------------------------------------- | ---------------------------------------- |
| Fund exploratory, hard-to-specify work               | **Grant** (peer-review + lottery near margin)           | NIH, ERC, SNSF, Wellcome                 |
| Fund novel / risky research                          | **PM-led ARPA-style** or **FRO**                        | DARPA, ARPA-H, ARIA, Convergent Research |
| Make decisions fast, low burden                      | **Fast grants / micro-ARPA**                            | Fast Grants, Emergent Ventures           |
| Reward a precise, verifiable target                  | **Prize**                                               | X-Prize, DARPA Grand Challenges          |
| Guarantee demand for a future product                | **AMC**                                                 | Pneumococcal vaccine, Frontier Climate   |
| Allocate a community treasury for small public goods | **QF** (small donors) or **RetroPGF** (impact realized) | Gitcoin Grants, Optimism Retro           |
| Fund a roadmap with measurable phases                | **Milestone-based grant**                               | Arbitrum DAO, Catalyst, most DAOs        |
| Fund science with IP upside                          | **IP-NFT / DAO grant**                                  | VitaDAO, Molecule-mediated DAOs          |
| Pick between options under empirical uncertainty     | **Futarchy / conditional prediction markets**           | DeSci pilots, governance research        |
| Discrete, small-scope task                           | **Bounty**                                              | Gitcoin Bounties, ImmuneFi               |
| Sustain OSS maintainers                              | **Direct streaming**                                    | Drips, Open Collective, STF              |

**Practical recipes:**

- **Conventional research call?** OpenReview or OJS for the review pipeline; CiviCRM + CiviGrant or Gandhi for full CRM / award / reporting. Strongly consider adding a lottery in the contested middle band.
- **Allocating a pool to many small public-goods projects with community input?** Fork Grants Stack on Allo, or EasyRetroPGF if retroactive. Be honest about the Sybil-resistance burden.
- **Continuously funding OSS dependencies you use?** Drips is the cleanest primitive; Open Collective if you want fiat-friendly nonprofit flow.
- **"Fund now, settle later" with proof-of-impact?** Hypercerts is the standard worth tracking, though the ecosystem is early.

---

<!-- Links -->

[gandhi]: https://github.com/mike-marcacci/gandhi
[civicrm]: https://civicrm.org/
[ojs]: https://pkp.sfu.ca/software/ojs/
[OpenReview]: https://openreview.net/
[allo]: https://github.com/allo-protocol
[grants-stack]: https://gitcoin.co/apps/gitcoin-grants-stack
[easy-retropgf]: https://easyretropgf.xyz/
[drips]: https://www.drips.network/
[hypercerts]: https://github.com/hypercerts-org
[octant]: https://octant.app/
[oso]: https://www.opensource.observer/
[open-collective]: https://github.com/opencollective/opencollective
[liberapay]: https://github.com/liberapay/liberapay.com
[stf]: https://www.sovereigntechfund.de/
[luminary-grants-prizes]: https://www.luminary-labs.com/whats-the-difference-between-grants-and-prizes/
[rethink-prizes]: https://rethinkpriorities.org/research-area/how-effective-are-prizes-at-spurring-innovation/
[amc-wiki]: https://en.wikipedia.org/wiki/Advance_market_commitment
[belfer-amc]: https://www.belfercenter.org/publication/using-advance-market-commitments-public-purpose-technology-development
[nber-azoulay]: https://www.nber.org/system/files/working_papers/w26889/w26889.pdf
[ssir-prizes]: https://ssir.org/articles/entry/prizes_and_challenges_matter_for_development
[peer-review-flaws]: https://academic.oup.com/rev/article/32/4/623/7328889
[grant-peer-review-health]: https://pmc.ncbi.nlm.nih.gov/articles/PMC5883382/
[bias-burden-conservatism]: https://f1000research.com/articles/8-851
[ranking-vs-rating]: https://pmc.ncbi.nlm.nih.gov/articles/PMC10553257/
[pmc-troubles]: https://pmc.ncbi.nlm.nih.gov/articles/PMC6893288/
[pmc-realist]: https://pmc.ncbi.nlm.nih.gov/articles/PMC8894828/
[pmc-qrp]: https://pmc.ncbi.nlm.nih.gov/articles/PMC8825646/
[pmc-bollen]: https://pmc.ncbi.nlm.nih.gov/articles/PMC3989857/
[springer-impact]: https://link.springer.com/article/10.1007/s10734-016-9995-x
[hsls-impact]: https://hsls.libguides.com/c.php?g=1073359&p=9009219
[mbio-lottery]: https://journals.asm.org/doi/full/10.1128/mbio.00422-16
[oxford-lottery-taxonomy]: https://academic.oup.com/rev/article/doi/10.1093/reseval/rvae025/7735322
[snsf-lottery]: https://www.tandfonline.com/doi/full/10.1080/2330443X.2022.2086190
[swiss-funder-draws-lots]: https://www.nature.com/articles/d41586-021-01232-3
[lottery-before-peer-review]: https://www.nature.com/articles/s41467-025-65660-9
[dora-lotteries]: https://sfdora.org/2025/03/27/using-lotteries-to-increase-fairness-and-efficiency-in-research-assessment/
[ror-randomizing-2025]: https://researchonresearch.blog/2025/07/12/randomizing-selection-under-uncertainty-designing-principled-partial-lotteries-for-grant-funding-and-beyond/
[rpr-acceptability]: https://researchintegrityjournal.biomedcentral.com/articles/10.1186/s41073-019-0089-z
[spp-survey]: https://academic.oup.com/spp/article/49/3/365/6463570
[preregistration-grant-review]: https://arxiv.org/pdf/2511.01439
[clearest-metascience-reform]: https://www.maximum-progress.com/p/the-single-clearest-metascience-policy
[micro-arpa]: https://fas.org/publication/micro-arpa/
[arpa-h-programs]: https://arpa-h.gov/explore-funding/programs
[liberal-radicalism]: https://scholar.harvard.edu/files/hitzig/files/buterin_hitzig_weyl_draft.pdf
[qf-gitcoin]: https://gitcoin.co/mechanisms/quadratic-funding
[arxiv-qf-pasquini]: https://arxiv.org/abs/2010.01193
[arxiv-qf-optimal]: https://arxiv.org/pdf/2207.14775
[qf-v2-antisybil]: https://dorafactory.medium.com/quadratic-funding-v2-protocol-anti-sybil-fairness-and-scalability-on-chain-2a7aec43541c
[gg24-interop]: https://gitcoin.co/case-studies/gg24-interop-round-retrospective
[gg23-retro]: https://www.gitcoin.co/blog/gitcoin-grants-23-retro
[retropgf-origin]: https://medium.com/ethereum-optimism/retroactive-public-goods-funding-33c9b7d00f0c
[unchained-retropgf]: https://unchainedcrypto.com/retroactive-public-goods-funding/
[retro-funding-2025]: https://www.optimism.io/blog/retro-funding-2025
[retro-funding-name-criticism]: https://unchainedcrypto.com/optimisms-3-billion-retroactive-funding-round-draws-criticism-for-name-change/
[social-choice-retropgf]: https://arxiv.org/pdf/2508.16285
[voting-vulnerabilities-retro]: https://arxiv.org/pdf/2505.16068
[oso-retro]: https://docs.opensource.observer/blog/tags/retroactive-public-goods-funding/
[gitcoin-milestone]: https://gitcoin.co/mechanisms/milestone-based-funding
[lampros-milestone]: https://lampros.tech/blogs/web3-grant-funding-2025-milestone-based-dao-models
[allied-milestone]: https://www.allied.vc/guides/milestone-based-vc-funding-explained
[arbitrum-foundation-grants]: https://arbitrum.foundation/grants
[bitget-daos]: https://www.bitget.com/academy/successful-dao-case
[ethereum-grants]: https://ethereum.org/community/grants/
[projectcatalyst-site]: https://projectcatalyst.io/
[cardano-dev-portal]: https://developers.cardano.org/docs/community/funding/
[cryptoslate-catalyst]: https://cryptoslate.com/cardanos-project-catalyst-is-changing-hands-and-the-pause-is-forcing-builders-to-face-a-brutal-funding-gap/
[yellow-desci]: https://yellow.com/research/decentralized-science-desci-research-funding-explained
[molecule-democratizing]: https://www.molecule.to/learn/democratizing-research-the-rise-of-decentralized-science
[stanford-desci]: https://law.stanford.edu/2023/07/27/unlocking-scientific-innovation-through-decentralized-science-part-ii/
[frontiers-futarchy]: https://www.frontiersin.org/journals/blockchain/articles/10.3389/fbloc.2025.1650188/full
[nature-agentic-ai]: https://www.nature.com/articles/d41586-026-01297-y
