We thank the reviewers for their constructive feedback. Our responses are as follows:

# Response to Reviewer A

## Q1. Initial Fine-Tuning Setup.

We employed separate fine-tuning for each task and PL (e.g., Python, Java), training independent models rather than
using multi-lingual or multi-task fine-tuning. This ensures each model achieves optimal performance for each task.

## Q2. Numbers in parenthesis in all tables.

The values in parentheses in all tables represent the normalized accuracy, as indicated in the table headers (“N.
Acc.”). The formal definition of this metric is provided in Section 3.4 of our paper.
Specifically, this metric measures the “Performance Retention Rate” of the merged model relative to the original
fine-tuned models. A higher value indicates that the fusion method more effectively preserves the original performance.

# Response to Reviewer B

## Q1. Errors introduced by fusion.

## Q2. Alternative scaling strategies and different normalization choices.

There are several possible choices for normalization:1.scaling based on standard deviation;2. scaling based on the maximum absolute value.
Due to time constraints, we only present results for a subset of tasks under the multi-task multi-PL setting, as shown below. The results indicate that the performance differences among different normalization strategies are similar, and all of them preserve 100% of the original fine-tuned model’s performance.

| N. choices | Code search (MRR) |Code Generation (Pass@5)| Vul. Detection (Accuracy) |Precision|Recall| F1|
|----|-----|-----|-------------------------|-----|-----|---|
|Mean Abs.|74.63|67.20| 92.42|48.98|22.57| 30.90 |
|Std.|74.62|67.90|92.48|50.00| 22.94|31.45|
|Max Abs.|74.59|67.00|92.60|51.70|22.57|31.42|


The table below shows the scaling factors produced by different strategies across tasks. As can be seen, their scaling factors are quite similar, indicating that any reasonable normalization strategy can achieve satisfactory results. However, we recommend using the mean absolute value or the standard deviation for scaling, as the maximum absolute value may be unstable when it is an outlier.

|N. choices|Code Sum.|Clone Detection|Code Repair|Code Gen.|Code Search|Vul. Detection|
|:---|---|---|---|---|---|---|
|Mean Abs.|1.0|1.61|1.73|1.85|1.97|2.71|
|Std.|1.0|1.62|1.73|1.85|1.97|2.68|
|Max Abs.|1.0|1.75|1.79|1.85|2.06|2.55|


# Response to Reviewer C

## Q1. Comparison with state-of-the-art tools.

