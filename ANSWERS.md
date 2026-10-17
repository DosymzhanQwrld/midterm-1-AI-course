# Midterm 1: answers

GitHub username: dosymzhanqwrld
Session code: maple-66
Fingerprint: 8d45f6fe

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used Claude (Anthropic) as an assistant, before and during the exam. Before the exam I used it to understand the formulas (recall@k, MRR@10, Wilson interval, token cost model, top-p) and to practise on the PRACTICE data. During the exam it helped me write and run a Python script that reads retrieval_ranks.csv and doc_results.csv and computes the metrics, intervals, costs and the Part 3 numbers, and it helped me draft the decision (1d), the next measurement (1e), the memo (2b) and the "Why" lines. I generated the data myself with the exam code, copied the fingerprint, read the results and checked the numbers against the formulas in the README. I can explain every number and decision in the oral check. I used no other tools.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
A	0.5500	0.6500	0.7000	0.6134
B	0.4167	0.6500	0.8667	0.5771

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
A, k=3	0.6500	0.5236	0.7583
A, k=5	0.7000	0.5749	0.8010
B, k=3	0.6500	0.5236	0.7583
B, k=5	0.8667	0.7583	0.9309

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
A, k=3	18099.23
A, k=5	23517.64
B, k=3	12433.59
B, k=5	14074.91

### 1d. Decision (at most 150 words)

Chosen candidate:

Only B fits the $16,100 budget: B,5 costs $14,075 and B,3 $12,434, while A,3 ($18,099) and A,5 ($23,518) are over. Among affordable options B,5 has recall 0.867 [0.758, 0.931] against 0.650 [0.524, 0.758] for B,3. The intervals separate only by a hair (B,3 upper 0.7583, B,5 lower 0.7583), so B,5 is better, but the evidence is thin. I accept $1,641 more per year than B,3 for it. Recall of B,5 by language: EN 0.95, RU 0.85, KK 0.80 (20 questions each, so wide uncertainty). Kazakh is served worst and is also the most expensive per request ($0.0091 vs $0.0044 for English).


### 1e. Next measurement (at most 60 words)

Collect at least 100 more Kazakh test questions (and some Russian) and re-measure recall@5 of B with a Wilson interval. Kazakh is the weakest at 0.80 on only 20 questions, and the B,3 vs B,5 gap barely clears. If Kazakh recall@5 falls below about 0.70, I would revisit chunking or the budget.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
H	0.906	KK, degraded	0.833	0.664	0.927	21845.25
L	0.800	KK, clean	0.700	0.521	0.833	14400.00

Personal-data pages per year sent abroad under H: 689,850

### 2b. Memo (at most 300 words)

To: Head of Operations. Re: invoice pipeline H or L.

Numbers (strict scorer, 30 invoices per cell). H: 90.6 % overall, $21,845 per year. L: 80.0 % overall, $14,400 per year (2 servers). The overall intervals only just separate (H [0.854, 0.940], L [0.736, 0.852]), so H is more accurate, but narrowly.
Worst served. L fails Kazakh scans most: clean 21/30 = 0.700 [0.52, 0.83], degraded 22/30 = 0.733 [0.56, 0.86], Kazakh overall 0.717. H's weakest cell is Kazakh degraded, 25/30 = 0.833 [0.66, 0.93]. The overall figures hide this: under L roughly one Kazakh invoice in four fails. With 30 invoices per cell I am only moderately confident about single cells.
Personal data. 27 % of pages carry a person's name and IIN: 689,850 pages per year would go abroad under H. Law 94-V covers the IIN, and a vendor's claim is not a compliance check. The three questions:

H: where do scans go? Abroad (known). Who stores them? Unknown, need the contract. How long? Unknown, need written retention terms.
L: where? Kazakhstan, our own server (known). Who stores? Our bank (known). How long? Our decision, we must set a rule.

Checkable safeguards: log which pipeline processed each page; automatic deletion of scans and outputs after a fixed period, tested; access control to the server with an audit log; a test with synthetic scans showing nothing leaves our network; human review of low-confidence Kazakh results.
Choice: L. It keeps personal data in Kazakhstan and is $7,445 per year cheaper, at the price of about 10 points of accuracy. Residual risk: Kazakh extraction errors that human review only partly covers, and a retention rule still to be written. This is not legal advice.

## Part 3: Explain your own numbers

### Q1

Number:
Why (at most 40 words):

Number: 2.27 Why (at most 40 words): Output tokens cost 6 times input. Kazakh input grows 2.50 times but output only 1.83 times, so the bill is a price-weighted mix of the two and lands at 2.27, below the 2.45 token ratio.

### Q2

Number:
Why (at most 40 words):
Number: 874 Why (at most 40 words): No. The sentence sits at about token 874, beyond e5's 512-token limit, so its text is never embedded and cannot drive a match. There is no error: the input is silently truncated and retrieval just fails.

### Q3

Number:
Why (at most 40 words):
Number: 78.2 Why (at most 40 words): Top-p cuts the unlikely tail by cumulative probability (keeping Monday, Friday, Sunday) and renormalises, preserving relative shape. Lower temperature rescales all scores, sharpening every probability toward the top token.

### Q4

Number:
Why (at most 40 words):
Number: 11 Why (at most 40 words): The leaderboard prefers X (66 vs 61). With the penalty Y wins (43 vs 32). Accuracy-only scoring never punishes wrong answers, so it teaches models to guess instead of saying "I don't know".

### Q5

Number:
Why (at most 40 words):
Number: 43550 Why (at most 40 words): By turn 25 the first message has been paid for 25 times. Prompt caching bills the repeated history prefix at a discounted rate, but the tokens are still sent and counted, and the history keeps growing.
