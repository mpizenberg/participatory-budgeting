# Sybil-resistant voting mechanisms for Cardano

*A technical synthesis of the state of the art in privacy-preserving, Sybil-resistant voting, with specific consideration of implementation on Cardano.*

---

## Executive summary

Sybil resistance and voter anonymity appear to conflict: preventing one person from casting many votes seems to require identifying voters, but identification destroys the secret ballot. Modern cryptography resolves this tension by **separating enrollment (where uniqueness is established) from voting (where unlinkability holds)**. The state of the art combines:

- an **identity layer** producing a one-per-person credential (typically via zero-knowledge proofs over a government-issued biometric document, sometimes via biometrics or social graph attestations), and
- a **voting layer** turning that credential into an anonymous, non-replayable ballot (typically via nullifier-based ZK signaling such as Semaphore or MACI, plus homomorphic tallying or mix-nets).

Cardano, since the Chang hard fork (2024) shipping Plutus V3 with BLS12-381 builtins, has the cryptographic primitives needed to verify the resulting succinct proofs on-chain. Working demonstrations of Groth16 verification on Cardano mainnet already exist. The remaining limitations are around transaction-budget ceilings for very large proofs (being addressed by CIP-0133 and the in-progress Halo2-Plutus verifier).

This report covers the trust model in depth: what cryptography can and cannot guarantee, how trust is relocated rather than eliminated, and what a verifier must actually check to make the system secure in practice.

---

## 1. The core tension and how cryptography resolves it

A Sybil attack is the creation of multiple identities by a single entity to gain disproportionate influence. Resisting it requires verifying that each voter is a unique person. Voter anonymity requires that no one can link a vote, or even a voting credential, back to the person who cast it. The apparent contradiction is resolved by **decoupling the moment of uniqueness verification from the moment of voting**.

A useful framing comes from the ZK-passport community: a private voting protocol must provide three properties simultaneously, and naïve designs sacrifice at least one:

| Property | Meaning |
|---|---|
| **Eligibility** | Only authorized people can vote |
| **Anonymity** | Votes cannot be linked to voters |
| **Uniqueness** | One person, one vote |

A scheme that only checks eligibility via a ZK proof of passport preserves anonymity but loses uniqueness — the user can vote unlimited times. A scheme that ties votes to a public commitment of the passport preserves uniqueness but lets the issuer (typically the state) run a dictionary attack to learn who voted. The robust designs achieve all three by using a credential that is *unique per person per scope* but *unlinkable to any underlying identity* — a property delivered by a deterministic nullifier emitted from inside a zero-knowledge proof.

---

## 2. State of the art (2024–2026)

### 2.1 The identity layer: proof of personhood

Five families currently dominate, each with different trust assumptions:

**ZK proofs over government biometric documents.** The most active area as of 2024–2026. Projects such as Rarimo's Freedom Tool, Self Protocol (self.xyz), OpenPassport (acquired by Self in February 2025), ZKPassport, and Anon Aadhaar exploit the NFC chip in ICAO 9303 biometric passports or national IDs. The user scans the document with a phone, the device locally generates a ZKP verifying citizenship, age, or uniqueness, and no passport data ever leaves the phone. The trust anchor is the government's existing signing infrastructure; the cryptography ensures the issuer cannot link the credential to votes.

**Biometric uniqueness systems.** Worldcoin's iris-scanning Orb and Humanity Protocol's palm biometrics establish global uniqueness without depending on any state. They achieve strong Sybil resistance at the cost of trust in the biometric operator and concerns about template linkage.

**Social-graph / web-of-trust schemes.** BrightID, Proof of Humanity, and Idena (synchronized Turing tests) infer personhood from social relationships or live coordination rather than from documents or biometrics. They are vulnerable to specific attack patterns discussed in section 3.

**Aggregated / composite scoring.** Human Passport (formerly Gitcoin Passport, acquired by Holonym Foundation in late 2024) combines multiple independent signals — social account history, on-chain activity, ZK government credentials, phone verification, biometric checks, community vouching — into a composite score on the principle that no single signal should be decisive.

**Anonymous credentials (classical cryptography).** BBS+, Idemix, U-Prove, and Chaumian blind signatures predate the blockchain era but remain the formal foundation. The canonical recent reference is the 32-author paper *Personhood Credentials: Artificial Intelligence and the Value of Privacy-Preserving Tools to Distinguish Who Is Real Online* (Adler, Hitzig, Jain et al., arXiv 2408.07892, latest revision January 2025), authored by researchers from OpenAI, Microsoft, MIT, Harvard, and others.

