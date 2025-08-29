# Uninformed Search

## 1. Agent types

- **Reflex Agents**
    - 只根据当前感知（percept）选动作
    - 不考虑未来后果
    - Example: Pacman 看到食物直接冲过去（可能掉悬崖）
        
- **Planning Agents**
    - 会问 “what if”
    - 用世界模型推演动作的未来后果
    - 必须设定目标（goal test）
## 2. Search Problems

- 组成部分：
    - **State space**: 所有可能状态
    - **Successor function**: 给定状态的后继（包含动作和代价）
    - **Start state**
    - **Goal test**
- **Solution**: 从初始状态到目标状态的一系列动作

## 3. State Space vs Search Tree

- **State space graph**
    - 节点 = 抽象的世界状态
    - 边 = 动作结果
    - 每个状态只出现一次

- **Search tree**
    - 节点 = 一个“计划”（从根走到这里的路径）
    - 可能包含重复状态

- **Fringe**: 边界，保存还没展开的节点

## 4. Uninformed Search Algorithms

### 1. Depth-First Search (DFS)
- 策略：优先扩展最深的节点
- 时间复杂度：O(b^m)
- 空间复杂度：O(bm)
- Complete? 不是（可能卡死在无限分支）
- Optimal? 不是

### 2. Breadth-First Search (BFS)
- 策略：优先扩展最浅的节点
- 时间复杂度：O(b^s)，s=最浅解的深度
- 空间复杂度：O(b^s)
- Complete? 是（如果解有限）
- Optimal? 只在 **所有步长代价 = 1** 时

### 3. Iterative Deepening Search (IDS)
- 重复做深度限制 DFS，逐步加深
- 时间复杂度：O(b^s)，比 BFS 多一点点重复
- 空间复杂度：O(b·s)，像 DFS 一样低
- Complete? 是
- Optimal? 是（步长代价 = 1 时）

### 4. Uniform Cost Search (UCS)
- 策略：每次扩展 **路径代价最小** 的节点
- 时间复杂度：O(b^(C*/ε))，C* = 最优解代价
- 空间复杂度：同上
- Complete? 是（假设边代价 ≥ ε > 0）
- Optimal? 是

## 5. 关键对比

| 算法  | 完备性 | 最优性          | 时间复杂度       | 空间复杂度       |
| --- | --- | ------------ | ----------- | ----------- |
| DFS | 否   | 否            | O(b^m)      | O(bm)       |
| BFS | 是   | 是（unit cost） | O(b^s)      | O(b^s)      |
| IDS | 是   | 是（unit cost） | O(b^s)      | O(bs)       |
| UCS | 是   | 是            | O(b^(C*/ε)) | O(b^(C*/ε)) |
