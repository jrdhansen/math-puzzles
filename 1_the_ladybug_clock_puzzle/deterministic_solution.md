# Problem: The Last Visitor on a Circular Random Walk

## Disclaimer
- I was familiar with random walks and knew that would be a part of this solution.
- However, I was rusty on other math useful for this situation (like Markov chains) and leaned on Gemini to help move me in the right direction. I wish that I could claim all of the work below as my own, but alas it is not.

&nbsp;
## Problem Statement
A ladybug starts at position 12 on a clock face. Every second, she moves to a neighboring number with probability $p = 1/2$. As numbers are visited, they are marked. The process continues until all numbers $\{1, 2, \dots, 12\}$ have been visited. What is the probability that the number **6** is the last number to be visited?

## Mathematical Formulation

### 1. Markov Chain Definition
Let $S = \{0, 1, \dots, 11\}$ be the set of states representing the clock positions (where $0 \equiv 12$). Let $(X_t)_{t \ge 0}$ be a simple symmetric random walk on a cycle graph $C_{12}$.
The transition probabilities are:
$$P(X_{t+1} = j \mid X_t = i) = \begin{cases} 1/2 & \text{if } j = i \pm 1 \pmod{12} \\ 0 & \text{otherwise} \end{cases}$$
With initial condition $X_0 = 0$.

### 2. Hitting Times
Define the first hitting time $T_i$ for any state $i \in S$ as:
$$T_i = \inf \{ t \ge 0 : X_t = i \}$$
We wish to calculate the probability $P(T_6 = \max_{j \in S} T_j)$.

### 3. The Neighbor Boundary Condition
For $6$ to be the last state visited, the ladybug must reach the set of its neighbors $\partial 6 = \{5, 7\}$ before it reaches $6$. Let $\tau$ be the first time the walk hits this boundary:
$$\tau = \min(T_5, T_7)$$
By the symmetry of the walk starting at $0$, we know:
$$P(X_\tau = 5) = P(X_\tau = 7) = 1/2$$



### 4. Reduction to Gambler's Ruin
Once the ladybug reaches a neighbor, say $X_\tau = 5$, the condition that $6$ is the last number visited requires that the walk hits $7$ (the other neighbor) before it hits $6$.

On a cycle of $n=12$ nodes, if the walk is at $5$, the state $6$ is $1$ step away in one direction, and the state $7$ is $n-1 = 11$ steps away in the other direction. We can treat this as a linear Gambler's Ruin problem on the interval $[0, 11]$ where:
* The current position is $x = 1$ (representing state $5$).
* The "fail" boundary is $0$ (representing state $6$).
* The "success" boundary is $11$ (representing state $7$).



For a symmetric random walk starting at $x$ on the interval $[0, N]$, the probability of hitting $N$ before $0$ is $x/N$. Substituting our values:
$$P(T_7 < T_6 \mid X_\tau = 5) = \frac{1}{11}$$

### 5. Symmetry and Total Probability
Applying the Law of Total Probability across both possible boundary entries:
$$P(\text{6 is last}) = P(T_7 < T_6 \mid X_\tau = 5)P(X_\tau = 5) + P(T_5 < T_6 \mid X_\tau = 7)P(X_\tau = 7)$$
$$P(\text{6 is last}) = \left( \frac{1}{11} \cdot \frac{1}{2} \right) + \left( \frac{1}{11} \cdot \frac{1}{2} \right)$$
$$P(\text{6 is last}) = \frac{1}{11}$$

## Conclusion
The probability that any specific number $k \neq X_0$ is the last to be visited on a cycle of $n$ nodes is $\frac{1}{n-1}$. For a clock ($n=12$), this yields:
$$P = \frac{1}{11} \approx 0.0909$$