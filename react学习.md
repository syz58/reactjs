

React 学习笔记

//multiple state can be converted into one sate



#### Two way binding, 

1. User input—>update the state (to record the user input by updating the state)

2. Update the state —>Update user input (when you need to clear the user input after submitting the form) 

   //usually it is fullfill by adding value attribute



#### Transfering Date

//transfer data from parent to children—>use props **(transfer upward)**

//transfer date from children to parent—>create a function that uses state function —> gives props the pointer to own created function —>in child, props call that function—> in the parent, you are going to have a state, then when the function is called in the child component, the attribute is changed by the state function inside your own created function —> **(transfer downward)**

#### When using inner style 

use {{.....}}



#### Form submission and Transfer upward

```react
const submitHandler = (event) => {
    event.preventDefault();
    props.onLogin(emailState.value, enteredPassword);
  };
// This is for form submit, 
// preventDefault can be used in form submit to avoid refresh of a new page
// onLogin is a function from parent component, this is used for sending infomation upward

<form onSubmit={submitHandler} ={enter a pointer to submit form}></form>
//This onSubmit is for submission purpose when click submit button


```



```react
<label htmlFor=???></label>
<input id = ???></input>   
//lable and input usually stick together
```



#### Use id not index for the key 

```react
	/*
   经典面试题:
      1). react/vue中的key有什么作用？（key的内部原理是什么？）
      2). 为什么遍历列表时，key最好不要用index?
      
			1. 虚拟DOM中key的作用：
					1). 简单的说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用。

					2). 详细的说: 当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】, 
												随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：

									a. 旧虚拟DOM中找到了与新虚拟DOM相同的key：
												(1).若虚拟DOM中内容没变, 直接使用之前的真实DOM
												(2).若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM

									b. 旧虚拟DOM中未找到与新虚拟DOM相同的key
												根据数据创建新的真实DOM，随后渲染到到页面
									
			2. 用index作为key可能会引发的问题：
								1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作:
												会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。

								2. 如果结构中还包含输入类的DOM：
												会产生错误DOM更新 ==> 界面有问题。
												
								3. 注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，
									仅用于渲染列表用于展示，使用index作为key是没有问题的。
					
			3. 开发中如何选择key?:
								1.最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。
								2.如果确定只是简单的展示数据，用index也是可以的。
   */
	
	/* 
		慢动作回放----使用index索引值作为key

			初始数据：
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			初始的虚拟DOM：
					<li key=0>小张---18<input type="text"/></li>
					<li key=1>小李---19<input type="text"/></li>

			更新后的数据：
					{id:3,name:'小王',age:20},
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			更新数据后的虚拟DOM：
					<li key=0>小王---20<input type="text"/></li>
					<li key=1>小张---18<input type="text"/></li>
					<li key=2>小李---19<input type="text"/></li>

	-----------------------------------------------------------------

	慢动作回放----使用id唯一标识作为key

			初始数据：
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			初始的虚拟DOM：
					<li key=1>小张---18<input type="text"/></li>
					<li key=2>小李---19<input type="text"/></li>

			更新后的数据：
					{id:3,name:'小王',age:20},
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			更新数据后的虚拟DOM：
					<li key=3>小王---20<input type="text"/></li>
					<li key=1>小张---18<input type="text"/></li>
					<li key=2>小李---19<input type="text"/></li>


	 */
	class Person extends React.Component{

		state = {
			persons:[
				{id:1,name:'小张',age:18},
				{id:2,name:'小李',age:19},
			]
		}

		add = ()=>{
			const {persons} = this.state
			const p = {id:persons.length+1,name:'小王',age:20}
			this.setState({persons:[p,...persons]})
		}

		render(){
			return (
				<div>
					<h2>展示人员信息</h2>
					<button onClick={this.add}>添加一个小王</button>
					<h3>使用index（索引值）作为key</h3>
					<ul>
						{
							this.state.persons.map((personObj,index)=>{
								return <li key={index}>{personObj.name}---{personObj.age}<input type="text"/></li>
							})
						}
					</ul>
					<hr/>
					<hr/>
					<h3>使用id（数据的唯一标识）作为key</h3>
					<ul>
						{
							this.state.persons.map((personObj)=>{
								return <li key={personObj.id}>{personObj.name}---{personObj.age}<input type="text"/></li>
							})
						}
					</ul>
				</div>
			)
		}
	}

	ReactDOM.render(<Person/>,document.getElementById('test'))
```



