# React 官网笔记

[React 官网](https://reactjs.org/)
[React 中文官网](https://react.docschina.org/)

## Hello World

简单示例：
```js
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

## JSX 简介

1、JSX， 是一种 JavaScript 的语法扩展，其实也是一种表达式。  
2、可以在 JSX 当中使用 JavaScript 表达式，但是要包含在大括号里。  
3、推荐在 JSX 代码的外面扩上一个小括号，这样可以防止分号自动插入的 bug。  
4、Babel 转译器会把 JSX 转换成一个名为 React.createElement() 的方法调用，转化为普通的 JavaScript 对象。  

使用示例：
```js
const element1 = <h1>Hello, world!</h1>;
```

### JSX 属性

```js
// 使用引号来定义以字符串为值的属性
const element = <div tabIndex="0"></div>;
// 使用大括号来定义以 JavaScript 表达式为值的属性
const element = <img src={user.avatarUrl}></img>;
```

### JSX 嵌套

```js
// 如果 JSX 标签是闭合式的，那么你需要在结尾处用 />
const element = <img src={user.avatarUrl} />;
// JSX 标签可以相互嵌套
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

**注意：** 在 JSX 中，class 变成了 className，tabindex 变成了 tabIndex，for 变成了 htmlFor 。

## 元素渲染

元素是构成 React 应用的最小单位。

元素示例：
```js
const element = <h1>Hello, world</h1>;
```

## 组件 & Props

1、组件从概念上看就像是函数，它可以接收任意的输入值（称之为“props”），并返回一个需要在页面上展示的React元素。  
2、组件名称必须以大写字母开头。  
3、组合组件：组件可以在它的输出中引用其它组件。  
4、提取组件：可以将组件切分为更小的组件。  
5、Props 是只读性的。

### 函数定义

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

### 类定义组件

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

## State & 生命周期
