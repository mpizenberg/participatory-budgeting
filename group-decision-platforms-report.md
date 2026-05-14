# Group Decision Platforms and Participatory Budgeting: A Field Report

## 1. What the field is about

Two overlapping traditions meet here:

**Group decision platforms** — software that helps a defined group (a team, cooperative, association, party, city council) move from discussion to a binding or advisory decision. The unit of analysis is the _deliberation_ and _vote_. Examples: Loomio, OpaVote, Helios, Decidim assemblies, Polis.

**Participatory budgeting (PB)** — a specific civic process where residents directly decide how to spend part of a public budget. It usually combines four moments: (1) idea collection, (2) eligibility/feasibility vetting by the administration, (3) public voting (often ranked or constant-sum), and (4) execution monitoring. Originally a face-to-face process; now overwhelmingly digital or hybrid.

The fields converge because PB, at scale, _requires_ group-decision infrastructure: tens of thousands of proposals, hundreds of thousands of voters, multilingual access, identity verification, and aggregation rules that map onto methodologies from the ranking-survey literature (constant-sum allocation, ranked voting, MaxDiff-style prioritization).

---

## 2. Historical evolution

### Phase I — Origins in Porto Alegre (1989–2001)

PB was invented in **Porto Alegre, Brazil, in 1989** when newly elected Workers' Party (PT) mayor Olívio Dutra institutionalized neighborhood assemblies that decided municipal investment priorities ([Wikipedia][wiki-pb], [Participedia][participedia-porto]). The mechanism combined two PT traditions: base-community organizing inherited from liberation theology, and a "popular administration" ethos. Participation rose from <1,000 people in 1990 to ~40,000 by 1999; sewer connections went from 75% to 98% of households over a decade.

By 2001 over 100 Brazilian cities had adopted PB. The 2001 World Social Forum in Porto Alegre turned the city into a global symbol of the "another world is possible" movement and exported the model abroad.

### Phase II — Internationalization and codification (2001–2011)

PB spread first across Latin America (Peru made it mandatory nationwide in 2003), then to Europe via Spain (Córdoba, Sevilla), Italy, France, Germany, and Portugal. **Lisbon (2008)** became the first European capital to adopt PB ([openedition][openedition-lisbon]). The World Bank, UN-Habitat, and OECD endorsed it as "best practice."

During this period PB was still mostly face-to-face. The software layer was thin — spreadsheets, basic CMS, paper ballots. The intellectual framing came from authors like **Brian Wampler, Yves Sintomer, Giovanni Allegretti, Boaventura de Sousa Santos** ([PSU Press — Wampler][psupress-wampler]).

### Phase III — Digital turn, post-Occupy (2011–2015)

Three things happened almost simultaneously:

- **Occupy Wall Street (2011)** popularized the idea that consensus-style decision-making at scale needed software. Activists from **Occupy Wellington** and the **Enspiral** cooperative network founded **Loomio** in 2012 in New Zealand — the first widely adopted general-purpose online group-decision tool, built as a worker cooperative ([Loomio history][loomio-history], [openDemocracy][opendemocracy-loomio]).
- **Spain's 15-M / Indignados movement (2011)** demanded "real democracy now." Its activists later took municipal power in 2015.
- **Taiwan's g0v (gov-zero) civic-hacker collective** formed in 2012; after the **Sunflower Movement (2014)** it launched **vTaiwan**, integrating **Polis** (built by Colin Megill, Christopher Small, Mike Bjorkegren) to surface "rough consensus" via PCA-based opinion mapping ([compdemocracy — vTaiwan case study][compdemocracy-vtaiwan]). Audrey Tang later institutionalized this work as Taiwan's first Digital Minister.

### Phase IV — The Spanish municipal platforms (2015–2017)

This is the inflection point that defines today's open-source landscape.