#### refs 三种不同的调用形式

*1_字符串形式的ref

```react

//创建组件
		class Demo extends React.Component{
			//展示左侧输入框的数据
			showData = ()=>{
				const {input1} = this.refs
				alert(input1.value)
			}
			//展示右侧输入框的数据
			showData2 = ()=>{
				const {input2} = this.refs
				alert(input2.value)
			}
			render(){
				return(
					<div>
						<input ref="input1" type="text" placeholder="点击按钮提示数据"/>&nbsp;
						<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
						<input ref="input2" onBlur={this.showData2} type="text" placeholder="失去焦点提示数据"/>
					</div>
				)
			}
		}
		//渲染组件到页面
		ReactDOM.render(<Demo a="1" b="2"/>,document.getElementById('test'))
  
```

2. **回调形式的refs******(常用)****

```react
//创建组件
		class Demo extends React.Component{
			//展示左侧输入框的数据
			showData = ()=>{
				const {input1} = this
				alert(input1.value)
			}
			//展示右侧输入框的数据
			showData2 = ()=>{
				const {input2} = this
				alert(input2.value)
			}
			render(){
				return(
					<div>
						<input ref={c => this.input1 = c } type="text" placeholder="点击按钮提示数据"/>&nbsp;
						<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
						<input onBlur={this.showData2} ref={c => this.input2 = c } type="text" placeholder="失去焦点提示数据"/>&nbsp;
					</div>
				)
			}
		}
		//渲染组件到页面
```



3. _createRef******(常用)****

```react
//创建组件
		class Demo extends React.Component{
			/* 
				React.createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点,该容器是“专人专用”的
			 */
			myRef = React.createRef()
			myRef2 = React.createRef()
			//展示左侧输入框的数据
			showData = ()=>{
				alert(this.myRef.current.value);
			}
			//展示右侧输入框的数据
			showData2 = ()=>{
				alert(this.myRef2.current.value);
			}
			render(){
				return(
					<div>
						<input ref={this.myRef} type="text" placeholder="点击按钮提示数据"/>&nbsp;
						<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
						<input onBlur={this.showData2} ref={this.myRef2} type="text" placeholder="失去焦点提示数据"/>&nbsp;
					</div>
				)
			}
		}
		//渲染组件到页面
		ReactDOM.render(<Demo a="1" b="2"/>,document.getElementById('test'))
```

#### useRef()

```react
(1). Ref Hook可以在函数组件中存储/查找组件内的标签或任意其它数据
(2). 语法: const refContainer = useRef()
(3). 作用:保存标签对象,功能与React.createRef()一样

//创建组件
		function Demo(){
			/* 
				React.createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点,该容器是“专人专用”的
			 */
			const myRef = React.createRef()
			const myRef2 = React.createRef()
			//展示左侧输入框的数据
			showData = ()=>{
				alert(myRef.current.value);
			}
			//展示右侧输入框的数据
			showData2 = ()=>{
				alert(myRef2.current.value);
			}
      
      return(
        <div>
          <input ref={myRef} type="text" placeholder="点击按钮提示数据"/>&nbsp;
          <button onClick={showData}>点我提示左侧的数据</button>&nbsp;
          <input onBlur={showData2} ref={myRef2} type="text" placeholder="失去焦点提示数据"/>&nbsp;
        </div>
      )
}
```



