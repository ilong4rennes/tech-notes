## React 组件生命周期

React 函数组件没有传统的生命周期方法，但可以通过 useEffect 来模拟生命周期行为。  
当依赖数组为空时，effect 只在组件挂载后执行一次。  
当依赖数组包含变量时，effect 会在这些变量更新时执行。  
如果在 effect 中返回一个函数，这个函数会在组件卸载时执行，用于清理副作用，比如定时器或事件监听。

## React 父子组件生命周期调用顺序

React 在函数组件中通过 useEffect 来模拟生命周期。
在组件挂载和更新时，渲染阶段会按照父组件到子组件的顺序执行函数体。
但在副作用执行阶段，useEffect 会按照子组件到父组件的顺序执行。
对于 cleanup 函数，无论是更新还是卸载阶段，也都是先执行子组件的 cleanup，再执行父组件的 cleanup。
**render 父先，effect 子先，cleanup 子先**

## React 组件通讯方式

React 组件通信最常见的方式包括：父组件通过 props 向子组件传递数据，子组件通过调用父组件传入的回调函数把数据传回父组件。对于跨层级组件通信，可以使用 Context。对于需要父组件直接调用子组件方法的场景，可以使用 ref、forwardRef 和 useImperativeHandle。对于更复杂的全局状态共享，通常会使用 Redux Toolkit、Zustand 或 MobX 等状态管理方案。

- 父子关系简单：用 `props`
- 子要告诉父：用“父传函数，子调用函数”
- 跨很多层：用 `Context`
- 父要直接调子的方法：用 `ref`
- 全局很多地方都要共享，逻辑还复杂：用状态管理库

## state 和 props 有什么区别？

props 是组件之间传递数据的方式，由父组件传递给子组件，是只读的，子组件不能直接修改。state 是组件内部的状态，由组件自己管理，可以通过 setState 或 useState 来更新。当 props 或 state 发生变化时，组件都会重新渲染。

## React 有哪些内置 Hooks ？

React 常用的内置 Hooks 可以分为几类：状态管理（useState、useReducer），副作用（useEffect、useLayoutEffect），上下文（useContext），引用（useRef），性能优化（useMemo、useCallback），以及一些高级 Hooks 如 useTransition、useDeferredValue、useId 等。

## 为何 dev 模式下 useEffect 执行两次？

在开发环境下，如果开启严格模式，React 会在实际运行 setup 之前额外运行一次 setup 和 cleanup。

这是一个压力测试，用于验证 Effect 的逻辑是否正确实现。如果出现可见问题，则 cleanup 函数缺少某些逻辑。cleanup 函数应该停止或撤消 setup 函数所做的任何操作。一般来说，用户不应该能够区分 setup 被调用一次（如在生产环境中）和调用 setup → cleanup → setup 序列（如在开发环境中）。

借助严格模式的目标是帮助开发者提前发现以下问题：

1. 不纯的渲染逻辑：例如，依赖外部状态或直接修改 DOM。
2. 未正确清理的副作用：例如，未在 useEffect 的清理函数中取消订阅或清除定时器。
3. 不稳定的组件行为：例如，组件在多次挂载和卸载时表现不一致。

通过强制组件挂载和卸载两次，React 可以更好地暴露这些问题。

## React 闭包陷阱

闭包陷阱是指在 useEffect 或事件回调中，函数捕获了旧的 state 或 props 值，导致使用的是过期数据。这通常发生在依赖数组为空时。解决方法包括将依赖加入依赖数组、使用 useRef 保存最新值，或者使用函数式更新来避免依赖旧值。

## React state 不可变数据

React 中 state 应该被当作不可变数据来处理，不能直接修改原值，而是需要创建一个新的值并通过 setState 或 useState 更新。这样 React 才能正确检测变化并触发重新渲染。

使用不可变数据可以带来如下好处：

1. **性能优化**
React 使用浅比较（shallow comparison）来检测状态是否发生变化。如果状态是不可变的，React 只需要比较引用（即内存地址）是否变化，而不需要深度遍历整个对象或数组。

2. **可预测性**
- 不可变数据使得状态的变化更加可预测和可追踪。
- 每次状态更新都会生成一个新的对象或数组，这样可以更容易地调试和追踪状态的变化历史。

3. **避免副作用**
- 直接修改状态可能会导致意外的副作用，尤其是在异步操作或复杂组件中。
- 不可变数据确保了状态的更新是纯函数式的，避免了副作用。

## React state 异步更新

React 的 state 更新不是立即生效的，而是会被批量处理，在当前事件执行完之后统一更新，这样可以减少重复渲染，提高性能。比如，setState 后立刻打印，还是旧值。state 更新不是立即生效，而是批量更新。

## React state 的“合并”特性

