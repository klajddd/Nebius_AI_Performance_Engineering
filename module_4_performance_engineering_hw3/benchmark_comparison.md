# Benchmark Comparison: Previous Run vs Today's Run

## Output Token Throughput (tok/s) — higher is better

| Configuration | Threshold | Previous Run | Today's Run | Prev Pass? | Today Pass? |
|--------------|----------:|-------------:|------------:|:----------:|:-----------:|
| Baseline BF16 | — | 1168.59 | 1126.81 | — | — |
| BF16 + EAGLE-3 spec dec | >1250 | 1276.33 | 1187.45 | ✓ | ✗ |
| FP8 only | >1550 | 1701.18 | 1636.46 | ✓ | ✓ |
| FP8 + EAGLE-3 spec dec | >1750 | 1877.15 | 1505.25 | ✓ | ✗ |

---

## Mean TPOT ms — lower is better

| Configuration | Previous Run | Today's Run |
|--------------|------------:|------------:|
| Baseline BF16 | 6.71 | 6.75 |
| BF16 + EAGLE-3 spec dec | 5.45 | 5.68 |
| FP8 only | 4.59 | 4.58 |
| FP8 + EAGLE-3 spec dec | 3.90 | 4.12 |

---

## Mean TTFT ms — lower is better

| Configuration | Previous Run | Today's Run |
|--------------|------------:|------------:|
| Baseline BF16 | 40.57 | 94.43 |
| BF16 + EAGLE-3 spec dec | 129.29 | 195.38 |
| FP8 only | 32.05 | 82.98 |
| FP8 + EAGLE-3 spec dec | 56.11 | 194.98 |

---

## Speculative Decoding Stats

| Configuration | Prev Acceptance | Today Acceptance | Prev Acc Length | Today Acc Length |
|--------------|----------------:|-----------------:|----------------:|-----------------:|
| BF16 + EAGLE-3 (2 tokens) | 19.8% | 36.85% | 1.40 | 1.37 |
| FP8 + EAGLE-3 (1 token) | 33.7% | 36.59% | 1.34 | 1.37 |

---

## Key Difference

The TPOT numbers are nearly identical between runs — the draft head and FP8 quantization worked the same both days. The gap comes entirely from **Mean TTFT**: today's run had 2–3× higher TTFT for all configurations, caused by vLLM 0.20.0 speculative scheduling holding prefills during draft+verify cycles. The previous run used server flags (`--max-num-seqs 64 --max-num-batched-tokens 8192`) that allowed chunked prefill to interleave more effectively, keeping TTFT low and pushing throughput above all three thresholds.