#### useReducer()

  const [emailState, dispatchEmail] = useReducer(emailReducer, {value: '', isValid: null,});

-->**emailReducer** a function that will be automatically called when an action is dispatched, the function then edit the emailState

-->action is controlled by and dispatched by **dispatchEmail**

```react
//this is NOT react component
const emailReducer = (state, action) => {
  if (action.type === 'USER_INPUT') {
    return { value: action.val, isValid: action.val.includes('@') };
  }
  if (action.type === 'INPUT_BLUR') {
    return { value: state.value, isValid: state.value.includes('@') };
  }
  return { value: '', isValid: false };
};

//this is the react component
const Login = (props) => {
  const [enteredPassword, setEnteredPassword] = useState('');
  const [passwordIsValid, setPasswordIsValid] = useState();
  const [formIsValid, setFormIsValid] = useState(false);
  
  const [emailState, dispatchEmail] = useReducer(emailReducer, {value: '', isValid: null,});
  
  const emailChangeHandler = (event) => {
    dispatchEmail({type: 'USER_INPUT', val: event.target.value});
    setFormIsValid(
      event.target.value.includes('@') && enteredPassword.trim().length > 6
    );
  };
  
  const validateEmailHandler = () => {
    dispatchEmail({type: 'INPUT_BLUR'});
  };

  const submitHandler = (event) => {
    event.preventDefault();
    props.onLogin(emailState.value, enteredPassword);
  };

  return (
    <Card className={classes.login}>
      <form onSubmit={submitHandler}>
        <div
          className={`${classes.control} ${
            emailState.isValid === false ? classes.invalid : ''
          }`}
        >
          <label htmlFor="email">E-Mail</label>
          <input
            type="email"
            id="email"
            value={emailState.value}
            onChange={emailChangeHandler}
            onBlur={validateEmailHandler}
          />
        </div>
        <div className={classes.actions}>
          <Button type="submit" className={classes.btn} disabled={!formIsValid}>
            Login
          </Button>
        </div>
      </form>
    </Card>
  );
};

export default Login;
```

#### useEffect()

```react
useEffect(()=>{can be a var pointing to a function that change some dependencies states 
          return ()=>{can be a function that is like willumount}
              }, [dependencies, this is a array]);

useEffect(() => {
    console.log('EFFECT RUNNING');

    return () => {
      console.log('EFFECT CLEANUP');
    };
  }, []);

   useEffect(() => {
     const identifier = setTimeout(() => {
       console.log('Checking form validity!');
       setFormIsValid(
         enteredEmail.includes('@') && enteredPassword.trim().length > 6
       );
     }, 500);
     

     return () => {
       console.log('CLEANUP');
       clearTimeout(identifier);
     };
   }, [enteredEmail, enteredPassword]);


//1
useEffect(() => {}) 
//everything is dependency // execute every time

//2
useEffect(() => {},[]) 
//no dependency //execute only when first time mount
//can add setTime, fetch, axios. 

//3
useEffect(() => {},[name, grade]) 
//dependency on name, grade
```

#### useContext()

```react
//create Context
import { useState, createContext } from "react";


function UserContext = React.createContext({
  user: "Jesse Hall"
  //make sure also create a dummy state here for IDE suggestions
});



//create the Provider
function Component1() {
  const [user, setUser] = useState("Jesse Hall");

  return (
    <UserContext.Provider value={
        {userctx: user
        ,//...add more state here...}
      }>
      <h1>{`Hello ${user}!`}</h1>
      <Component2 user={user} />
    </UserContext.Provider>
  );
}

      
      
//no need for 2,3,4 to take props that transfer downward to 5 
function Component2() {<Component3 />}
function Component3() {<Component4 />}
function Component4() {<Component5 />}

      
      
      
// use Context Hook
function Component5() {
  const ctx = useContext(UserContext);

  return (
    <>
      <h1>Component 5</h1>
      <h2>{`Hello ${ctx.userctx} again!`}</h2> 
    //this ctx.user is from Provider not from UserContext, so you can add more state value to the UserContext.Provider value part. (not add to the UserContext part)
    </>
  );
}
      
```

