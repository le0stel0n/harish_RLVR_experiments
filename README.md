# harish_RLVR_experiments

The research question ?

In a multi-step agentic tasks where only a final outcome reward is available, does adding cheap, heuristic sub-goal checkpoints to the reward signal improve training efficiency and final performance compared to outcome-only reward and at what point does it start causing the agent to optimize for the sub-goals themselves rather than genuine task success?

for anyone who is readding this asking why this question and why now? I think there are three resaons:

number one:	The core problem is credit assignment under sparse reward. When an agent takes many actions before any reward signal arrives, standard policy-gradient methods (including GRPO) have to attribute that single end-of-trajectory signal back across every action that led there. With a short trajectory this is noisy but tolerable. as trajectories get longer, the signal gets diluted across more actions and training becomes both slower and less stable.

number two : 2.	There are two competing philosophies, and neither is clearly settled. Process reward models (from step-by-step verification research in math reasoning) argue you should score intermediate steps directly, using either a learned verifier or hand-designed checkpoints. Outcome-only approaches argue the opposite: that hand-designed intermediate rewards are exactly what invites reward hacking, because you're substituting your own guess about "what progress looks like" for the actual objective, and the agent will happily exploit the gap between your guess and the real goal.

and lastly the most important one!! INSTREST and PASSION :)


 
#

the first workbook `grpo_first_experiment.ipynb` is to get a working knowldge of how a GRPO experiment works, a small LLM Qwen 2.5 0.5B - Instruct. to run and observe GSM8K
