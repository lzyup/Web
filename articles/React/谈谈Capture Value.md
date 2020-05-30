### 什么是Capture Value
每次Render的内容都会形成一个快照并保留下来，因此当状态变更而Renderer时，就形成了N个Render状态，而每个Render状态都拥有自己固定不变的Props与State

### 每一次渲染都有它自己的Props And State

当setCount(举例)的时候，React都会带着一个不同的`count`值再次调用组件。然后，React会更新DOM以保持和渲染输出一致。

#### 每一次渲染都有自己的事件处理
```
const App = () => {
  const [temp, setTemp] = React.useState(5);

  const log = () => {
    setTimeout(() => {
      console.log("3 秒前 temp = 5，现在 temp =", temp);
    }, 3000);
  };

  return (
    <div
      onClick={() => {
        log();
        setTemp(3);
        // 3 秒前 temp = 5，现在 temp = 5
      }}
    >
      xyz
    </div>
  );
};
```
在`log`函数执行的那个Render过程中，`temp`的值可以看作常量`5`,**执行setTemp(3)时会交由一个全新的Render渲染，所以不会执行log函数**。而3秒后执行出来的内容是由temp为5的那个render发出的，所以结果是5


#### 如何绕过Capture Value
利用`useRef`就可以绕过Capture Value的特性。**可以认为ref在所有的Render过程中保持着唯一引用，因此所有对ref的赋值或取值，拿到的都只有一个状态**，而不会在每个Render间存在隔离。
```
fucntion Example(){
    const [count ,setCount] = useState(0);
    const lastCount = useRef(count);
    
   useEffect(() => {
    // Set the mutable latest value
    latestCount.current = count;
    setTimeout(() => {
      // Read the mutable latest value
      console.log(`You clicked ${latestCount.current} times`);
    }, 3000);
  });
}
```
`ref`是Mutable,而`state`是Immutable 
    