- **Madrid**: Ahora Madrid (the citizens' platform that won the 2015 election) launched **Decide Madrid** in September 2015. Its codebase, **CONSUL**, was released as free software ([Democracy Technologies][democracy-tech-consul]).
- **Barcelona**: Barcelona en Comú (Ada Colau's coalition) launched **decidim.barcelona** on 31 January 2016, initially as a fork of CONSUL. In 2017 the team **rewrote it from scratch** in Ruby on Rails as **Decidim** — modular, federated, with a strong "democratic guarantees" social contract that every install must honor ([Computational Culture][comp-culture-decidim]).

Both platforms supported the full PB cycle (proposal → debate → vetting → vote → monitoring). By the mid-2020s CONSUL was deployed in 130+ institutions across 33 countries; Decidim in 400+ instances including national governments (France's _Make.org_ partnerships, Mexico City, the European Commission's _Conference on the Future of Europe_).

### Phase V — Scale, AI, and the legitimacy crisis (2017–today)

Several trends shape the current era:

- **Hyperscale PB**. **Paris** raised its PB envelope to **€100 million/year** (2014→), and **Madrid** matched it — making them the largest sustained PB processes ever ([EURAC blog][eurac-pb]). **Cascais (Portugal)** became a global reference for execution quality. Portugal alone now hosts hundreds of municipal PBs.
- **Global numbers**. Estimates vary by definition, but **7,000–11,500 municipal PB processes** are reportedly running worldwide as of the mid-2020s ([EU Parliament briefing][europarl-pb]).
- **Decline at the origin**. Porto Alegre's PB has eroded since 2017: shrinking fiscal envelopes, political disinvestment after PT lost the mayoralty, and "gradual policy abandonment" as undelivered projects discouraged participants ([WRI][wri-pb], [Cambridge — _Time of Closure_][cambridge-closure]). Wampler's comparative work across eight Brazilian cities shows that political commitment, not technology, predicts success.
- **AI-mediated deliberation**. Polis remains the canonical tool, but newer experiments (Talk to the City, DeepDemocracy, Generative AI-augmented Decidim modules) cluster comments and synthesize "bridging" positions automatically.
- **Methodological diversification**. Cities are moving past "approve up to N projects" toward methods aligned with the ranking-survey literature — constant-sum point allocation, ranked-choice tabulation, and quadratic voting/funding (pioneered by Glen Weyl and used in Colorado's 2019 legislative appropriations and in Gitcoin Grants).

---

## 3. Anatomy of a modern PB process

Most digital PB cycles follow the same pipeline:

| Stage                 | Typical duration | Decision-theory mapping                               |
| --------------------- | ---------------- | ----------------------------------------------------- |
| Agenda framing        | 2–4 weeks        | Often opaque — handled by the administration          |
| Proposal collection   | 4–8 weeks        | Open submissions, often with seconding thresholds     |
| Technical feasibility | 4–8 weeks        | Admin filtering — the most politically contested step |
| Public vote           | 2–4 weeks        | Approval, ranked, or constant-sum (€ allocation)      |
| Implementation        | 1–3 years        | Monitored via "follow-up" dashboards                  |

The **vote** is where the ranking-survey methodologies become directly relevant: Paris and Lisbon use approval-style ballots; some smaller cities use constant-sum (give the citizen €X to allocate); a handful experiment with IRV or Condorcet aggregation when picking among rival mutually-exclusive projects.

---

## 4. Map of the current open-source platform landscape

| Platform                                          | Origin                             | Best for                                                   | License  |
| ------------------------------------------------- | ---------------------------------- | ---------------------------------------------------------- | -------- |
| **[Decidim][decidim]**                            | Barcelona, 2017                    | Full civic-participation suite incl. PB, assemblies, votes | AGPL-3.0 |
| **[CONSUL Democracy][consuldemocracy]**           | Madrid, 2015                       | PB + petitions + debates; lighter than Decidim             | AGPL-3.0 |
| **[Loomio][gh-loomio]**                           | Wellington, 2012                   | Small/medium groups, cooperatives, internal decisions      | AGPL-3.0 |
| **[Polis][gh-polis]**                             | Seattle, 2012                      | Mapping viewpoints, finding rough consensus                | AGPL-3.0 |
| **[Your Priorities][gh-your-priorities]**         | Iceland, 2008 (after Reykjavík PB) | Idea generation + pro/con argument ranking                 | AGPL-3.0 |
| **[Cobudget][cobudget]** (Greaterthan / Enspiral) | NZ, 2014                           | Money allocation by small groups; constant-sum native      | AGPL-3.0 |
| **[Belenios][belenios]**                          | INRIA, France                      | Verifiable secret ballots (Schulze/STV/MJ)                 | CeCILL   |
| **[CIVS][civs]**                                  | Cornell, 2003                      | Condorcet polls — academic & FOSS communities              | open     |
| **[Stanford PB Platform][pbstanford]**            | Stanford, 2015                     | Research-backed approval/knapsack-vote PB                  | open     |

A working **PB stack** today typically looks like: Decidim (or CONSUL) for the full lifecycle; a national ID or municipal SSO for verification; Polis if there's a deliberation phase before proposals are written; Belenios when ballot secrecy or verifiability is required; the Stanford PB platform when the city wants knapsack-style "budget-aware" voting.

---

## 5. Open tensions in the field

These are the open debates if you want to go deeper:

- **Inclusion vs. self-selection**. Digital PB tends to over-represent already-engaged citizens; mitigations include sortition (citizen assemblies), proactive outreach, and youth/school PB.
- **Binding vs. advisory**. Most processes are advisory in law; their legitimacy comes from the political commitment to implement results — exactly the variable Wampler identifies as decisive.
- **Aggregation rule choice**. Approval voting is dominant because it's simple but produces "winner-take-all-the-popular-categories" results. Knapsack voting (vote subject to budget constraint) and constant-sum better reflect resource scarcity; Condorcet/IRV better handle rival mutually-exclusive projects.
- **AI-augmented deliberation**. Polis-style clustering is increasingly paired with LLM summarization (Anthropic's collaboration with the Computational Democracy Project on _Talk to the City_, Meta's _Community Forums_, the _Recursive Public_ prototype). The risk is that the model becomes the de facto agenda-setter.
- **Federation and platform sovereignty**. Decidim's "democratic guarantees" contract is an attempt to prevent platform capture; whether municipal IT can sustain self-hosting versus reverting to SaaS is unresolved.

---

## 6. Reading list for going deeper

- Brian Wampler, Stephanie McNulty, Michael Touchton — _[Participatory Budgeting in Global Perspective][oup-wampler]_ (OUP, 2021) — the current reference book.
- Yves Sintomer, Carsten Herzberg, Anja Röcke — _Participatory Budgeting in Europe_ (Routledge).
- [EU Parliament briefing — _Participatory budgeting: a pathway to inclusive governance_ (2024)][europarl-pb].
- [Computational Culture — _The Decidim 'soft infrastructure'_][comp-culture-decidim] — the best academic account of Decidim's design philosophy.
- [openDemocracy — _From Occupy to online democracy: the Loomio story_][opendemocracy-loomio].
- [Computational Democracy Project — vTaiwan case study][compdemocracy-vtaiwan].
- [WRI — _What if citizens set city budgets?_][wri-pb] on Porto Alegre's decline.
- [Participedia][participedia] — case database of 2,000+ deliberative and PB processes.

---

## Bottom line

Participatory budgeting started in 1989 as a left-wing municipal reform in southern Brazil and is now a globally institutionalized practice running in 7,000+ cities. Its digital evolution traces a specific lineage: **Porto Alegre → Reykjavík/Your Priorities (2008) → Loomio/Occupy (2011) → vTaiwan/Polis (2014) → Decide Madrid/CONSUL (2015) → Decidim Barcelona (2016–17)**. The open-source stack that emerged from this trajectory (Decidim, CONSUL, Polis, Loomio, Belenios) is now mature enough to run national-scale processes, but the field's main constraint is no longer technical — it is political will to honor the outcomes, the variable Porto Alegre's decline most clearly illustrates.

---

## Sources

- [Wikipedia — Participatory budgeting][wiki-pb]
- [Wikipedia — Participatory budgeting by country][wiki-pb-country]
- [Participedia — Porto Alegre 1989–present][participedia-porto]
- [WRI — Porto Alegre PB challenges][wri-pb]
- [Cambridge — _A Time of Closure_][cambridge-closure]
- [Cambridge — Changing urban movements after Porto Alegre's PB erosion][cambridge-urban]
- [Wampler — _Participatory Budgeting in Brazil_ (PSU Press)][psupress-wampler]
- [Wampler, McNulty, Touchton — _Participatory Budgeting in Global Perspective_ (OUP)][oup-wampler]
- [EU Parliament briefing (2024)][europarl-pb]
- [Eurac — Rise and spread of PB in European cities][eurac-pb]
- [Lisbon PB results (openedition)][openedition-lisbon]
- [Cascais PB (GIFT)][cascais-gift]
- [Centre for Public Impact — Green PB Lisbon][cpi-lisbon]
- [Democracy Technologies — Decide Madrid & CONSUL][democracy-tech-consul]
- [Tandfonline — Technopolitical platforms in Madrid and Barcelona][tandfonline-tech]
- [Computational Culture — Decidim soft infrastructure][comp-culture-decidim]
- [Decidim][decidim]
- [CONSUL Democracy case study (EU OSOR)][osor-consul]
- [Loomio cooperative history][loomio-history]
- [Wikipedia — Loomio][wiki-loomio]
- [openDemocracy — From Occupy to online democracy][opendemocracy-loomio]
- [Computational Democracy Project — vTaiwan case study][compdemocracy-vtaiwan]
- [RadicalxChange — Taiwan: Grassroots Digital Democracy That Works (PDF)][radicalxchange-tw]
- [Democracy Foundation — list of e-voting & deliberation projects][democracy-foundation]
- [Participedia][participedia]

<!--## Links-->

[wiki-pb]: https://en.wikipedia.org/wiki/Participatory_budgeting
[wiki-pb-country]: https://en.wikipedia.org/wiki/Participatory_budgeting_by_country
[wiki-loomio]: https://en.wikipedia.org/wiki/Loomio
[participedia-porto]: https://participedia.net/case/5524
[participedia]: https://participedia.net/
[openedition-lisbon]: https://journals.openedition.org/factsreports/3363
[psupress-wampler]: https://www.psupress.org/books/titles/978-0-271-03252-8.html
[oup-wampler]: https://global.oup.com/academic/product/participatory-budgeting-in-global-perspective-9780192897756
[loomio-history]: https://www.loomio.coop/history.html
[opendemocracy-loomio]: https://www.opendemocracy.net/en/from-occupy-to-online-democracy-loomio-story/
[compdemocracy-vtaiwan]: https://compdemocracy.org/case-studies/2014-vtaiwan/
[democracy-tech-consul]: https://democracy-technologies.org/participation/decide-madrid-and-consul/
[comp-culture-decidim]: http://computationalculture.net/the-decidim-soft-infrastructure/
[eurac-pb]: https://www.eurac.edu/en/blogs/eureka/the-rise-and-spread-of-participatory-budgeting-in-european-cities
[europarl-pb]: https://www.europarl.europa.eu/RegData/etudes/BRIE/2024/762412/EPRS_BRI(2024)762412_EN.pdf
[wri-pb]: https://www.wri.org/insights/what-if-citizens-set-city-budgets-experiment-captivated-world-participatory-budgeting
[cambridge-closure]: https://www.cambridge.org/core/journals/journal-of-latin-american-studies/article/time-of-closure-participatory-budgeting-in-porto-alegre-brazil-after-the-workers-party-era/44EC7210668F4E4CC82853961C5133E9
[cambridge-urban]: https://www.cambridge.org/core/journals/latin-american-politics-and-society/article/changing-urban-movements-repertoires-following-the-erosion-of-porto-alegres-participatory-budgeting-from-institutionalized-participation-to-deinstitutionalization/39480EBC25E1C90D56E7B76845121AFF
[decidim]: https://decidim.org/
[consuldemocracy]: https://consuldemocracy.org/
[gh-loomio]: https://github.com/loomio/loomio
[gh-polis]: https://github.com/compdemocracy/polis
[gh-your-priorities]: https://github.com/CitizensFoundation/your-priorities-app
[cobudget]: https://www.cobudget.com/
[belenios]: https://www.belenios.org/
[civs]: https://civs1.civs.us/
[pbstanford]: https://pbstanford.org/
[cascais-gift]: http://guide.fiscaltransparency.net/case-study/cascais-participatory-budgeting-portugal/
[cpi-lisbon]: https://centreforpublicimpact.org/public-impact-fundamentals/green-participatory-budgeting-lisbon-portugal/
[tandfonline-tech]: https://www.tandfonline.com/doi/full/10.1080/10630732.2020.1786337
[osor-consul]: https://interoperable-europe.ec.europa.eu/collection/open-source-observatory-osor/document/case-study-consul-democracy
[radicalxchange-tw]: https://www.radicalxchange.org/updates/papers/Taiwan_Grassroots_Digital_Democracy_That_Works_V1_DIGITAL_.pdf
[democracy-foundation]: https://democracy.foundation/similar-projects/