Vitalik Buterin's conclusion — endorsed by that paper — is that there is no single ideal form of proof of personhood; the strongest path combines methods rather than betting on one.

### 2.2 The voting layer: anonymous, non-replayable ballots

Given a one-per-person credential, several primitives turn it into a private vote:

**Nullifier-based ZK signaling (Semaphore, MACI).** Semaphore lets a user prove membership in a Merkle tree of registered identities and emit a deterministic *nullifier* per vote scope. Because each nullifier is uniquely derived from the identity and the scope, double-voting is detected by checking whether that hash already exists, while the proof reveals nothing else about the voter. MACI (Minimum Anti-Collusion Infrastructure) extends this with key-switching so a voter can invalidate a coerced vote, defeating bribery on top of Sybils.

**Homomorphic tallying (Helios, ElectionGuard).** Votes are encrypted under a threshold key; the tally is computed on ciphertexts so individual votes are never decrypted. Combined with mix-nets or ZK proofs of correct encryption, this yields end-to-end verifiable, anonymous elections.

**Mix networks and re-encryption mixnets.** Unlink ballots from senders through cryptographic shuffling with proofs of correct shuffle.

**Blind signatures.** Chaum's original construction remains the simplest scheme: the registrar blind-signs a token, the voter unblinds it and submits it anonymously.

Recent academic work in 2025 includes *Burn Your Vote: Decentralized and Publicly Verifiable Anonymous Voting at Scale* (Cryptology ePrint 2025/1022), targeting scalable trustless privacy on-chain.

### 2.3 Mechanism-design caveats

Even with strong cryptography, the *voting rule itself* can be Sybil-fragile. A Stanford analysis by Bennett (2025) demonstrates that unpermissioned blockchains with quadratic mechanisms remain vulnerable: through indirect wallet creation an attacker can devolve quadratic voting into linear voting. Mitigations include authentication, fees, and vote thresholds.

A complementary economic-design paper by Meir, Shahaf, Shapiro et al. (*Review of Economic Design*, 2025) characterizes the tradeoff between safety and liveness, identifying the exact conditions under which voting rules remain both resilient to Sybils and responsive to verified participation.

Graph-learning *detection* approaches are also emerging: DuPont (2024) trained a graph convolutional neural network on Snapshot.org DAO voting and clustered embeddings to identify Sybil activity, pruning 2–5% of the voting graph.

---

## 3. Defeating the partition attack in social-graph schemes

A canonical failure mode of pure web-of-trust systems is the **partition attack** (or split-graph attack): if subgraphs do not communicate, each can independently validate the same person under different names, producing multiple "unique" credentials for one human. BrightID's own anti-Sybil test suite explicitly simulates this: *"A group of seed nodes attempt to create multiple clusters and connect sybils in each cluster to random sybils in other clusters."*

Pure social-graph schemes without any global uniqueness anchor cannot solve this. Every working system adds one or more anchors outside the graph itself.

### 3.1 Graph topology assumptions (SybilGuard family)

The classical defense — used by BrightID — assumes the boundary between honest and Sybil regions has *limited attack edges*. Algorithms such as SybilRank, SybilLimit, and BrightID's official GroupSybilRank do early-terminated random walks from trusted seed nodes; a random walk from a non-Sybil seed has a higher degree-normalized landing probability on non-Sybil nodes than on Sybils.

The catch is that this produces a *probabilistic score*, not a binary verdict, and security collapses if the topology assumption fails. Recent work (arXiv 2501.16624, January 2025) makes the limitation explicit: existing algorithms are designed under a homophily assumption (most edges connect same-type nodes) that does not hold in many real-world graphs. A determined attacker who patiently builds two well-integrated personas in two weakly connected communities will defeat pure structural analysis.

### 3.2 Single global searchable registry (Proof of Humanity model)

Forces subgroups to share a namespace. Every registration carries a video of the person saying a specific phrase, and the entire registry is publicly browsable. Uniqueness is enforced through challenges with economic stakes: users submit a deposit to challenge a submission, the deposit serves as a bounty for whoever correctly identifies a duplicate, and disputes are resolved by the Kleros decentralized arbitration court.

