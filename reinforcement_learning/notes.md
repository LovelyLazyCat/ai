# 强化学习

强化学习(Reinforcement Learning, RL)框架包含学习与其环境互动的智能体。

在每个时间步，智能体都收到环境

## Monte Carlo, 蒙特卡洛方法

当智能体位于一个它不了解的MDP中的时候，无法直接计算Action的状态值，也就无法直接通过动态规划来选择最优的策略。

蒙特卡洛方法使用概率来估计真值。让智能体在环境中产生episode，通过这些episode来动态地猜测，不断更新状态值。

通过这些猜测的状态值来评估现有的策略，随后使用Epsilon贪婪策略来优化策略。

总体来说，Monte Carlo方法分为两步，一步叫做MC预测，指的是试探环境，预测当前策略的效果；另外一步叫做MC控制，利用MC预测的结果来动态地改进策略。

### MC预测: 状态值

该问题旨在确定策略π对应的值函数v(π)或q(π)。

通过和环境互动评估策略π的方法分为两大类别:

- 在线策略方法使智能体与环境互动时遵守的策略π与要评估的相同。
- 离线策略方法使智能体与环境互动时遵守的策略b（注意b != π）与要评估的策略π不同。

状态s∈S在某个阶段中的每次出现称为s的一次经历。

有两种类型的MC预测方法:

- 首次经历将v(π,s)估算为仅在首次经历之后的平均回报（即忽略后续经历相关的回报）
- 所有经历将v(π,s)估算为s所有经历之后的回报

下面是首次经历的伪代码:

```python
# First-Visit MC Prediction (for State Values)
def MC_prediction_state(policy, num_episodes):

    # Initialize N(s)=0 for all s in S
    # Initialize returns_sum(s) = 0 for all s in S
    N = defaultdict(0)
    returns_sum = defaultdict(list)

    visited = []

    for i in range(1, num_episodes):

        # Generate an episode {s0, a0, r1, ..., st} using π
        episode = generate_episode(policy)
        states, rewards = zip(*episode)

        for state in episode:

            # If state is first visit
            if state in visited:
                visited.append(state)

                # update num and sum to calculate mean.
                N[state] += 1
                returns_sum[state] += sum(rewards[state + 1:])

    # Calculate mean value, as result.
    V = returns_sum / N

    return V
```

### MC预测: 动作值

动作值的预测更加具有意义，因为它能帮助我们选择更优的动作。

状态动作对s,a(s∈S, a∈A)在某个episode中首次出现称为s,a的一次经历。

有两种用于估算q(π)的MC预测方法:

- 首次经历MC将q(π, s, a)估算为仅在s, a首次经历后的平均回报（即忽略与后续经历相关的回报）
- 所有经历MC将q(π, s, a)估算为s, a所有经历后的平均回报

下面是估算q(π)的伪代码(和算状态值只有细微的差别):

```python
# First-Visit MC Prediction (for Action Values)
def MC_prediction_action(policy, num_episodes):

    # Initialize N(s)=0 for all s in S
    # Initialize returns_sum(s) = 0 for all s in S
    N = defaultdict(0)
    returns_sum = defaultdict(list)

    visited = []

    for i in range(1, num_episodes):

        # Generate an episode {s0, a0, r1, ..., st} using π
        episode = generate_episode(policy)
        states, actions, rewards = zip(*episode)

        for state in states:

            action = actions[state]
            # If (St, At) is a first visit
            if state, action not in visited:
                visited.append((state, action))
                N[state, action] += 1
                returns_sum[state, action] += sum(rewards[state + 1:])
    V = returns_sum / N

    return V
```

### 广义策略迭代

广义策略(GPI)迭代旨在解决控制问题的算法会通过与环境互动确定最优策略π。

GPI通过交替给进行策略评估和改进步骤来探索最优策略的广义方法。

### MC控制: 策略改进

- 若对于每个状态s∈S，它保证会选择满足a=argmax(Q(s, a))，也就是每次选择动作值最大的动作，那么这种策略相对于Q来说是贪婪策略（通常将选动作称之为贪婪动作）
- 贪婪策略不会选择次优的action，这对于策略的改进来说并不是好事（一些潜在的更好的策略可能就潜藏在次优的action中），所以我们也要给予次优的action一些被选择的机会。这就引入了epsilon贪婪策略。
  - 概率为1 - epsilon: 智能体选择贪婪动作
  - 概率为epsilon: 智能体均匀地选择一个动作

#### 探索与利用

所有强化学习智能体都面临探索-利用环境，即智能体必须在根据当前信息采取最优动作（利用）和需要获取信息以做出更好的判断（探索）之间找到平衡。

为了使MC控制收敛于最优策略，必须满足有限状态下的无限探索贪婪算法(GLIE)条件:
  - 所有状态动作对s, a(s∈S, a∈A)被经历无数次
  - 策略收敛于相对于动作值函数估值Q来说是贪婪策略的策略

使用满足GLIE的算法来进行MC控制的代码如下:

```python
# GLIE MC Control
def MC_control(num_episodes):
    
    # Initialize N(s)=0 for all s in S
    # Initialize returns_sum(s) = 0 for all s in S
    N = defaultdict(0)
    Q = defaultdict(0)

    visited = []

    for i in range(1, num_episodes):
        epsilon = 1.0 / i
        policy = epsilon_greedy_choose_policy(Q)

        # generate an episode {S0, A0, R1, ..., St} using policy π
        episode = generate_episode(policy)

        states, actions, rewards = zip(*episode)

        for state in states:
            action = actions[state]
            if state, action not in visited:
                N[state, action] += 1
                Q[state, action] += (1 / N[state, action]) *\
                (sum(rewards[state + 1:]) - Q[state, action])
            
    return policy
```

#### MC控制: 常量alpha

步长参数alpha必须满足0 &lt; alpha &lt;= 1。alpha越大，学习速度越快，但是如果alpha的值过大，可能导致MC控制无法收敛于π。

Constant-alpha CLIE Control的伪代码和普通的CLIE Control伪代码几乎一样，区别在于更新Q的时候使用:

```python
Q[state, action] += alpha * (sum(rewards[state + 1:]) - Q[state, action])
```