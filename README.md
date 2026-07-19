# harish_RLVR_experiments

The research question ?

In a multi-step agentic tasks where only a final outcome reward is available, does adding cheap, heuristic sub-goal checkpoints to the reward signal improve training efficiency and final performance compared to outcome-only reward and at what point does it start causing the agent to optimize for the sub-goals themselves rather than genuine task success?

for anyone who is reading this asking why this question and why now? I think there are three resaons:

number one:	The core problem is credit assignment under sparse reward. When an agent takes many actions before any reward signal arrives, standard policy-gradient methods (including GRPO) have to attribute that single end-of-trajectory signal back across every action that led there. With a short trajectory this is noisy but tolerable. as trajectories get longer, the signal gets diluted across more actions and training becomes both slower and less stable.

number two : 2.	There are two competing philosophies, and neither is clearly settled. Process reward models (from step-by-step verification research in math reasoning) argue you should score intermediate steps directly, using either a learned verifier or hand-designed checkpoints. Outcome-only approaches argue the opposite: that hand-designed intermediate rewards are exactly what invites reward hacking, because you're substituting your own guess about "what progress looks like" for the actual objective, and the agent will happily exploit the gap between your guess and the real goal.

and lastly the most important one!! INSTREST and PASSION :)


 
#

The first workbooks `grpo_first_experiment.ipynb` , `grpo_first_experiment_larger_groups_size.ipynb`

**Goal:** Get hands-on with GRPO training end-to-end on a small model and a single-turn math task, using a purely verifiable (RLVR-style) reward.

**Model:** Qwen/Qwen2.5-0.5B-Instruct, fine-tuned via LoRA (r=16, targeting q/k/v/o attention projections). ~0.44% of parameters trainable (2.16M / 496M).

**Dataset:** GSM8K, 500-example training subset, 10-example held-out test set.


## Overall Conclusions

1. The larger group size (8 vs. 4) measurably fixed the zero-reward-variance problem predicted from the base model's 8% zero-shot accuracy on this task — confirmed directly via `reward_std` and `frac_reward_zero_std` metrics, not just inferred.
2. The apparent "training-induced degeneration" in Run 1 was a false conclusion.The actual cause was a generation-configuration bug (stale training-mode settings applied at inference time), unrelated to training dynamics. This was only caught by cross-checking multiple independent signals — training-time completion logs vs. separate held-out generation — rather than trusting a single observation.
3. Methodological lesson: a model producing bad output at evaluation time does not by itself confirm the training run was the cause. Always verify whether an inference-time configuration issue could produce the same symptom before attributing a failure to the training process itself.