The critical structural move is **cascading punishment for vouchers**: if a submission is rejected for Sybil attack or identity theft, all vouchers are removed from the registry. Vouching is no longer a free click; it puts your own credential at risk.

### 3.3 Synchronized validation events (Idena, pseudonym parties)

All validators must participate in a global ceremony at the same wall-clock moment, solving attention-demanding puzzles. Because a single biological person cannot solve two such tests in two ceremonies simultaneously, the protocol mechanically caps each human to one identity per epoch. Originally proposed by Bryan Ford as in-person *pseudonym parties*; Idena implements it online.

Elegant in principle but scales poorly (everyone must show up at the same time) and excludes those who cannot participate at the chosen hour.

### 3.4 Biometric uniqueness anchor

Even nominally social systems usually bolt on a biometric uniqueness check. Faces, irises, or palms provide a natural global identifier that cannot be partitioned by subgraph. Worldcoin's iris hashing and Humanity Protocol's palm scans are the obvious examples; Proof of Humanity uses video that humans and deepfake detection models can compare. The trust assumption moves from "the social graph is well-connected" to "the biometric matcher and its operators are honest."

### 3.5 Composite / cross-context attestations

The newest approach (Human Passport, the MIT/OpenAI Personhood Credentials framework) refuses to rely on any single subgraph. Multiple independent signals are combined; no single signal is decisive; if one signal is compromised, the rest still hold. A partition attack must now succeed against every signal source simultaneously.

### 3.6 The information-theoretic limit

If two subgraphs share *no* common observable about a candidate — no video, no biometric, no public history, no shared time anchor, no economic stake that crosses both — then by symmetry they cannot distinguish "same person twice" from "two different people." Every working defense adds such a shared observable. The honest summary: no purely social, purely cryptographic, or purely biometric scheme survives a sophisticated partition attack on its own; robust constructions are deliberately heterogeneous.

---

## 4. ZK passports as a practical substrate

### 4.1 What makes a passport ZK-compatible

To prove anything about a passport in zero knowledge, the project must verify, inside a circuit, the signature chain from the CSCA (Country Signing Certificate Authority) through the DSC (Document Signer Certificate) to the SOD (Security Object Document) and onward to the data groups. Each state chooses its own signature algorithm — most commonly RSA (e.g., French passports use RSA-2048 DSCs under RSA-4096 CSCAs), with ECDSA the second most common and DSA rare.

A country's passport works with a ZK project if and only if:

1. The chip is present and readable over NFC (ICAO 9303 compliance).
2. The project's circuits implement the country's signature scheme, curve, and hash.
3. The project has registered that country's CSCA and DSC certificates from ICAO's Public Key Directory (PKD).

### 4.2 Coverage by major project

**Self Protocol (self.xyz)** — the broadest coverage after Self Labs acquired OpenPassport in February 2025. Supports 1 billion biometric passport holders from 129 countries, plus biometric IDs across 27 EU countries, Turkey, Ukraine, Vietnam, Ghana, and Saudi Arabia. Circuits support RSA, RSA-PSS, and ECDSA; SHA-1, SHA-256, SHA-384, SHA-512; RSA key lengths 1024–6144 bits and ECDSA 224–521 bits. Live coverage map at `map.self.xyz`.

**ZKPassport (zkpassport.id)** — supports most passports, national IDs, and residence permits that comply with ICAO standards. Failures usually mean unsupported signature scheme or insufficient chip security guarantees. Maintains a public coverage map.

**Rarimo (Freedom Tool)** — designed for activist use cases (originally for Russian dissidents). Notable for emphasis on **active authentication**: passive authentication alone allows an attacker who copies the chip data to generate proofs without physical possession, while active authentication uses the chip's onboard key to sign a challenge, defeating cloning. Not every country's chip supports active auth.

### 4.3 Beyond passports

ZK-friendly identity infrastructure exists without requiring a passport:

- **India — Aadhaar.** Anon Aadhaar and Self Protocol natively support proofs from Aadhaar via the mAadhaar app's signed QR. Covers ~1.3 billion people.
- **European Union — eID cards.** Most EU members issue ICAO-style biometric national IDs; Self supports all 27.
- **Other supported national IDs:** Turkey, Ukraine, Vietnam, Ghana, Saudi Arabia.
- **Estonia and other EU members** have Smart-ID and the upcoming EUDI Wallet (eIDAS 2.0) moving toward ZK-compatible selective disclosure.

