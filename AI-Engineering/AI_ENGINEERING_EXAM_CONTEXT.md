# AI Engineering — Exam Context & Progress

**Book:** *AI Engineering: Building Applications with Foundation Models* — Chip Huyen (O'Reilly, 2025).
**Project root:** `/workspace/AI-Engineering/`
**Purpose:** Process each chapter into a 4-file study bundle (study_notes, line_by_line, flashcards_qna, practice_exam), verify the MCQ answer key balances to exactly 10/10/10/10 (a/b/c/d), then push to the private GitHub repo `workid17-cpu/books` via the staging repo `/tmp/opencode/books-push`.

---

## Progress Matrix

| Chapter | Title | Status | Bundle folder |
|---|---|---|---|
| Ch_01 | Introduction to Building AI Applications with Foundation Models | COMPLETE | `chapter_01/` |
| Ch_02 | Understanding Foundation Models | COMPLETE | `chapter_02/` |
| Ch_03 | Evaluation Methodology | PENDING | — |
| Ch_04 | Evaluate AI Systems | PENDING | — |
| Ch_05 | Prompt Engineering | PENDING | — |
| Ch_06 | RAG and Agents | PENDING | — |
| Ch_07 | Finetuning | PENDING | — |
| Ch_08 | Dataset Engineering | PENDING | — |
| Ch_09 | Inference Optimization | PENDING | — |
| Ch_10 | AI Engineering Architecture | PENDING | — |

**Ch_01 and Ch_02 complete; Ch_03–Ch_10 pending. Do not look ahead to unprocessed chapters while working (hard rule).**

---

## Source File IDs (Google Drive folder: "Ai engineering")

Download URL pattern: `https://drive.google.com/uc?export=download&id=<ID>`

| Chapter | Drive File ID | Local PDF | Extracted text |
|---|---|---|---|
| Ch_01 | `16caYbtziT2q769TpAdtH2D0xkgqk4ATB` | `/tmp/opencode/aie_ch01.pdf` (48 pp) | `/tmp/opencode/aie_ch01.txt` (1736 lines) |
| Ch_02 | `120lYlxKCnZ88_wM7cpOzkPGKhGSnbltO` | `/tmp/opencode/aie_ch02.pdf` | `/tmp/opencode/aie_ch02.txt` (2478 lines) |
| Ch_03 | `1KyGH1quBm0U3F6JmEhkh7BQOal7JyoSG` | — | — |
| Ch_04 | `1oYQQgTnKpUKXlOc8SZiHVoltyZJBBVBV` | — | — |
| Ch_05 | `1gGDmbS4D0-4D1NG03LsQ5bg_TM-s6SqY` | — | — |
| Ch_06 | `1zVTP7zPdHEEstvfWqo5evpZqIkFsYNGv` | — | — |
| Ch_07 | `1c2rYxL0sMGfb4TrewTGHyMX4O5CaUV5o` | — | — |
| Ch_08 | `1nhVfZhy0xYFld2Kn03S32ebgr2MZzI9c` | — | — |
| Ch_09 | `1q42Wp-n8hf8oxYWQUHCHyV4izzb1BPRT` | — | — |
| Ch_10 | `1UGbuvcX2jCn1CtKvDZQ9ZFofb8hN6SBI` | — | — |

---

## Bundle File Conventions (same as HOLLM project)

Each chapter folder contains 4 files:

1. **`study_notes.md`** — Core Theme, Key Definitions, Core Concepts & Frameworks, Important Numbers/Stats/Tokens, Algorithms & Formulæ, Diagrams/Visuals, Common Exam Traps, Chapter Summary, Confidence Check + §2 Code & Pseudocode Breakdown (this book has tables/pseudocode, fewer code listings than HOLLM).
2. **`line_by_line.md`** — quoted passage → plain-English explanation + word meanings + technical terms; figures/tables annotated (e.g., Table 1-1, Table 1-5).
3. **`flashcards_qna.md`** — **123 items** (Part 1 Terms→Definitions ~53, Part 2 Short Answer ~30, Part 3 Fill-in-the-Blank ~40). Target range ~115–125.
4. **`practice_exam.md`** — Section A: 40 MCQs (1 pt each) + Section B: 15 True/False + Section C: 10 short-answer + Section D: 5 essays; full answer key with model answers and grading notes; scoring guide.

### MCQ Key Balance Requirement
The Section A answer key MUST contain exactly 10 a's, 10 b's, 10 c's, 10 d's. Verify with:

```python
python3 -c "
import re
from collections import Counter
text = open('<chapter>/practice_exam.md').read()
m = re.search(r'### Section A: Multiple Choice\n(.*?)(?=\n### Section B)', text, re.S)
ans = re.findall(r'^\d+\. ([abcd])$', m.group(1), re.M)
print(len(ans), Counter(ans))
"
```

If unbalanced, reorder options in individual questions (moving the correct option to the needed letter) and update the key — NOT by changing the correct answers themselves.

---

## Chapter 1 Notes (processed)

- **Processed:** 2026-08-13. Source: `16caYbtziT2q769TpAdtH2D0xkgqk4ATB` → `/tmp/opencode/aie_ch01.pdf` (48 pages, `%PDF-1.6`, text-extractable) → `/tmp/opencode/aie_ch01.txt` (1736 lines), fully read.
- **Bundle:** `chapter_01/study_notes.md` (327 lines), `line_by_line.md` (432 lines, 125 items), `flashcards_qna.md` (373 lines, 123 items), `practice_exam.md` (387 lines). MCQ key verified = exact 10/10/10/10 via python; T/F = 8 T / 7 F.
- **Status:** Pushed to `workid17-cpu/books` on branch `main` (commit after `29ac2be`), remote tree verified.

### Ch 1 key content (for bundle generation)
Scale post-2020 (electricity consumption, risk of running out of public internet data); two consequences (more capable models; few orgs afford training → model as a service → AI engineering); self-supervised learning (labels inferred from input vs unsupervised = no labels; enables massive unlabeled corpora → LLMs); Table 1-1 (`<BOS>`/`<EOS>` markers, 6 next-token pairs for "I love street food."); footnote — weights now generically mean all parameters; counterintuitive — larger models need more training data; low entrance barrier (APIs, AI writes code, plain English; Sam Altman Sept 2022 quote re adapting models for apps); 4 open source AI engineering tools reached traction within 2 years (AutoGPT, Stable Diffusion web UI, LangChain, Ollama — more GitHub stars than Bitcoin); use cases — coding (GitHub Copilot >$100M ARR in 2 years; Magic $320M, Anysphere $60M both Aug 2024; gpt-engineer & screenshot-to-code 50k GitHub stars within a year), conversational/voice assistants (Google Assistant, Siri, Alexa; 3D NPCs — NVIDIA Inworld/Convai; Sims/Skyrim), information aggregation (Salesforce 2023 survey: 74% of generative AI users summarize/distill); Figure 1-8 — companies favor internal-facing apps; close-ended tasks (classification) easier to evaluate; competitive advantages = technology, data, distribution (data flywheel; Calendly/Mailchimp/Photoroom as "feature of a bigger product" cases); AI engineering vs ML engineering — 3 differences (1) use pretrained models → focus on adaptation not training, (2) bigger/compute-heavy/latent models → inference optimization + GPU clusters, (3) harder evaluation (open-ended, no exhaustive ground truths); Gemini Dec 2023 MMLU controversy — CoT@32 (32 examples) vs ChatGPT 5-shot; Table 1-5 MMLU scores (Gemini Ultra 90.04% CoT@32 / 83.7% CoT / 71.8% 5-shot; Gemini Pro 79.13% CoT@8 / 86.4% 5-shot reported; GPT-4 87.29% CoT@32 via API; GPT-3.5 70% 5-shot; PaLM 2-L 78.4% 5-shot; Claude 2 78.5% 5-shot / 79.6% CoT (Inflection-2 was 79.6% CoT, Claude 2 78.5% 5-shot per chapter table); Grok 1 73.0% 5-shot; Llama-2 68.0% 5-shot); prompt engineering = desirable behavior from input alone without changing weights; sampling variables deferred to Chapter 2; feedback loops with production data remain from ML engineering; MMLU = Hendrycks et al. 2020; three-layer stack (application development, model development, infrastructure); pre/post/finetuning spectrum; quantization ≠ training; build-product-first (Shawn Wang "The Rise of the AI Engineer", 2023).

## Chapter 2 Notes (processed)

- **Processed:** 2026-08-13. Source: `120lYlxKCnZ88_wM7cpOzkPGKhGSnbltO` → `/tmp/opencode/aie_ch02.pdf` → `/tmp/opencode/aie_ch02.txt` (2478 lines), read in full (lines 1–2478).
- **Bundle:** `chapter_02/study_notes.md` (412 lines), `line_by_line.md` (675 lines, 154 items), `flashcards_qna.md` (378 lines, 123 items = Part1 53 / Part2 30 / Part3 40), `practice_exam.md` (414 lines). MCQ key verified = exact 10/10/10/10 via python; T/F = 8 T / 7 F.
- **Status:** Pushed to `workid17-cpu/books` on branch `main` (commit `3888a2b`, after `4d33404`); remote `main` SHA verified via `git ls-remote` (matches local commit).

### Ch 2 key content (for bundle generation)
Training data: Common Crawl (nonprofit, ~2–3B pages/mo 2022–23), C4 (Google's clean subset); English 45.88% / Russian 5.97% of Common Crawl; Table 2-1 (≥1% langs); Table 2-2 world:CC ratios (Punjabi 231.56 … English 0.40); MMLU = 14,000 Qs / 57 subjects; Project Euler — English >3× Armenian/Farsi, 0/6 Burmese/Amharic; MASSIVE median token length EN 7 / HI 32 / MY 72 → ~10× slower+costlier; NewsGuard: ChatGPT refused 6/7 EN misinformation prompts but 7/7 in Chinese; GPT-2 heuristic = Reddit links ≥3 upvotes; Gunasekar 1.3B on 7B high-quality coding tokens beats bigger models; domain models AlphaFold (~100k proteins), BioNeMo, Med-PaLM2. Modeling: transformer (Vaswani 2017, attention); seq2seq (2014, RNN) two problems (final-hidden-state-only decode + sequential processing); Q/K/V roles; attention formula softmax(QKᵀ/√d)V; Llama 2-7B hidden 4096 / 32 heads / 128 per head; prefill (parallel, K/V state) vs decode (sequential); transformer block = attention (Q/K/V/out-proj matrices) + MLP (linear + activation); ReLU(x)=max(0,x) (GPT-2) vs GELU (GPT-3); Table 2-4 Llama dims (2-7B: 32 blocks/4096/11008/32K/4K … 3-405B: 126/16384/53248/128K/128K); alternatives RWKV, SSMs/S4/H3, Mamba (3B, linear vs quadratic scaling), Jamba (hybrid MoE 52B/12B active, 80GB GPU, 256K ctx). Model size: 3 numbers (params=capacity, training tokens=learning, FLOPs=cost); Llama 3-8B > Llama 2-70B on MMLU; 7B×2B=14GB min; sparsity (90% sparse → 700M non-zero); MoE Mixtral 8x7B = 46.7B total / 12.9B active; dataset tokens vs training tokens (epochs; Llama 1.4T/2T/15T; RedPajama-v2 30T ≈ 450M books ≈ 5,400× Wikipedia); Table 2-5 (LaMDA 137B/168B … Chinchilla 70B/1.4T); FLOP vs FLOP/s vs FLOP/s-day (86,400); H100 NVL 60 TeraFLOP/s; GPT-3-175B 3.14×10²³ FLOPs; 256 H100s ≈ 236 days ≈ 7.8 mo; ~$4M ($4,142,811.43) at 70% util/$2-h; utilization 50% ok / >70% great. Inverse scaling: Anthropic 2022 alignment paradox; Inverse Scaling Prize (99 submissions, 11 third prizes, no second/first). Scaling law: Chinchilla ~20 tokens/param (3B→60B), scale size+tokens equally; Sardana inference-adjusted; last mile (3.4→2.8 nats = 10× data). Scaling extrapolation: 40M→6.7B transfer; 10 hyperparams = 1,024 combos; emergent abilities. Bottlenecks: data (Villalobos; Grok/AI-generated data; model collapse; proprietary data; C4 28%→45% restricted; Reddit/SO term changes) + electricity (1–2% → 4–20% by 2030; ~50× cap); Dario Amodei $100B model. Post-training: two steps (SFT + preference); InstructGPT 98% pre / 2% post; Shoggoth analogy; SFT = demonstration data (behavior cloning), labelers ~90% college, >1/3 master's, 30 min/pair, $10 → 13,000 pairs ≈ $130k; LAION (13,500 volunteers, 90% male); Gopher [A]/[B] heuristics. Preference: RLHF (reward model + optimize; PPO 2017); DPO (Llama 3); RLAIF; comparison data (prompt, winning, losing); LMSYS 3–5 min, $3.50 vs $25; InstructGPT UI scores 1–7 but only ranking used; 73% agreement; 3 ranked → 3 pairs; loss = −log(σ(rθ(x,y_w) − rθ(x,y_l))); weak model can judge strong; best of N (Stitch Fix, Grab). Sampling: logits→softmax; greedy; temperature (logits [1,2]: T=1 [0.27,0.73], T=0.5 [0.12,0.88]; 0.7 creative; providers 0–2; T=0 = arg max); logprobs (underflow; sum; average logprob; OpenAI best_of; top-20 limit; Anthropic none); top-k (50–500, cuts softmax); top-p/nucleus (0.9–0.95, dynamic, no softmax cut); min-p; stopping conditions. Test time compute: best of N; beam search; diversity; 2 outputs ≈ 2× cost; selection = max prob/avg logprob, verifiers (≈30× size boost; 100M+verifier ≈ 3B), heuristics; OpenAI peak 400, Stanford log-linear to 10,000; TIFIN parallel latency; self-consistency (Gemini 32 samples on MMLU); robustness (image extraction 3 tries). Structured outputs: semantic parsing (text-to-SQL, text-to-regex); downstream JSON; guidance/outlines/instructor/llama.cpp; JSON mode = syntax not content, truncation risk; 5 layers (prompting/post-processing/test-time compute = bandages; constrained sampling/finetuning = intensive); LinkedIn defensive YAML 90→99.99% (chose YAML, less verbose); classifier head = feature-based transfer. Probabilistic nature: inconsistency (2 scenarios; cache/fix sampling vars/fix seed; hardware); hallucination (law firm fine June 2023; 2016 Goyal; self-delusion Ortega/DeepMind + snowballing Zhang; mitigations RL obs-vs-actions + SL factual/counterfactual; mismatched knowledge Leo Gao + Schulman verification/RL; InstructGPT RLHF worsened hallucinations yet preferred). Summary: workflows around probabilistic nature → evaluation pipeline (Ch 3–4).

---

## Repository & Push Setup

- **Remote repo:** `https://github.com/workid17-cpu/books.git` (private; branch `main`).
- **Staging repo:** `/tmp/opencode/books-push` — used for ALL commits/pushes (`.git` origin points to the repo). Latest commit on `main` before Ch_01 push: `29ac2be`.
- **Local `/workspace/.git`:** no remotes; NOT used for pushes.
- **Push workflow:**
  1. Copy new/updated bundle folder(s) into `/tmp/opencode/books-push/AI-Engineering/`.
  2. `git add -A && git commit -m "..."`.
  3. `git push origin main`.
  4. Verify: `gh api repos/workid17-cpu/books/git/trees/main?recursive=1` (check the new blobs/tree exist).
- **Credentials:** GitHub PAT stored in env var `GH_TOKEN`. NEVER echo the token value in any output.
- **No-delete rule:** use `mv` to backups (e.g., `/tmp/opencode/<name>_backup/`) instead of `rm`.

---

## Session Log

- **Session 1 (2026-08-13):** Created `/workspace/AI-Engineering/` project. Downloaded Ch 1 PDF (ID `16caYbtziT2q769TpAdtH2D0xkgqk4ATB`; `%PDF-1.6`, 48 pages), extracted text (1736 lines), read in full. Generated complete Ch 1 bundle (study_notes 327 lines / line_by_line 432 lines / flashcards 123 items / practice_exam with MCQ key verified 10/10/10/10, T/F 8T/7F). Created this context file. Pushed `chapter_01/` to `workid17-cpu/books` (branch `main`), remote verified.
- **Session 2 (2026-08-13):** Downloaded Ch 2 PDF (ID `120lYlxKCnZ88_wM7cpOzkPGKhGSnbltO`), extracted text (2478 lines), read in full. Generated complete Ch 2 bundle (`chapter_02/`): study_notes 412 lines, line_by_line 675 lines (154 items), flashcards 123 items (Part1 53 / Part2 30 / Part3 40), practice_exam 414 lines (MCQ key verified 10/10/10/10, T/F 8T/7F). Updated this context file (Progress Matrix, Ch_02 Notes, Session Log, Resume Instructions). Pushed `chapter_02/` to `workid17-cpu/books` (branch `main`) as commit `3888a2b`; remote verified via `git ls-remote` SHA match. (Note: `gh api`/GH_TOKEN unavailable this session; `git push` success + remote ref SHA confirmed instead.)

---

## Resume Instructions

Next action: **process Chapter 3 (Evaluation Methodology, ID `1KyGH1quBm0U3F6JmEhkh7BQOal7JyoSG`)**

1. Download: `curl -L -o /tmp/opencode/aie_ch03.pdf 'https://drive.google.com/uc?export=download&id=1KyGH1quBm0U3F6JmEhkh7BQOal7JyoSG'`; check `%PDF-` header; `pdftotext` → `/tmp/opencode/aie_ch03.txt`; read full text.
2. Create `chapter_03/`, write the 4 bundle files per conventions.
3. Verify MCQ key = 10/10/10/10 and T/F count; adjust if needed.
4. Update this context file (Progress Matrix, Ch_03 Notes, Session Log).
5. Copy to `/tmp/opencode/books-push/AI-Engineering/`, commit, push `main`, verify via `gh api`.
6. Report; offer next chapter.

## Commands to Continue

- Download chapter PDF (replace ID): `curl -L -o /tmp/opencode/aie_chNN.pdf 'https://drive.google.com/uc?export=download&id=<ID>'`
- Extract text: `pdftotext /tmp/opencode/aie_chNN.pdf /tmp/opencode/aie_chNN.txt`
- Check PDF header: `head -c 8 /tmp/opencode/aie_chNN.pdf`
- Key balance check: (see python snippet above)
- Remote verify: `gh api repos/workid17-cpu/books/git/trees/main?recursive=1`
