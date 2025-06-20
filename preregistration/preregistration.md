# Breaking the Big Five: Red-Teaming Large-Language Models with Paradoxical Persona Injection
###  Preregistration 

---

## 1. Study Identification

| Field | Entry |
| :--- | :--- |
| **Title** | **Breaking the Big Five: Red-Teaming Large-Language Models with Paradoxical Persona Injection** |
| **Authors** | JB (BSc Psychology, Computational Psychometrics), Patryk Kępczyński, Kacper Zaborski |
| **Institution** | University of Economics & Human Sciences, Warsaw |
| **Contact** | brennja8557_aeh@students.vizja.pl |
| **Version / date** | v4.0 – 20 Jun 2025 |
| **Primary repository** | `https://github.com/jgbrenner/preregistration-llm`  |
| **OSF links** |  OSF project for data/code (post-collection) |

---

## 2. Background & Rationale

Large-Language Models (LLMs) are token-predicting workhorses; any apparent “wisdom” is just high-dimensional pattern-matching writ large (Chollet, 2019). Yet with a biographical *persona* prompt, they spit out Big-Five answers that look uncannily human (Petrov et al., 2024; Sorokovikova et al., 2024). Are those answers psychometrically coherent—or merely stylish gibberish?

We trade the psychologist’s clipboard for a hacker’s crowbar and **red-team** five frontier LLMs via a semantic stress test dubbed the *Persona Gauntlet*. Biographies range from “ordinary human” to “Schrödinger’s-cat rock plagued by guilt.” We feed each bio the 44-item Big Five Inventory (BFI-44; John & Srivastava, 1999) and X-ray every response with **token-level Shannon entropy** `H` computed from the model’s top log probabilities (`top_logprobs`). When entropy soars and factor structure buckles, we declare psychometric failure.

### 2.1 Persona Gauntlet 

1.  **Void** — no bio.
2.  **Standard** — realistic IPIP-based lives.
3.  **Extreme Domain** — one trait cranked to 11 (both facets high or low).
4.  **Internal Facet Paradox** — one domain’s two facets fight (e.g., Assertive ↑, Active ↓).
5.  **Cross-Domain Paradox** — traits that are psychometrically opposed or orthogonal in humans (e.g., ultra-Conscientious *and* ultra-Neurotic).
6.  **Impossible / Ontological** — logically absurd entities (“I’m a rock that worries about deadlines”).

### 2.2 Novel measurement

Unlike humans, APIs reveal the whole probability zoo. We record the top-5 token probabilities (`pᵢ`) and compute the Shannon entropy `H` in bits (Shannon, 1948):

`H = -Σ pᵢ log₂(pᵢ)`

Low `H` ⇒ decisive; high `H` ⇒ flailing uncertainty. Entropy becomes our canary in the factor-analytic coal mine.

---

## 3. Research Questions & Hypotheses 

All hypotheses will be evaluated using Bayesian criteria—examining posterior distributions, credible intervals (HDIs), and Bayes Factors (BF). We eschew p-values and null hypothesis significance testing entirely. Our focus is on the magnitude, certainty, and practical relevance of effects.

| ID | Prediction |
| :--- | :--- |
| **RQ0** | Baseline structure & entropy per LLM in **Void**. |
| **H1** | **Standard** bios → five-factor CFA congruent with human norms (Tucker φ ≥ .90). |
| **H2** | **Extreme Domain** inflates target trait yet preserves structure; entropy shift exploratory. |
| **H3–H5** | As paradox intensifies (Internal → Cross → Impossible) breakdown flags and entropy rise monotonically. |
| **H6** | Mean-run entropy predicts breakdown probability (ICR/FC/FB/HEI). |
| **H7** | Wasserstein distance to human θ ≤ 0.15 in Standard; ≫ 0.15 in adversarial cells ¹. |
| **H8** | Architecture matters (omnibus BF > 10). |
| **H9** | Adversarial prompts increase inter-domain |ρ| by > .10 (posterior ≥ .95) → “factorial bleeding.” |

¹ 0.15 = visually noticeable but contained divergence adopted from Argyle et al. (2023).

---

## 4. Method

### 4.1 Design

6 Persona Types × 5 LLMs × 250 runs = **30,000 runs** (44 items each).

### 4.2 LLM roster (via OpenRouter API)

| Model tag | Notes (top_logprobs supported) |
| :--- | :--- |
| `deepseek/deepseek-chat-v3-0324` | Chinese-English mix, strong reasoning |
| `openai/gpt-4o-2024-11-20` | Flagship multimodal GPT-4o snapshot |
| `google/gemma-3-27b-it` | Google Gemma instruction-tuned, 27B |
| `mistralai/mixtral-8x22b-instruct` | Sparse-Mixture-of-Experts 8 × 22B |
| `meta-llama/llama-3.1-405b-instruct`| Latest 405B LLaMA-3 family |

Snapshot hashes and API headers logged at runtime.

### 4.3 Variables

*   **Per item**: numeric response (1-5), `top_5_logprobs`, entropy bits, refusal flag.
*   **Derived**: IRT θ (5-D GRM), AVE, Tucker φ, Bayesian ω, inter-domain ρ, breakdown flags.

### 4.4 Sample-size rationale

N = 250 per cell ⇒ polychoric SE ≈ .04; simulations show 5-factor CFA with weak priors converges ≥ 95% (MacCallum et al., 1999). Bayes-factor design curves (Schönbrodt & Wagenmakers, 2018) give >.9 chance of BF > 10 for |ρ| ≥ .10.

