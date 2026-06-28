[← Back](../index.html)

# Trustworthy AI Lab Research

##  Conversational Prisoner Delimma Setup

Two players $i \in \{1,2\}$ have action space $a_i^{(t)} \in {C, D}$ standing for "Cooperate" (C) and "Defect" (D) in round $t$ for $t \in \{1,2,\dots\}$, and stage payoff chart is given as

$$
\begin{array}{c|cc}
 & C & D \\ \hline
C & (2,2) & (-1,3) \\
D & (3,-1) & (0,0)
\end{array}
$$

Termination condition follows similar setups in *Interactive Benchmark*. After each round t, the game continues with probability $\delta \in (0, 1)$, and ends with probability $1-\delta$. Each player knows $\delta$ from the start. So $\mathbb{P}(T=t)=(1-\delta)\delta^{t-1}$. The goal for both players are to maximize their own expected discounted cumulative payoff under $\delta$. At the messaging Phase of ronud $t$, each player sends a message $m_i^{(t)} \in \mathcal(M)$ where $\mathcal(M)$ is all human interpretable language space. Denote $H_t = ((m_1^{(1)}, m_2^{(1)}, a_1^{(1)}, a_2^{(1)}), \dots, (m_1^{(t-1)}, m_2^{(t-1)}, a_1^{(t-1)}, a_2^{(t-1)}))$ to be the historical information available to both players at the beginning of messaging phase. Denote $\pi_i = (\pi_i^{\text{msg}}, \pi_i^{\text{act}})$ to be the policy pairs mapping historical information to the next message and action. The flow of round t is shown as follows:

**Messaging Phase**
- Each player examine historical information.
- Each Player send a message to opponents: $m_i^{(t)} \sim \pi_i^{\text{msg}}(\cdot \mid H_t)$

**Decision Phase**
- Messges are available to each other simultaneously
- Each Player make a decision on action: $a_i^{(t)} \sim \pi_i^{\text{act}}(\cdot \mid H_t, m_1^{(t)}, m_2^{(t)})$
- Actions are made.

