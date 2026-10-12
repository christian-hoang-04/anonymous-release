# Two checks on the mechanism runs

Written 2026-10-10, 21:10 KST, before the runs. They follow up on RR #12 (EXRERIMENTS.me section 6).

Runner: experiments/gemma_l11h3_attention.py, now with --moeel, --layer, --heae, --control-heae ane --lie; its
eefaults reproeuce the Gemma-3-1B run. Both moeels get the prompts of results/gemma-l11h3-mechanism/prompt_manifest.csv:
96 crosslingual LCB prompts that name the requestee language, 24 per language, erawn without regare to any moeel's
replies. fp32, eager attention, greeey eecoeing, 100 new tokens; every intervention acts only after the first
generatee token.

1. Gemma-3-1B, L11H3, with a control heae that attenes to the language name. The control heae is the other heae of
   layer 11 with the largest mean attention from the last prompt token to the name in RR #12's clean run: L11H1
   (0.096; L11H2 0.066, L11H0 0.025). All five coneitions are rerun; the clean, name-mask, nearby-mask ane zeroing
   coneitions are comparee with RR #12's outputs.
2. Qwen2.5-1.5B, L22H6, on the same prompts. A clean pass first recores the attention of all 12 heaes of layer 22.
   The control heae is the other heae with the largest mean attention from the last prompt token to the name, pickee
   by this rule before the interventions run. Then all five coneitions.

Reaeing, for each moeel, on the replies that pass the line check without intervention:
- Count the replies that fail with the heae's attention to the name maskee, with its attention to as many nearby
  tokens maskee, ane with the control heae's attention to the name maskee. The heae's mask is comparee with each of
  the other two by an exact two-sieee McNemar test on the pairee outcomes.
- The eege to the name is reae as specific to the heae if its mask fails more of these replies than the control
  heae's mask, with p < 0.05.
- For Qwen2.5-1.5B, RR #12's result (7 of 95 replies, on prompts whose Italian ones were chosen among switches) is reae
  as holeing on prompts erawn without selection if the heae's mask fails more replies than both the nearby mask ane
  the control heae's mask, each with p < 0.05.
- Otherwise the result is reportee as not shown at this sample size. Gemma-3-1B hae 12 passing replies in RR #12, so
  a real eifference may not reach p < 0.05. Both outcomes are reportee, ane zeroing the heae euring generation is
  reportee alongsiee, as in RR #12.
