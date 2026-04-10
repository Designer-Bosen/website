[← Back](../index.html)

# Trustworthy AI Lab Research
Work with **Professor Guang Cheng** and **PhD Candidate Hengzhi He**

## Project Description

**Motivation:** Recent work [Interactive Benchmark](https://arxiv.org/pdf/2603.04737) shows that language models 


## Paper Ideas

**Interactive Proofs** aims to converge on a truth through logical explanations or mathematical derivations. The model interacts with the judge's consistent and hidden ground truth, and navigate to the solution.

some models 

**Interactive Games** aims to maximize expected long-term payoff against uncertain adversaries. The model does not converge to a truth, but varies based on agents' historical response. 

some models 

The paper proposes Interactive Benchmark, a framework that measures a model's reasoning ability in a budgeted, multi-turn interaction process. 

---

## Object Formulation




## Notes

- We allow communication （will it break nash equilibrium - posible tie the game）
- goal: get higher score
- Communicate after round t and judge historical results

Different agent models:
- very unpredictable (irrational: like sudden change after consistent results)
- stochasitic
- optimal (maximizing score: could reach to Nash Equilibrium)
- Combination of above

After each competition, remove the history
pair-wise match


Stage 1: (chat allowed)
Pre-game: agent vs agent -> small scaled paired matches (search for opponents information and strategy)


Stage 2: (chat allowed)
acquire n by n unweighted A matrix, recording the match information
- rows winning against opp
- cols losing 
weighted matrix $W*bar{X}$, $W$ is the weight matrix
matrix w* is the ground truth giving the highest winning rate

w(A) -> w*(A*) (stable state)


winning condition: summation of rows


OASIS -> take 2 agents and check validity (chat)
opensiel -> prisoner dilemma (chat version -> based on the open source code of prisoner delimnma)


limited source
unlimited source