#### useMemo()

```react

export default React.memo(Button);
//when using React.memo(...) to export, it compares the current virtual DOM with previous one, if the props, state... changes, the pages will make a new DOM based
//this comparison can only be done with primitive values, such as string, boolean
//for a function that is not primitive type, it needs useCallback to see if changes or not

```

#### useCallback()

```react
function App() {
  const [showParagraph, setShowParagraph] = useState(false);
  const [allowToggle, setAllowToggle] = useState(false);

  console.log('APP RUNNING');

  const toggleParagraphHandler = useCallback(() => {
    if (allowToggle) {
      setShowParagraph((prevShowParagraph) => !prevShowParagraph);
    }
  }, [allowToggle]);

  const allowToggleHandler = () => {
    setAllowToggle( );
  };

  return (
    <div className="app">
      <h1>Hi there!</h1>
      <DemoOutput show={showParagraph} />
      <Button onClick={allowToggleHandler}>Allow Toggling</Button>
      <Button onClick={toggleParagraphHandler}>Toggle Paragraph!</Button>
    </div>
  );
}
//useCallback()-->store a function unless certain dependency changes
//toggleParagraphHandler the button will not be re-rendered unless the dependency allowToggle changes. 
```

#### useMemo()

```react
//memorize the data so that when re-render, that data will not change
function App() {
  const [listTitle, setListTitle] = useState('My List');

  const changeTitleHandler = useCallback(() => {
    setListTitle('New Title');
  }, []);

  const listItems = useMemo(() => [5, 3, 1, 10, 9], []);

  return (
    <div className="app">
      <DemoList title={listTitle} items={listItems} />
      <Button onClick={changeTitleHandler}>Change List Title</Button>
    </div>
  );
}
```

#### **Uuid **

**nanoid  *生成unique id

** keyCode—>judge if the user press the enter (13)

{newItem, ...list}



#### Fragment

```react
	<Fragment><Fragment>
	<></>


```

作用

> 可以不用必须有一个真实的DOM根标签了

<hr/>

#### **propTypes**

```react
import PropTypes from 'prop-types'
	//对接收的props进行：类型、必要性的限制
	static propTypes = {
		todos:PropTypes.array.isRequired,
		updateTodo:PropTypes.func.isRequired,
		deleteTodo:PropTypes.func.isRequired,
	}
```



#### onComfirm? 确定删除吗

```react

	//删除一个todo的回调
	handleDelete = (id)=>{
		if(window.confirm('确定删除吗？')){
			this.props.deleteTodo(id)
		}
	}
```

#### keyboard 13--enter

```react
//键盘事件的回调
	handleKeyUp = (event)=>{
		//解构赋值获取keyCode,target
		const {keyCode,target} = event
		//判断是否是回车按键
		if(keyCode !== 13) return
		//添加的todo名字不能为空
		if(target.value.trim() === ''){
			alert('输入不能为空')
			return
		}
		//准备好一个todo对象
		const todoObj = {id:nanoid(),name:target.value,done:false}
		//将todoObj传递给App
		this.props.addTodo(todoObj)
		//清空输入
		target.value = ''
	}
  
<input onKeyUp={this.handleKeyUp} type="text" placeholder="请输入你的任务名称，按回车键确认"/>
```

#### get props info

```react
//1  ****常用
const {id,name,done} = props
           ||
           ||
const id = props.id
const name = props.name 
const done = props.done

//2
const{name:username} = props
					||
      		||
const username = props.name


//3 
const obj = {a: {b:1}}
const {a{b : data}} = obj
console.log(data) //1

            ||
            ||
data = obj.a.b //1



```