[//]: # (We thank the reviewer for this insightful question. Our work is an empirical study focused on evaluating recent model fusion techniques for Software Engineering tasks. The central aim is to assess how well these methods preserve the capabilities of the original, task-specific models. Therefore, the most direct and relevant baseline for our research question is the performance of the original fine-tuned models.)

[//]: # ()

[//]: # (This baseline choice is well-justified. As evidenced by prior literature &#40;e.g., [1, 2, 3]&#41;, fine-tuned Large Language Models have demonstrated promising performance and often surpass traditional non-LLM SOTA methods on many SE tasks. In our experiments, the performance of our fine-tuned models is comparable to that reported in these studies &#40;e.g., in code search [4] and summarization [Citation2]&#41;. This confirms that our fine-tuned models achieve a strong and credible level of performance, thereby serving as a valid reference point.)

[//]: # ()

[//]: # ()
Our study focuses on model fusion techniques, whose goal is enabling a single model to handle multiple software
engineering tasks simultaneously. Therefore, in our experiments, we primarily care about how well the merged model can
preserve the performance of the original fine-tuned models across tasks, rather than whether it achieves the absolute
best performance on any single task.

In principle, for each SE task, any the SOTA model could be used as an input to our fusion method. In practice, prior
work has shown that fine-tuning large models on SE tasks is highly promising and often outperforms traditional SOTA
methods. Consistent with these findings, the fine-tuned models in our experiments achieve performance close to the
current researchs (as supported by our results). Thus, fine-tuned models can, to a certain extent, be regarded as
representative of SOTA in our setting.

[//]: # (我们研究的对象是模型融合技术，目的是使一个模型能够同时胜任多个软件工程任务，因此我们实验中主要关注的指标是融合后模型在多大程度上能够保留原有模型的效果，而不是模型在单个任务上是否能达到最优效果。理论上，每个软工任务中效果最好的模型都可以是我们方法的融合对象；实际上，在我们的实验中，我们微调出来的模型也达到了现有最优方法的表现（佐证），因此，微调模型一定程度上可以代表SOTA。)

## Q2. Implications or recommendations for SE researchers and developers.

We thank the reviewer for this insightful question regarding the practical impact of our work. The implications and
recommendations of our study provides as follows:

1. Problem & Solution: Fine-tuning LLMs for specific SE tasks with different PLs has gradually become a new paradigm.
   However, this approach leads to the generation of numerous fine-tuned model checkpoints for each task and PL.
   Deploying all these checkpoints simultaneously creates a significant demand for GPU memory, complicating the
   management processes. Model fusion technique offers a direct and practical solution by fusing multiple task-specific
   checkpoints into a single, memory-efficient model.

2. Implications & Recommendations: While model fusion techniques have been proposed in recent years, these methods have
   primarily been evaluated in NLP and computer vision (CV) tasks, with little known about their effectiveness in the SE
   domain. Our work presents the first systematic evaluation of these fusion techniques for SE tasks. We also identify a
   critical challenge: tasks with minimal parameter changes often experience substantial performance degradation after
   fusing, compared to the original fine-tuned models.
   Based on this observation, we propose Scaling-Masks, which successfully preserves 100% of the original fine-tuned
   performance across all tasks in our experiments. In practical deployments, developers can leverage our fusion method
   to substantially reduce model parameters while incurring almost no loss in performance.

## Other Concerns:

- **Result explanation:**  
  Tables 8 and 9 present the performance of the fine-tuned models and the performance of the models obtained by applying
  different fusion methods to models fine-tuned on different PLs or tasks. The values in parentheses represent the
  normalized accuracy (please see response to Reviewer A Q2 for the detailed definition).

[//]: # (We appreciate the reviewer’s concerns and would like to clarify the primary research objective of our work. Our goal is to investigate whether it is possible to fuse fine-tuned checkpoints for software engineering &#40;SE&#41; tasks across different programming languages while preserving the original fine-tuned performance. Unlike methods that propose new approaches to improve the performance of large models on specific tasks, our work focuses on efficiently merging checkpoints without compromising the performance achieved during fine-tuning.)

[//]: # ()

[//]: # (A potential application of our proposed fusion method is its integration with checkpoints fine-tuned using existing SOTA methods. This combination could significantly reduce GPU memory consumption during deployment. By merging multiple fine-tuned checkpoints into a single model, our method allows for the deployment of several task-specific models without the need to load all individual checkpoints into memory, which would otherwise be prohibitively expensive.)

[//]: # ()

[//]: # (In addition to investigating the performance of fusing checkpoints from the same task across different programming languages, our research also explores more complex scenarios. We examine multi-task fusing and multi-programming language multi-task fusing, thereby offering a comprehensive evaluation of existing fusion techniques. Through these experiments, we identify the limitations of current methods and propose effective improvements to overcome these challenges.)

### Reference

[1] Huang K, Zhang J, Bao X, et al. Comprehensive Fine-Tuning Large Language Models of Code for Automated Program
Repair[J]. IEEE Transactions on Software Engineering, 2025.

[2] Wang S, Geng M, Lin B, et al. Natural language to code: How far are we?[C]//Proceedings of the 31st ACM joint
European software engineering conference and symposium on the foundations of software engineering. 2023: 375-387.

[3] Jiang X, Wu L, Sun S, et al. Investigating large language models for code vulnerability detection: An experimental
study[J]. arXiv preprint arXiv:2412.18260, 2024.

[4] Chen Y, Liu M, Ou G, et al. Are Decoder-Only Large Language Models the Silver Bullet for Code Search?[J]. arXiv
preprint arXiv:2410.22240, 2024.

[5] Afrin S, Call J, Nguyen K N, et al. Resource-Efficient & Effective Code Summarization[C]//2025 IEEE/ACM Second
International Conference on AI Foundation Models and Software Engineering (Forge). IEEE, 2025: 224-235.