# 设计哲学

# 组件

## 函数组件与类组件



## 通信方式

1. `props` & `callback`
    - 所有参数通过 `props` 传递，子组件通过父组件传递的 `callback` 通知父组件。
2. `context`
    - 顶级组件向后台组件跨层级传值。
3. `Event` 事件
4. `ref` 传递
5. 全局状态管理

## 组件强化

1. `props` & `callback`
    - 所有参数通过 `props` 传递，子组件通过父组件传递的 `callback` 通知父组件。
2. `context`
    - 顶级组件向后台组件跨层级传值。
3. `Event` 事件
4. `ref` 传递
5. 全局状态管理

# Hooks

## State Hook

1.   `useState`：声明一个**状态变量**。

     ```typescript
     const [state, setState] = useState(initialState);
     ```

     -   `Params`：

         -   `initialState`：初始化值，仅在初始渲染中使用，后续此参数会被**忽略**（而非不使用）。

             可以传递纯函数，且不应当接受任何参数。
     -   `Return`：
     -   `Notice`：

         -   `set` 使用 `Object.is() `对新值进行一次浅比较。

             如果不同，则进行更新；相同，则跳过更新。

         -   `React` 在所有事件处理函数运行并调用其 `set` 函数后才会更新屏幕。

             如果需要更早的更新屏幕，使用  [`flushSync`](https://zh-hans.react.dev/reference/react-dom/flushSync)。

         -   当次渲染中组件中获得的 `state` 永远只是当前渲染的一次快照。

             只有在下次渲染时 `state` 才会被更新。

         -   在渲染期间也可以调用 `set` 函数（不在事件处理函数中执行 `set`，而是在组件函数体中）。
             这会使 React 丢弃其输出（`set` 其后的值也会被丢弃，所以早点 `return` 会提升性能）并重新使用新值进行渲染。极少的情况会需要用到这种特性。（如果写在 `useEffect` 中则是一种更差的写法，组件会被更新两次。）

             而且函数体中的 `set` 必须位于条件语句中，否则将会进入死循环渲染。

             此外，只能更新当前渲染组件的状态，不能调用另一个组件的 `set` 函数。

2.   `useReducer`：声明一个 `Reducer`。

     ```typescript
     const [state, dispatch] = useReducer(reducer, initialArg, init?)
     ```

     -   `Params`：
     
         -   `reducer`：用于更新 `state` 的纯函数，参数为 `state` 和 `action`。
     
         -   `initialArg`：初始化 `state` 的任意值。
     
         -   `init`：可选值，如果存在则按 `init(initialArg)` 的结果作为初始值。
     
             一般用途是避免重复获得 `initialArg` 所带来的性能损耗。
     -   `Return`：
     
         - `state`：当前的 `state`。
     
         - `dispatch`：用于更新 state 并触发组件的重新渲染。
     
             此函数无返回值。
     
             -   `dispatch` 的参数名为 `action`。`action` 的组成一般为：
     
                 ```typescript
                 {
                 	type: someType,		// 标识动作类型
                 	paylad: somePayload	// 标识额外信息
                 }
                 ```
     -   `Notice`：注意事项基本与 `useState` 相同。

## Context Hook

1.   `useContext`：允许读取和订阅组件中的 `context`。

     ```typescript
     const value = useContext(SomeContext)
     ```

     -   `Params`：
     
         -   `SomeContext`：之前用 `createContext` 创建的 `context`。
     
             `context` 本身不包含信息，只是代表你可以提供或读取的信息类型。
     -   `Return`：
     
         - 距离调用组件上方最近的 `SomeContext.Provider` 的 `value` 的值。
         - 如果没有 `Provider`，则会读取创建该 `context` 传递给 `createContext` 的 `defaultValue`。
     -   `Notice`：
     
         -   Context 可互相嵌套，可起到累计信息，覆盖前值的作用。
         -   `createContext` 中传入的参数为默认值，不可变，如果 `useContext` 没有读取到有效内容则会使用这个值。

## Ref Hook

1.   `useRef`：可引用一个不需要渲染的值。

     ```typescript
     const ref = useRef(initialValue)
     ```

     -   `Params`：
     -   `Return`：
     -   `Notice`：

2.   `useImperativeHandle`：可自定义由 `ref` 暴露出来的句柄。

     ```typescript
     useImperativeHandle(ref, createHandle, dependencies?)
     ```

     -   `Params`：
     -   `Return`：
     -   `Notice`：

## Effect Hook

1.   `useEffect`：允许组件与外部系统同步。

     ```typescript
     useEffect(setup, dependencies?)
     ```

     -   `Params`：
     -   `Return`：
     -   `Notice`：

2.   `useLayoutEffect`：`useEffect` 的一个版本，在浏览器重新绘制屏幕之前触发。

     ```typescript
     useLayoutEffect(setup, dependencies?)
     ```

     -   `Params`：
     -   `Return`：
     -   `Notice`：

3.   `useInsertionEffect`：在布局副作用触发之前将元素插入到 `DOM` 中。

     ```typescript
     useInsertionEffect(setup, dependencies?)
     ```

     -   `Params`：
     -   `Return`：
     -   `Notice`：

## 性能 Hook

1.   `useMemo`：在每次重新渲染的时候能够缓存**计算结果**。

     ```typescript
     const cachedValue = useMemo(calculateValue, dependencies)
     ```

     -   `Params`：
     -   `Return`：
     -   `Notice`：

2.   `useCallback`：在每次重新渲染的时候能够缓存**函数**。

     ```typescript
     const cachedFn = useCallback(fn, dependencies)
     ```

     -   `Params`：

         -   `fn`：想要缓存的函数本身。
         -   `dependencies`：依赖项。
     -   `Return`：
     -   `Notice`：

         -   常和 `memo` 同时使用，保证传递的**参数相同**。

         -   总之，当函数作为**依赖项**时，需要用到 `useCallback`。

             >   必须作为依赖项，典型场景如编写自定义 Hooks 时需要返回函数时。
             >
             >   但是，当**没有必要作为依赖项**时，是完全可以不用 `useCallback` 的。
             >   比如在 `useEffect` 中可以直接将函数在其中声明，也不需要到外层声明再用 `useCallback` 包裹一遍。

         -   ~~~typescript
             // 在 React 内部的简化实现
             function useCallback(fn, dependencies) {
               return useMemo(() => fn, dependencies);
             }
             ```
             ~~~

             

3.   `useTransition`：在不阻塞 UI 的情况下来更新状态。

     ```typescript
     const [isPending, startTransition] = useTransition()
     ```

     -   `Params`：
     -   `Return`：
     -   `Notice`：

4.   `useDeferredValue`：可延迟更新 UI 的某些部分。

     ```typescript
     const deferredValue = useDeferredValue(value)
     ```

     -   `Params`：
     -   `Return`：
     -   `Notice`：

## 其他 Hook

1.   `useDebugValue`：在 React 开发工具中为自定义 Hook 添加标签。

     ```typescript
     useDebugValue(value, format?)
     ```

     -   `Params`：
     -   `Return`：
     -   `Notice`：

2.   `useId`：可以生成传递给无障碍属性的唯一 ID。

     ```typescript
     const id = useId()
     ```

     -   `Params`：
     -   `Return`：
     -   `Notice`：

3.   `useSyncExternalStore`：订阅外部 store。

     ```typescript
     const snapshot = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?)
     ```

     -   `Params`：
     -   `Return`：
     -   `Notice`：

# 架构