#### Checkbox

```react
<input type = "checkbox" checked ={.....true? false?...} onchange ={} />
//checked and onChange should used together
```

#### Arrow Function and conditon function

```react
const [enteredNameisValid, setEnteredName]=useState("true")
{!enterNameisValid && <p>The entered name is not valid</p>}  //show the <p> if the enteredNameisValid === false
{enterNameisValid ? <p>The entered name is valid</p> : <p>The entered name is NOT valid</p>}
func = () => {} //arrow function 
func = funct(){}
```



#### Array.reduce/map/filter/find

```




```



#### HTTP request/post

```







```

#### React中配置代理(`proxy`)

>1. `简单代理`:在package.json中追加如下配置 :`"proxy":http://localhost:5000`
>   - ps:当你请求`http://localhost:5000`产生跨域(本身在3000端口)时,添加此代码, 之后你请求时用`http://localhost:3000`进行请求,当其在`3000`端口中找不到资源时将会自动转发至`5000`端口进行请求,不产生跨域问题
>   - 优点：配置简单，前端请求资源时可以不加任何前缀。
>   - 缺点：不能配置多个代理
>   - 工作方式：上述方式配置代理，当请求了3000不存在的资源时，那么该请求会转发给5000 （优先匹配前端资源）
>2. 方法二: 在src下创建配置文件：`src/setupProxy.js`
>   - ps:必须是这个文件名,react项目运行的时候会自动查找这个文件,并将其加入webpack的配置中,所以当你修改此文件后,你需要重新启动项目
>   - 优点：可以配置多个代理，可以灵活的控制请求是否走代理。
>   - 缺点：配置繁琐，前端请求资源时必须加前缀。
>
>```js
>//代码示例
>const proxy = require('http-proxy-middleware')
> module.exports = function(app) {
>   app.use(
>     proxy('/api1', {  //api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
>       target: 'http://localhost:5000', //配置转发目标地址(能返回数据的服务器地址)
>       changeOrigin: true, //控制服务器接收到的请求头中host字段的值
>       /*
>       	changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
>       	changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:3000
>       	changeOrigin默认值为false，但我们一般将changeOrigin值设为true
>       */
>       pathRewrite: {'^/api1': ''} //去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
>     }),
>     proxy('/api2', { 
>       target: 'http://localhost:5001',
>       changeOrigin: true,
>       pathRewrite: {'^/api2': ''}
>     })
>   )
>}
>```



#### Axios

```
search = ()=>{
		//获取用户的输入(连续解构赋值+重命名)
		const {keyWordElement:{value:keyWord}} = this
		//发送请求前通知App更新状态
		this.props.updateAppState({isFirst:false,isLoading:true})
		//发送网络请求
		axios.get(`/api1/search/users?q=${keyWord}`).then(
			response => {
				//请求成功后通知App更新状态
				this.props.updateAppState({isLoading:false,users:response.data.items})
			},
			error => {
				//请求失败后通知App更新状态
				this.props.updateAppState({isLoading:false,err:error.message})
			}
		)
	}

```



#### `fetch`发送请求

> 概念:`关注分离`的设计思想

