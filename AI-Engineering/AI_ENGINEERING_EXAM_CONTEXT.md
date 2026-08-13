# AI Engineering — Exam Context & Progress

**Book:** *AI Engineering: Building Applications with Foundation Models* — Chip Huyen (O'Reilly, 2025).
**Project root:** `/workspace/AI-Engineering/`
**Purpose:** Process each chapter into a 4-file study bundle (study_notes, line_by_line, flashcards_qna, practice_exam), verify the MCQ answer key balances to exactly 10/10/10/10 (a/b/c/d), then push to the private GitHub repo `workid17-cpu/books` via the staging repo `/tmp/opencode/books-push`.

---

## Progress Matrix

| Chapter | Title | Status | Bundle folder |
|---|---|---|---|
| Ch_01 | Introduction to Building AI Applications with Foundation Models | COMPLETE | `chapter_01/` |
| Ch_02 | Understanding Foundation Models | PENDING | — |
| Ch_03 | Evaluation Methodology | PENDING | — |
| Ch_04 | Evaluate AI Systems | PENDING | — |
| Ch_05 | Prompt Engineering | PENDING | — |
| Ch_06 | RAG and Agents | PENDING | — |
| Ch_07 | Finetuning | PENDING | — |
| Ch_08 | Dataset Engineering | PENDING | — |
| Ch_09 | Inference Optimization | PENDING | — |
| Ch_10 | AI Engineering Architecture | PENDING | — |

**All 10 chapters pending except Ch_01. Do not look ahead to unprocessed chapters while working (hard rule).**

---

## Source File IDs (Google Drive folder: "Ai engineering")

Download URL pattern: `https://drive.google.com/uc?export=download&id=<ID>`

| Chapter | Drive File ID | Local PDF | Extracted text |
|---|---|---|---|
| Ch_01 | `16caYbtziT2q769TpAdtH2D0xkgqk4ATB` | `/tmp/opencode/aie_ch01.pdf` (48 pp) | `/tmp/opencode/aie_ch01.txt` (1736 lines) |
| Ch_02 | `120lYlxKCnZ88_wM7cpOzkPGKhGSnbltO` | — | — |
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

---

## Resume Instructions

Next action: **process Chapter 2 (Understanding Foundation Models, ID `120lYlxKCnZ88_wM7cpOzkPGKhGSnbltO`)**

1. Download: `curl -L -o /tmp/opencode/aie_ch02.pdf 'https://drive.google.com/uc?export=download&id=120lYlxKCnZ88_wM7cpOzkPGKhGSnbltO'`; check `%PDF-` header; `pdftotext` → `/tmp/opencode/aie_ch02.txt`; read full text.
2. Create `chapter_02/`, write the 4 bundle files per conventions.
3. Verify MCQ key = 10/10/10/10 and T/F count; adjust if needed.
4. Update this context file (Progress Matrix, Ch_02 Notes, Session Log).
5. Copy to `/tmp/opencode/books-push/AI-Engineering/`, commit, push `main`, verify via `gh api`.
6. Report; offer next chapter.

## Commands to Continue

- Download chapter PDF (replace ID): `curl -L -o /tmp/opencode/aie_chNN.pdf 'https://drive.google.com/uc?export=download&id=<ID>'`
- Extract text: `pdftotext /tmp/opencode/aie_chNN.pdf /tmp/opencode/aie_chNN.txt`
- Check PDF header: `head -c 8 /tmp/opencode/aie_chNN.pdf`
- Key balance check: (see python snippet above)
- Remote verify: `gh api repos/workid17-cpu/books/git/trees/main?recursive=1`
