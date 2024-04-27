总结 React 中最常用和重要的核心 Hook 及其作用：

# 1. useState
用于在函数组件中创建状态变量。它返回一个状态变量和一个更新状态的函数。
典型用法：管理组件内部的状态。

```js
import React, { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};
```
# 2. useEffect
用于处理副作用，例如数据获取、事件监听、DOM 操作等。
可以通过依赖数组控制 Hook 的运行时机。
```js
import React, { useEffect } from 'react';

const MyComponent = () => {
  useEffect(() => {
    console.log('Component mounted');

    return () => {
      console.log('Component unmounted');
    };
  }, []); // 依赖数组为空，表示只在组件挂载和卸载时运行
};
```

# 3. useContext
允许函数组件使用 React Context，避免通过层层组件传递 props。
典型用法：在全局范围内共享数据。
```js
import React, { useContext } from 'react';

// 创建一个 Context
const MyContext = React.createContext();

const ChildComponent = () => {
  const contextValue = useContext(MyContext); // 获取 Context 的值
  return <div>Context value: {contextValue}</div>;
};

const ParentComponent = () => {
  return (
    <MyContext.Provider value="Hello, Context!">
      <ChildComponent />
    </MyContext.Provider>
  );
};
```

# 4. useReducer
用于在函数组件中实现 Redux 风格的状态管理。
提供了一个更复杂的状态逻辑，适合处理复杂状态的组件。
```js
import React, { useReducer } from 'react';

// 定义 reducer
const reducer = (state, action) => {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
};

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
};
```

# 5. useCallback
返回一个记忆化的回调函数，避免不必要的重新创建。
适用于在依赖变化时重新创建回调的场景。

```js
import React, { useState, useCallback } from 'react';

const MyComponent = () => {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount(count + 1);
  }, [count]); // 仅在 count 变化时重新创建回调

  return (
    <div>
      <button onClick={increment}>Increment</button>
    </div>
  );
};
```

# 6. useMemo
返回一个记忆化的值，避免昂贵计算的重复执行。
适用于需要缓存计算结果的场景。

```js
import React, { useMemo } from 'react';

const MyComponent = ({ data }) => {
  const computedValue = useMemo(() => {
    return data.reduce((acc, val) => acc + val, 0);
  }, [data]); // 仅在 data 变化时重新计算

  return <div>Computed value: {computedValue}</div>;
};
```

# 7. useRef
返回一个可变的引用，能够在组件生命周期内保持不变。
适用于获取 DOM 元素的引用或在组件间共享非状态变量。
```js 
import React, { useRef } from 'react';

const MyComponent = () => {
  const inputRef = useRef(null); // 创建引用

  const focusInput = () => {
    if (inputRef.current) {
      inputRef.current.focus(); // 使用引用操作 DOM 元素
    }
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
};
```

# 8. useLayoutEffect
类似于 useEffect，但在浏览器绘制之前执行，通常用于需要同步更新 DOM 的场景。
小心使用，因为它可能导致性能问题。

```js
import React, { useLayoutEffect } from 'react';

const MyComponent = () => {
  useLayoutEffect(() => {
    console.log('useLayoutEffect called');
  }, []);

  return <div>My Component</div>;
};
```


这些是 React 中最重要的核心 Hook。它们提供了丰富的功能，帮助开发者在函数组件中实现状态管理、副作用处理、复杂逻辑、优化性能等需求。
通过灵活使用这些 Hook，开发者可以构建更加简洁、模块化和高效的 React 组件。



-------------------------------------------------------------------------------------------------

useEffect 是 React 中的一个核心 Hook，主要用于在函数组件中处理副作用（side effects）。副作用是指组件在渲染过程中产生的额外行为，这些行为通常不直接影响组件的输出，但可能影响组件的生命周期、状态、或者与外部系统的交互。

useEffect 的用法
useEffect Hook 允许开发者在组件的生命周期中指定副作用的行为。它的基本用法是：

```js
import React, { useEffect } from 'react';

const MyComponent = () => {
  useEffect(() => {
    console.log('Component mounted');

    return () => {
      console.log('Component unmounted');
    };
  }, []); // 依赖数组为空，表示只在组件挂载和卸载时运行

  return <div>My Component</div>;
};

export default MyComponent;
```

主要功能
组件挂载和卸载：当组件首次渲染时，useEffect 内的代码会运行。这相当于组件挂载（mount）。当组件从页面中移除时，useEffect 内的返回函数（清理函数）会运行，这相当于组件卸载（unmount）。
依赖变化：useEffect 可以接收一个依赖数组。数组中的元素是状态或道具（props），当这些依赖发生变化时，useEffect 会重新执行。如果依赖数组为空，useEffect 只在组件挂载和卸载时运行。
```js
import React, { useState, useEffect } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log('Effect ran because count changed');

    return () => {
      console.log('Cleanup when count changes');
    };
  }, [count]); // 当 count 变化时，effect 和 cleanup 会运行

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};

export default Counter;
```

常见用例
- 数据获取：在组件挂载时发起网络请求，并在组件卸载时取消请求。
- 事件监听：在组件挂载时添加事件监听器，并在组件卸载时移除它。
- 订阅和取消订阅：在组件挂载时订阅外部数据源，并在组件卸载时取消订阅。
- DOM 操作：在组件挂载后进行 DOM 操作，确保组件正确渲染。
  
注意事项
避免无限循环：在 useEffect 中改变依赖项可能导致无限循环。确保在 useEffect 内部的状态更新不会触发额外的 useEffect 调用。
确保清理：如果 useEffect 内部创建了副作用（如事件监听器、计时器等），务必在清理函数中正确清理。


useEffect 是 React 函数组件中处理副作用的主要工具。它提供了灵活的方式来在组件生命周期中管理数据获取、事件监听、订阅等操作。通过依赖数组，可以控制 useEffect 何时运行以及何时清理，确保组件在不同状态下保持正确的行为。
