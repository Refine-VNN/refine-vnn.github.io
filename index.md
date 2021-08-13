**Summary:** We present extensive empircal results to positive supprt our approach as the reviewers requested:

- [Bound tightness](#bound-tightness-comparisons-against-lp-based-refinements) and [time](#running-time-comparisons-against-lp-based-refinements) comparisons against LP-based refinements: we can achieve a very small gap to LP based refinement with 1000x less runtime
- [Distribution of Lower bound](#lower-bound-distributions): our bounds are tighter overall compared to baselines
- [Cactus plots](#cactus-plots): we can verify noticeably more datapoints than baselines
- [Updated results with improved implementation](#updated-results-with-improved-implementation): our new implementation has improved efficiency, which greatly improve our verified accuracy and avg. lower bounds.
- [marabou-cifar10 benchmark](#results-on-the-marabou-cifar10-benchmark-in-vnn-comp-2021) in VNN-COMP 2021: we can verify more examples than the winner of VNN-COMP 2021 on a **pre-existing and very hard** benchmark, `marabou-cifar10`.

Please find detailed descriptions for the new results in the main text of the rebuttal.

### [Bound tightness comparisons against LP-based refinements](#bound-tightness-comparisons-against-lp-based-refinements)

In Figure R1, we compare the tightness of our refined lower bounds against the tightest possible LP verifier **with** intermediate bound refinements (denoted as LP with refinements), as well as the state-of-the-art verifier β-CROWN. The tightest LP with refinements involves solving two LPs for each interemdiate layer neuron with a split constraint, and thousands of LPs are required for obtaining the refined bound under a single split constraints.

Table R1 shows that, to achieve a **very small gap** (1e-3 to 1e-4) to the tighest bound by LP with refinements, our approach **only requires 6 or 64 seconds** (50 or 500 iterations) with 1 CPU and 1 GPU, while the tightest LP refinement requires 72 CPU hours for refining a single split on MNIST-FC6, and 315 CPU hours on MNIST-FC7. The entire experiment took us around **50,000 CPU hours** to finish.

Note that β-CROWN theoretically converges to the LP verifier **without** intermediate bounds refinement (denoted as "LP w/o refinement", which solves a single LP for the final objective without refining intermediate layer bounds), while our new approach converges to the tightest LP verifier with intermediate bounds refinement.

![Figure R1](/figures/figR1.PNG)

### [Running time comparisons against LP-based refinements](#running-time-comparisons-against-lp-based-refinements)

See [Bound tightness comparisons](#bound-tightness-comparisons-against-lp-based-refinements) above for detailed experiments desciptions. Our method is over 1000 times fater than LP, while not too much slower compared to SOTA bound propagation based methods such as β-CROWN.

![Table R1](/figures/tabR1.PNG)


### [Lower bound distributions](#lower-bound-distributions)

In Figure R2, we plot the distribution of lower bounds obtained by different methods on MNIST-FC6, MNIST-FC7,and CIFAR-Conv models (this figure will **replace Figure 1** in our submission). A larger lower bound indicates better verification performance. The figures show that we can achieve **tighter bounds** (larger bounds) than all baseline methods.

![Figure R2](/figures/figR2.PNG)

### [Cactus plots](#cactus-plots)

In Figure R3, we present the cactus plots comparing against SOTA verifiers, including a newly released tool OVAL-BaB which contains a improved implementation of A.Set and BDD+ BaBSR.  Although many existing approaches can verify some easy instances quickly, their performance saturates with runtime and cannot solve hard instances regardless of runtime. **We verify more examples** compared to SOTA baselines.

![Figure R3](/figures/figR3.PNG)

### [Updated results with improved implementation](#updated-results-with-improved-implementation)

Due to the implementation complexity, our original implementation was rather slow compared to other SOTA verifiers. Our code has been significantly improved after our submission, leading to **much better results compared to the ones presented in the original submission**. 

Here in Table R2 we present the updated improvements on bound tightness and verified accuracy (this table **replaces Table 1** in our submission). Noticeably, our verified accuracy is improved from 61.5% to **72.0%**, and from 40.5% to **54.5%** on MNIST-FC6 and MNIST-FC7 models, respectively. The CIFAR-Conv network is very hard - although we could not improve its verified accuracy, the average lower bound improved from -17.62 to **-15.05**.

![Table R2](/figures/tabR2.PNG)

### [Results on the marabou-cifar10 benchmark in VNN-COMP 2021](#results-on-the-marabou-cifar10-benchmark-in-vnn-comp-2021)

Besides hard verification problems included in our paper, here we present the comparisons against SOTA complete verifiers on the `marabou-cifar10` benchmark in **VNN-COMP 2021**.This benchmark includes 72 instances; 52 out of the 72 instances can be attacked via PGD (unsafe), and the remaining 20 instances are very hard to verify - the network has many unstable neurons and **almost all tools fail to verify any of them**. Only **1** instance can be verified safe by the *winning* verifier of VNN-COMP 2021. In contrast, when our bound refinement is engaged, we can verify **3 out of 20** instances. We present detailed results in the table below:

| | Ours | alpha-beta-CROWN | VeriNet | Oval | ERAN |
|:-: |:----:|:------------------:|:--------:|:------------:|:-------------:|
| | (this work) | VNN COMP 1st place | 2nd | Tied for 3rd | Tied for 3rd |
|#verified/total | 3/20 | 1/20 | 0/20 | 0/20 | 0/20 |