### 4.4 Practical caveats

- Older passports (pre-2010 in many countries) either lack chips or use deprecated algorithms (SHA-1) that may be deprioritized.
- Some chips reject NFC reads from certain phone models or require precise antenna alignment.
- Active-authentication coverage is much narrower than passive coverage; security-vs-coverage tradeoffs are explicit.
- Coverage maps update frequently; always consult the live map of the project you intend to use.

---

## 5. Local proof generation: how the trust model actually works

A critical clarification: **the proof is generated fully locally on the user's device.** Self, ZKPassport, and Rarimo are *not* services that receive your passport data — they are open-source software stacks that run entirely on the phone. Nothing sensitive ever leaves the device.

### 5.1 The actual flow

1. **NFC scan.** The phone reads the chip, extracting data groups and the country's signatures.
2. **Local circuit execution.** The phone runs a ZK circuit (compiled from Circom, Noir, or Halo2) taking passport data as private inputs and the country's public certificate as a public input. It proves statements such as "I hold a passport signed by this country whose date of birth makes me over 18, with this nullifier."
3. **Proof output.** A small artifact (a few KB for Groth16) revealing only the chosen public outputs. The passport data, photo, and name are never in the proof.
4. **Submission.** Only the proof leaves the phone, sent to whichever verifier (smart contract, web backend) needs to check it.

Every public-facing app states this explicitly; it is a structural property of the design, not a marketing claim. The whole point of ZK is to avoid trusting any external party with the data.

### 5.2 What the projects actually provide

Four things, all of them public or open-source:

1. **Compiled ZK circuits** for each supported signature algorithm. Open source on GitHub (`zk-passport/openpassport`, `zkpassport/circuits`, `rarimo/passport-zk-circuits`).
2. **A mobile app** with polished UX for NFC scanning, MRZ reading, and proof generation. Also open source.
3. **A snapshot of the ICAO Public Key Directory** — the per-country CSCA/DSC public keys. Bundled so users do not have to fetch them.
4. **Trusted setup output** (Groth16) or **SRS parameters** (PLONK/Halo2) — public artifacts from one-time ceremonies.

The companies package open-source cryptography into something installable in two taps. Any of them can be forked, audited, and run independently. The proof produced is identical and verifiable by the same on-chain contract.

### 5.3 Building it independently

To build an independent stack:

- A mobile NFC reader (libraries like `NFCPassportReader` exist; ICAO 9303 is public).
- A ZK circuit verifying the country's signature chain in Circom, Noir, or Halo2.
- A prover binary (snarkjs, arkworks, barretenberg) compiled for mobile. Modern phones generate Groth16 proofs in 2–10 seconds for a passport-sized circuit.
- ICAO PKD certificates (free public download), refreshed periodically.
- A verifier contract on the target chain (Plutus V3 for Cardano).

Non-trivial engineering, but architecturally identical to what the projects do. There is no secret sauce on a server.

### 5.4 Trust assumptions that remain

Even with fully local execution, three trust assumptions persist:

1. **The user's device.** A compromised phone could read passport data before or during proof generation. Same trust model as any local wallet.
2. **The ICAO PKD.** The CSCA public keys must come from somewhere. Mitigated by widespread cross-publication and the ability to pin keys.
3. **The trusted setup (Groth16 only).** A rigged ceremony enables forgery. Mitigated by using outputs from large multi-party ceremonies, or by adopting PLONK or Halo2 (universal / transparent setup).

What is *not* trusted: Self Labs, ZKPassport, Rarimo, any cloud service, any centralized server. If any of them disappeared, anyone with the open-source code could continue generating and verifying valid proofs.

---

## 6. Cardano implementation

### 6.1 The two layers that need to talk

A ZK-passport flow has two distinct cryptographic stages, only one of which is chain-specific:

