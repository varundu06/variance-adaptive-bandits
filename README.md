# UCB1's Variance Blind Spot

Meausring how much UCB1's blindness to variance causes it to overexplore suboptimal arms and how the standard fix recovers the loss, and where the improvement actually shows up.

A single self-contained Jupyter notebook that builds a bandit simulator and runs five classical baselines along with two variance adapted algorithms.

---

## Summary

UCB1's exploration mechanism is based on Hoeffding's inequality which does not take into account how noisy an arm's reward distribution is. This means that an arm that pays a coin flip and one that pays `0.5 +/- 0.001` look identical to it. The latter only needs a few pulls to determine its expected value and potentially rule it out, however UCB1 spends a lot more.

The standard fix is to estimate each arm's variance and plug it into an empirical Bernstein bound instead. This bound is comprised of a term that shrinks with variance and a term that only depends on the reward range. Audibert et al. state both and show that the second cannot be removed.

This notebook measures to what extent each term contributes to the confidence radius of the bound. The range term is the larger of the two for every variance low enough that variance adaptation would have been worth having, and the crossover between them recedes further as the variance falls.

Variance adaptation lowers the number of pulls that a suboptimal arm recieves from `1/gap^2` to `1/gap`. Since regret is gap times pulls, the gaps cancel out and regret for low variance arms becomes nearly independent of the gap.

---

## Notebook structure

| Part | Contents |
|---|---|
| 1 | Building the simulator: arm interface, reward distributions, instances and pseudo-regret. There is also a scalar and vectorised runner, an equivalence test between them, and five baseline algorithms. |
| 2 | Where UCB1's radius comes from, why it is blind to variance, how many samples that wastes, and a direct confirmation |
| 3 | UCB-V and empirical Bernstein UCB, plus the three experiments above |
| 4 | Findings, limitations, next steps |

Baselines implemented: Uniform, Explore-first, epsilon-greedy, Successive Elimination, UCB1.
Variance-adaptive: UCB-V (Audibert, Munos and Szepesvari 2009) and EB-UCB (Maurer and Pontil 2009).

## References

- A. Slivkins. *Introduction to Multi-Armed Bandits*. Foundations and Trends in Machine Learning,
  2019. arXiv:1904.07272.
- P. Auer, N. Cesa-Bianchi, P. Fischer. *Finite-time Analysis of the Multiarmed Bandit Problem*.
  Machine Learning 47, 2002.
- J.-Y. Audibert, R. Munos, C. Szepesvari. *Exploration-exploitation tradeoff using variance
  estimates in multi-armed bandits*. Theoretical Computer Science 410(19), 2009, 1876 to 1902.
- A. Maurer, M. Pontil. *Empirical Bernstein Bounds and Sample Variance Penalization*. COLT 2009.
