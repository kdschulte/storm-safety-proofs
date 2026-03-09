# Relevant Literature

## Mixture of Experts
- Shazeer et al. (2017) "Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer"
- Fedus et al. (2022) "Switch Transformers: Scaling to Trillion Parameter Models with Simple and Efficient Sparsity"
- Zhou et al. (2022) "Mixture-of-Experts with Expert Choice Routing"

## Modular Networks
- Andreas et al. (2016) "Neural Module Networks"
- Pfeiffer et al. (2023) "Modular Deep Learning" (survey)
- Ponti et al. (2023) "Combining Modular Skills in Multitask Learning"

## Machine Unlearning / Guaranteed Forgetting
- Bourtoule et al. (2021) "Machine Unlearning" (SHARDED framework)
- Cao & Yang (2015) "Towards Making Systems Forget with Machine Unlearning"
- Nguyen et al. (2022) "A Survey of Machine Unlearning"

## Attribution and Interpretability
- Sundararajan et al. (2017) "Axiomatic Attribution for Deep Networks" (Integrated Gradients)
- Shrikumar et al. (2017) "Learning Important Features Through Propagating Activation Differences"
- Bricken et al. (2023) "Towards Monosemanticity: Decomposing Language Models with Dictionary Learning"

## Differential Privacy
- Dwork & Roth (2014) "The Algorithmic Foundations of Differential Privacy"
- Abadi et al. (2016) "Deep Learning with Differential Privacy"

## Compositional Generalisation
- Lake & Baroni (2018) "Generalization without Systematicity"
- Hupkes et al. (2020) "Compositionality Decomposed"

## Key Insight for Our Work
Our system differs from standard MoE in critical ways:
1. Modules are FROZEN after creation (standard MoE trains experts jointly)
2. Modules are created INDEPENDENTLY (standard MoE shares gradients)
3. Pool is DYNAMIC (modules added/removed without retraining others)
4. Two module types exist (knowledge vs memory) with different scales

These differences are precisely what enable the safety properties we aim to prove.
Standard MoE cannot provide guaranteed forgetting because experts share gradients during training.