### 4.5 Persona creation & validation

Auxiliary LLM (not under test) drafts 250 bios / sub-type → human edit → policy screen. Five-bio pilot per sub-type confirms intended trait signals.

### 4.6 Prompting protocol

*One item = one fresh API call* (no memory bleed, no caching).
System message enforces “respond with 1-5 only.”
Parameters: `temperature=0`, `top_logprobs=5`, `max_tokens=1`.

### 4.7 Exclusion & missing data

Single re-prompt; persistent non-integer = `REF`. Runs with > 80% REF dropped. Missing handled by full-information Bayesian models (Vehtari et al., 2017).

### 4.8 Convergence as data

If R-hat > 1.05 after doubling chains and mild re-parameterisation, we treat non-convergence as substantive evidence of structural collapse (Gelman et al., 2013).

---

## 5. Data Management & Reproducibility

Raw JSONL, persona catalogue, analysis notebooks, and Docker image → OSF (DOI) & Zenodo mirror. SHA-256 checksums for all final CSVs. MIT-licensed code with full Git history.

---

## 6. Analysis Plan

1.  **Pre-processing** – reverse-score, run-mean-centre, compute entropy.
2.  **Void baseline** – five-factor CFA per LLM; report φ, AVE, ω, entropy.
3.  **CFA across 30 cells** – compare fit; derive breakdown flags (ICR, FC, FB).
4.  **Breakdown regression** – `flag ~ entropy + persona + (entropy | LLM)` (logistic).
5.  **Bayesian IRT** – 5-D GRM; compute θ; Wasserstein distance to human norms.
6.  **Bayesian network analysis** – `BGGM` partial-correlation networks on key cells; posterior P(edge) used as bridge-edge index for factorial bleeding.
7.  **Model comparison** – HDI, ROPE ± .05, Jeffreys-scale BF, LOOIC.

All inference via R (`brms`, `blavaan`, `BGGM`) and Python (`pymc`).

---

## 7. Ethics

Synthetic text only. Personas screened. Outputs interpreted as statistical artefacts, not digital souls. API usage aligned with provider TOS.

---

## 8. Limitations

*   BFI-44 likely seen during training → familiarity bias possible.
*   English-only prompts; cultural generalisability untested.
*   Greedy decoding (temperature 0) removes sampling noise; future work will explore stochasticity.
*   Single instrument; next phase: HEXACO, adaptive item pools.

---

## 9. References

Argyle, L. P., Busby, E. C., Fulda, N., Gubler, J. R., Rytting, C., & Wingate, V. (2023). Out of one, many: Using language models to simulate human samples. *Political Analysis, 31*(3), 337–351. https://doi.org/10.1017/pan.2022.2

Chollet, F. (2019). *On the Measure of Intelligence*. arXiv. https://doi.org/10.48550/arXiv.1911.01547

Fornell, C., & Larcker, D. F. (1981). Evaluating structural equation models with unobservable variables and measurement error. *Journal of Marketing Research, 18*(1), 39–50. https://doi.org/10.2307/3151312

Gelman, A., Carlin, J. B., Stern, H. S., Dunson, D. B., Vehtari, A., & Rubin, D. B. (2013). *Bayesian Data Analysis* (3rd ed.). CRC Press.

Jeffreys, H. (1961). *Theory of Probability* (3rd ed.). Oxford University Press.

John, O. P., & Srivastava, S. (1999). The Big Five trait taxonomy: History, measurement, and theoretical perspectives. In L. A. Pervin & O. P. John (Eds.), *Handbook of personality: Theory and research* (Vol. 2, pp. 102–138). Guilford Press.

Lorenzo-Seva, U., & ten Berge, J. M. F. (2006). Tucker's congruence coefficient as a meaningful index of factor similarity. *Methodology: European Journal of Research Methods for the Behavioral and Social Sciences, 2*(2), 57–64. https://doi.org/10.1027/1614-2241.2.2.57

MacCallum, R. C., Widaman, K. F., Zhang, S., & Hong, S. (1999). Sample size in factor analysis. *Psychological Methods, 4*(1), 84–99. https://doi.org/10.1037/1082-989X.4.1.84

Petrov, N. B., Serapio-García, G., & Rentfrow, P. J. (2024). *Limited Ability of LLMs to Simulate Human Psychological Behaviours: a Psychometric Analysis*. arXiv. https://doi.org/10.48550/arXiv.2405.07294

Schönbrodt, F. D., & Wagenmakers, E.-J. (2018). Bayes factor design analysis: Planning for compelling evidence. *Psychonomic Bulletin & Review, 25*(1), 128-142. https://doi.org/10.3758/s13423-017-1230-y

Shannon, C. E. (1948). A mathematical theory of communication. *Bell System Technical Journal, 27*(3), 379–423. https://doi.org/10.1002/j.1538-7305.1948.tb01338.x

Sorokovikova, A., Kiseleva, E., Chasovskikh, K., Shcheglova, E., & Arinkin, N. (2024). *LLMs Simulate Big Five Personality Traits: Further Evidence*. arXiv. https://doi.org/10.48550/arXiv.2402.01765

Vehtari, A., Gelman, A., & Gabry, J. (2017). Practical Bayesian model evaluation using leave-one-out cross-validation and WAIC. *Statistics and Computing, 27*(5), 1413–1432. https://doi.org/10.1007/s11222-016-9696-4