>1. Fetch 是浏览器提供的原生 AJAX 接口。
>
>由于原来的XMLHttpRequest`不符合关注分离原则`，且基于事件的模型在处理异步上已经没有现代的Promise等那么有优势。因此Fetch出现来解决这种问题。
>
>2. 特点:
>
>   - fetch: `原生函数`，不再使用XmlHttpRequest对象提交ajax请求
>
>   - `老版本浏览器可能不支持`
>
>   - 使用 fetch 无法`取消一个请求`。这是因为Fetch API`基于 Promise`，而Promise无法做到这一点。由于Fetch是典型的异步场景，所以大部分遇到的问题不是 Fetch 的，其实是 Promise 的。
>
> 3. 如果直接使用`fetch`,返回的并不是直接的结果它只是一个`HTTP响应`，而不是真的数据。想要获取数据,方法有二:
>
>    ① 使用async+await获取
>
>    ② 使用promise的链式调用,再第一个then中将其返回,再下个then中在使用
>
>4. 代码示例
>
>```js
>//代码示例
>----------------------------- 未优化:使用then链式调用 ---------------------------------------------------------
>fetch(`/api1/search/users2?q=${keyWord}`).then(
>			response => {
>				console.log('联系服务器成功了');
>				return response.json()
>			},
>			error => {
>				console.log('联系服务器失败了',error);
>				return new Promise(()=>{})
>			}
>		).then(
>			response => {console.log('获取数据成功了',response);},
>			error => {console.log('获取数据失败了',error);}
>) 
>----------------------------- 优化后:使用async+await ---------------------------------------------------------
>try {
>		const response= await fetch(`/api1/search/users2?q=${keyWord}`)
>		const data = await response.json()
>		console.log(data);
>		} catch (error) {
>		console.log('请求出错',error);
>		}
>}
>```



#### Redux in Function Component

```react
import { createStore } from 'redux';

const initialState = { counter: 0, name： null }
//Reducer方法返回一个完整的state obj, reducer方法用于接收dispatched message
const counterReducer = (state = initialState , action) => {  // the state will only be initialized once
  if (action.type === 'increment') {
    return {
      counter: state.counter + 1,
      name: null
    };
  }
  
  if (action.type === 'increasebyFive') {
    return {
      counter: state.counter + action.amount,
      name: null
    };
  }

  if (action.type === 'decrement') {
    return {
      ...state
      counter: state.counter - 1,
    };
  }
  return state;
};
const store = createStore(counterReducer);//创建一个store，并且传入一个reducer
export default store;
```

```react
import { useSelector, useDispatch } from 'react-redux';

import classes from './Counter.module.css';

const Counter = () => {
  const dispatch = useDispatch(); //used for dispathing message
  const counter = useSelector(state => state.counter); //subscribe to get new state of counter

  const incrementHandler = () => {
    dispatch({ type: 'increment' });
  };
  const decrementHandler = () => {
    dispatch({ type: 'decrement' });
  };
  const increasebyFive =()=>{
    dispatch({type: "increaseFive", amount: 5})//it can have mutiple values to deliver
  }
  const toggleCounterHandler = () => {..TODO..same as above};
  
  return (
    <main className={classes.counter}>
      <h1>Redux Counter</h1>
      <div className={classes.value}>{counter}</div>
      <div>
        <button onClick={incrementHandler}>Increment</button>
        <button onClick={increasebyFive}>Increment by 5</button>
        <button onClick={decrementHandler}>Decrement</button>
      </div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};

export default Counter;
```

#### **Router**

```react
class App extends Component {
  
  render() {
    return (
      <div>
        <div>
          <h2>Welcome to React Router</h2>

          <NavLink to={"welcome"}> Welcome </NavLink>
          <NavLink to={"home"}> Home </NavLink>
          <NavLink to={"about"}>About</NavLink>

          <Routes>
            <Route  path="welcome" element ={<Welcome/>} />
            <Route path="home/*" element={<Home />} />
            <Route  path="about" element={<About />} />
          </Routes>
        </div>
      </div>
    );
  }
}
```

#### Route in Router

```react
export default class Home extends Component {
  render() {
    return (
      <div>
        <h3>I am Home</h3>
        <div>
          <ul>
            <li>
              <NavLink to="news">News</NavLink>
            </li>
            <li>
              <NavLink to="message">Message</NavLink>
            </li>
          </ul>

          {/* 注册路由 */}
          <Routes>
            <Route  path="news" element={<News/>} />
            <Route  path="message/*" element={<Message />} />
          </Routes>

        </div>
      </div>
    );
  }
}
```