- **Prover side (off-chain, on the user's phone):** A circuit verifies the country's RSA/ECDSA signature chain and outputs a succinct proof over a SNARK-friendly curve such as BLS12-381. This is the part that varies per country.
- **Verifier side (on-chain):** The validator sees only the SNARK proof and public inputs. It does not care whether the underlying assertion was about a French RSA-2048 passport or a Japanese ECDSA P-256 passport; it just runs the Groth16/PLONK/Halo2 verifier over BLS12-381.

The country-specific complexity lives entirely off-chain. Cardano needs to support the outer proof system, not 174 different national PKIs.

### 6.2 Cardano's cryptographic surface

Plutus V3 (Chang hard fork, 2024) shipped the needed primitives:

- **CIP-0381**: built-in BLS12-381 operations (G1, G2, pairings) using the `blst` library.
- Native Ed25519, ECDSA over SECP256k1 / SECP256r1, Schnorr signature verification.
- SHA-256, SHA-3, Blake2b, Keccak, Ripemd-160 hashing.

A working proof-of-concept exists on Cardano mainnet: a Groth16-based age verification demo using Plutus V3 entirely on-chain, no sidechains or off-chain verification. The Aiken library `ak-381` from Modulo-P provides Groth16 verification tooling for Plutus V3 directly compatible with Circom and SnarkJS — the same toolchain Self.xyz and OpenPassport use.

### 6.3 Current limits and active improvements

The hard limit today is not "which country" but "how big the public input vector is." Groth16 verification cost is dominated by a multi-scalar multiplication (MSM) whose size scales with the number of public inputs. Per the current Plutus V3 numbers:

- A G1 MSM of size 129 over BLS12-381 consumes roughly 7.74% of the transaction compute budget.
- MSM beyond ~129 cannot fit into a single transaction.

A passport proof typically has 5–20 public inputs (country code, age threshold, nullifier, scope, anonymity-set root), comfortably within current limits.

Active improvements:

- **CIP-0133**: proposes native MSM-over-BLS12-381 as a Plutus builtin. Accepted, in implementation by the Plutus team.
- **Halo2-Plutus verifier**: IOG announced (August 2025) a Halo2 verifier with EasyCrypt-proven foreign-field arithmetic, removing the trusted-setup requirement and enabling larger circuits.

Once both ship, even complex passport proofs (large anonymity sets, recursive composition, many disclosures) will fit.

### 6.4 Theoretical country coverage

All ~174 ICAO countries that Self.xyz can produce proofs for could in principle have their proofs verified by a Plutus V3 validator. The country-specific work lives on the user's phone; the Plutus script sees an opaque proof. Today, in production, this works for proofs of moderate complexity; the residual ceiling for limit-pushing use cases will lift with CIP-0133 and the Halo2 verifier.

### 6.5 Three architectural options on Cardano

1. **Plutus V3 L1 verifier directly.** Submit the proof in the transaction redeemer; verify with an Aiken / PlutusTx Groth16 script. Works today for typical passport circuits. Maximally trustless, sufficient transparency, on-chain nullifier set.
2. **Midnight as a privacy sidechain.** IOG's Midnight is purpose-built for ZK applications, uses Halo2 natively, and provides higher proof throughput and shielded data. A passport-credential issuance flow fits naturally there, with the credential reusable across applications without putting every per-use proof on L1.
3. **Hyperledger Identus (formerly Atala PRISM).** The DID / verifiable-credential layer originally built by IOG, now under Hyperledger. Wraps the passport proof into a reusable credential; Plutus V3 used only for on-chain attestations or nullifier registries.

---

## 7. The verifier's checklist

The cryptographic proof check is the easy part. The trust model that makes the proof *mean* something has many more moving parts. A verifier that skips any of the following has a plausible-looking system with a hole an attacker can drive a truck through.

### 7.1 Cryptographic proof check

For Groth16 over BLS12-381: take the proof `(A, B, C)`, the public inputs, and the verification key `vk`; compute the input MSM; check the pairing equation `e(A, B) = e(α, β) · e(MSM_result, γ) · e(C, δ)`. If it holds, the proof is sound *with respect to the circuit defined by `vk`* — a narrower claim than "the user has a valid passport."

### 7.2 Bind the proof to the right circuit

- **Pin the verification key.** Hard-code `vk` (or its hash) in the validator. Never accept `vk` as untrusted input.
- **Pin the proof system parameters** (curve, hash, MSM size) to prevent confused-deputy attacks.

### 7.3 Check the public inputs *mean* what you require

The single most common bug class. The pairing check confirms the prover knew a witness consistent with these public inputs and this circuit; it does *not* tell you what the public inputs are.

- **Extract and inspect every public input.** Typical passport-proof inputs: `[cscaTreeRoot, currentDate, ageThreshold, allowedCountriesHash, nullifier, scope, userCommitment, …]`.
- **Check each one matches the policy.** If you want "over 18," check `ageThreshold == 18` was actually bound into the proof — otherwise the prover could have proved "over 5" and it still verifies.
- **Bound-check field elements** that your application semantics treat as smaller integers.
- **Bind a scope (external nullifier / domain separator).** Without it, proofs generated for one poll can be replayed in another.

### 7.4 Nullifier handling (replay protection)

- **Deterministic derivation inside the circuit:** `nullifier = Hash(passport_secret, scope)`. Same passport, same scope → same nullifier.
- **Maintain a consumed-nullifier set.** On Cardano, typically via a UTxO pattern or a mint policy where the token name is the nullifier hash and re-mints are disallowed.
- **Scope must be application-specific and bound inside the circuit.** A scope like `Election2026-Proposition3` produces a unique nullifier per poll; the same user can vote in `Election2026-Proposition4` without revealing they are the same person.
- **No malleability.** The nullifier must be uniquely derived from inputs the attacker cannot freely choose. If the prover picks the nullifier, the same passport produces N nullifiers and votes N times.

### 7.5 Trust roots for the credential

- **Maintain an authoritative root of accepted CSCA public keys** (a Merkle root of the ICAO PKD snapshot), governance-controlled and updatable as countries rotate or revoke keys.
- **Reject proofs against stale or unknown roots.** If any root is accepted, an attacker can construct one containing a self-issued "country" key and forge passports for an imaginary nation.
- **Plan for revocation.** Short-lived root snapshots, a revocation accumulator, or periodic re-attestation.
- **Bind the document type.** Differentiate passports, national IDs, residence permits so a national-ID proof cannot satisfy a passport-required verifier.

### 7.6 Liveness / non-cloning (active authentication)

Passive authentication does not prove physical possession — the data can be copied. To rule this out:

- **Verify an active-authentication challenge-response inside the circuit** (chip's `DG15` onboard key).
- **The challenge must be fresh and bound to the proof's scope** to prevent challenge-response replay.
- **Active-auth verification must be a public output** of the circuit, not assumed.

Coverage tradeoff: not every country supports active auth.

### 7.7 Trusted setup integrity (Groth16-specific)

- Use a `vk` from a well-known multi-party ceremony (perpetual powers-of-tau with many contributors). Trust assumption: 1-of-N honest, reasonable when N is large and publicly verifiable.
- Document the ceremony provenance in the validator or specification.
- Or adopt a transparent / universal setup: PLONK (KZG, one ceremony per curve), Halo2 (no trusted setup), STARKs (no trusted setup). Halo2 is what IOG is targeting for Cardano in part for this reason.

### 7.8 Cryptographic and hardware assumptions

The verifier does not actively check these, but the security argument depends on them:

- Hardness: discrete log over BLS12-381, q-DLOG and KOE for Groth16, collision resistance of Poseidon / SHA-256.
- Honest implementation: verifier code matches the proof-system spec. Audited libraries are essential; subtle bugs in pairing checks have historically allowed proof forgery in production.
- No prover side-channel compromise (outside verifier scope but part of user trust).
- Honest issuer: a stolen national CSCA key compromises all proofs from that country. Detection is impossible; only revocation mitigates.

### 7.9 Application-level binding

- **Bind the recipient address or action payload** as a public input. Without this, observers can copy the proof into their own front-running transaction.
- **Bind a recent block hash or timestamp** for freshness, preventing replays after the relevant context has changed.
- **Bind application-specific parameters** (proposal ID, campaign ID, access policy) so a proof for one context cannot be ported to another.

### 7.10 Putting it together: the Plutus V3 validator checklist

For a one-person-one-vote election on Cardano, the validator script should:

1. Load a hard-coded `vkHash` and verify the supplied `vk` matches.
2. Run the BLS12-381 pairing check against `vk`, the proof, and the public inputs.
3. Extract public inputs and verify: `cscaTreeRoot` equals the current governance-maintained root; `scope` equals this election's identifier; the policy fields (age threshold, country filter, etc.) match expectations; `recipientAddress` equals the tx recipient; `activeAuthVerified` is true; the proof is within the allowed time window.
4. Check the `nullifier` is not already consumed — typically by enforcing that a token with `nullifier` as token-name is being minted under a policy that disallows re-mints.
5. On success, perform the action (mint credential, record vote) and add the nullifier to the consumed set.

Missing any one of these silently turns a strong privacy-preserving Sybil-resistant scheme into a Sybil factory.

---

## 8. Recommended architecture

For a privacy-preserving, Sybil-resistant voting system on Cardano combining the current state of the art:

1. **Identity layer:** ZK passport proof (Self, ZKPassport, Rarimo, or a fork) generated locally on the user's phone, optionally combined with a Human Passport composite score for users without a biometric passport.
2. **Registration:** One-time on-chain registration via a Plutus V3 validator that verifies the passport proof and inserts the user's identity commitment into a Merkle anonymity set, indexed by a passport-bound nullifier.
3. **Voting:** A Semaphore-style or MACI-style ZK proof of membership in the anonymity set, with a per-poll nullifier. Verified by a Plutus V3 validator that maintains the consumed-nullifier set and records the vote.
4. **Tally:** Either homomorphic (threshold-decrypted at the end of the poll) or mix-net based, depending on privacy requirements against the tallier.
5. **Verifier policy:** Implements every item in section 7.10. Governance-controlled CSCA root, audited verifier libraries, well-known ceremony provenance for `vk`, active-auth required where coverage permits.

Architecturally, the L1-direct path is feasible today for typical poll sizes. Midnight is the right answer for high-throughput voting or where shielded vote contents matter. Hyperledger Identus is the right answer when reusable credentials across many applications are wanted.

---

## 9. Key references

### Foundational papers and specifications

- Adler, Hitzig, Jain et al. (2024, rev. Jan 2025). *Personhood credentials: Artificial intelligence and the value of privacy-preserving tools to distinguish who is real online.* arXiv 2408.07892.
- Meir, Shahaf, Shapiro et al. (2025). *Safe voting: resilience to abstention and sybils.* Review of Economic Design.
- Bennett, A. (2025). *Going Parabolic: Analyzing Sybil Resistance in Quadratic Voting Mechanisms for Blockchain-Based DAOs.* Stanford Digital Repository.
- DuPont, Q. (2024). *Graph Deep Learning on Anonymous Voting Networks to Identify Sybils in Polycentric Governance.* arXiv 2311.17929.
- *Burn Your Vote: Decentralized and Publicly Verifiable Anonymous Voting at Scale.* Cryptology ePrint Archive, 2025/1022.
- Bryan Ford. *Pseudonym Parties: An Offline Foundation for Online Accountability.* (Foundational for synchronized PoP.)
- ICAO Doc 9303, *Machine Readable Travel Documents*.

### ZK passport projects

- Self Protocol — <https://self.xyz>, docs at <https://docs.self.xyz>, coverage map at <https://map.self.xyz>, GitHub `selfxyz/self`.
- ZKPassport — <https://zkpassport.id>, docs at <https://docs.zkpassport.id>, GitHub `zkpassport/circuits`.
- Rarimo Freedom Tool — <https://docs.rarimo.com/zk-passport/>, GitHub `rarimo/passport-zk-circuits`.
- OpenPassport — <https://github.com/zk-passport/openpassport> (now part of Self).
- Anon Aadhaar — Indian Aadhaar ZK proofs.

### Proof-of-personhood systems

- Worldcoin / World ID — iris biometrics.
- Humanity Protocol — palm biometrics.
- BrightID — social graph + GroupSybilRank. GitHub `BrightID/BrightID-AntiSybil`.
- Proof of Humanity + Kleros — video + vouching + dispute arbitration.
- Idena — synchronized Turing-test ceremonies.
- Human Passport (formerly Gitcoin Passport, now under Holonym/human.tech) — composite scoring.

### Cardano primitives and tooling

- CIP-0381 — BLS12-381 builtins for Plutus V3.
- CIP-0133 — Multi-Scalar Multiplication over BLS12-381 (proposed/accepted, in implementation).
- Modulo-P `ak-381` — Aiken Groth16 verifier library for Plutus V3.
- IOG Halo2-Plutus verifier (August 2025 announcement).
- Hyperledger Identus (formerly Atala PRISM) — DID and verifiable credentials.
- Midnight — Cardano-aligned privacy sidechain.

### Anonymous voting primitives

- Semaphore — Privacy & Scaling Explorations (PSE) group, Ethereum Foundation.
- MACI (Minimum Anti-Collusion Infrastructure) — collusion-resistant anonymous voting.
- Helios voting — homomorphic tallying with verifiability.
- Chaumian blind signatures — original anonymous credential scheme.

---

*Last updated: May 2026.*
