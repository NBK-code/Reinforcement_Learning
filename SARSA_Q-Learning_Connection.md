# When Does SARSA Become Equivalent to Q-Learning?

At first glance, SARSA and Q-learning appear to be different reinforcement learning algorithms. However, under a purely greedy policy (i.e., no exploration), the two algorithms become mathematically identical.

This note makes that statement explicit and formal.

---

# SARSA Update Rule

SARSA updates the Q-value using the action that is actually selected by the current policy.

The update rule is:

$$
Q(s,a)
\leftarrow
Q(s,a)
+
\alpha
\left[
r + \gamma Q(s',a') - Q(s,a)
\right]
$$

where:

- $s$ = current state
- $a$ = current action
- $r$ = reward
- $s'$ = next state
- $a'$ = next action selected by the current policy

The key feature is that SARSA uses the value of the *actual next action taken*.

---

# Q-Learning Update Rule

Q-learning updates the Q-value using the maximum possible next-state action value.

The update rule is:

$$
Q(s,a)
\leftarrow
Q(s,a)
+
\alpha
\left[
r + \gamma \max_{a'} Q(s',a') - Q(s,a)
\right]
$$

The key feature is that Q-learning uses the *best possible next action*, regardless of which action was actually taken.

---

# Greedy Policy Assumption

Suppose the policy is purely greedy:

$$
a' = \arg\max_{a} Q(s',a)
$$

That is, exploration is disabled:

$$
\epsilon = 0
$$

in epsilon-greedy action selection.

Then the selected action $a'$ always satisfies:

$$
Q(s',a')=Q(s', \arg\max_a Q(s',a))
=\max_a Q(s',a)
$$

---

# Substituting into the SARSA Update

The SARSA target becomes:

$$
r + \gamma Q(s',a')
$$

Using the greedy-policy identity:

$$
Q(s',a') = \max_a Q(s',a)
$$

we obtain:

$$
r + \gamma \max_a Q(s',a)
$$

Therefore the SARSA update becomes:

$$
Q(s,a)
\leftarrow
Q(s,a)
+
\alpha
\left[
r + \gamma \max_a Q(s',a) - Q(s,a)
\right]
$$

which is exactly the Q-learning update rule.

---

# Conclusion

Under a purely greedy policy ($\epsilon = 0$):

$$
\boxed{
\text{SARSA} \equiv \text{Q-learning}
}
$$

because the action actually selected by the policy is always the maximizing action.

The difference between SARSA and Q-learning only appears when exploration is present.

---

# Interpretation

## SARSA

SARSA learns the value of the policy actually being followed.

With epsilon-greedy exploration, the update incorporates the consequences of exploratory actions.

Thus SARSA is an **on-policy** algorithm.

---

## Q-Learning

Q-learning learns the value of the optimal greedy policy regardless of the actions actually taken during exploration.

Thus Q-learning is an **off-policy** algorithm.

---

# The Essential Difference

The distinction appears only when:

$$
a' \neq \arg\max_a Q(s',a)
$$

which occurs during exploratory action selection.

In that case:

- SARSA uses the value of the exploratory action actually taken.
- Q-learning uses the value of the greedy optimal action.

Without exploration, the two targets coincide exactly.
