# 如何书写更好的 React 代码

9 个书写更好的 React 代码实用技巧：学习关于 Linting 、propTypes 、PureComponent 等。

React 可以使创建交互式 UI 变得轻而易举。为应用中为每个状态（state）设计简单的视图，当数据发生变化时，React 将有效地更新和渲染出正确的组件。

在这篇文章中，我将向您展示一些有助于你成为更好地 React 开发者的技巧。我将介绍从工具到实际代码风格的一系列内容，它可以帮助你提高 React 的技能。



## 我们来谈谈 Linting

对于编写更好的代码来说一件非常重要的事就是良好的 linting （代码风格检测）。

> 译者注：lingting 是运行程序的过程中分析程序代码来查找错误。如 ESLint 。

因为如果我们配置了一套良好的 linting 规则，你的代码编辑器就能够捕获任何可能导致代码出现问题的内容。但不仅仅限于捕捉问题，你的 ESLint 配置还能不断地让你能够了解到 React 的最佳实践的代码风格。

```js
import react from 'react'
/* Other imports */

/* Code */

export default class App extends React.Component {
    render() {
        const { userIsLoaded, user } = this.props
        if (!userIsLoaded) return <Loader />
        return (
        	/* Code */
        )
    }
}
```

用上面的代码来说，假设你要在 render 函数中引用一个名为 `this.props.hello` 的新属性。你的 linter 将会立刻变红并且提示到：

```
'hello' is missing in props validation (react/prop-types)
// props 验证中缺少 'hello'
```

Linting 将会帮助你了解到 React 中的最佳实践并且塑造你对代码的理解。很快你就会在编写代码的时候避免这些错误。

你可以自己在 ESLint 为 JavaScript 设置 linting 规则，或者你可以使用  [Airbnb’s JavaScript Style Guide](https://github.com/airbnb/javascript) 。你可以安装  [React ESLint Package](https://www.npmjs.com/package/eslint-plugin-react) 。



## 共享和重用组件

使用类似于 [**Bit**](https://bit.dev/) 这样的工具去快速建造一个拥有可重复使用组件的新应用。

使用 Bit 你可以快速地共享和组织所有你的团队的组件，然后可以在任何其他项目中发现和安装这些组件。

![img](https://cdn-images-1.medium.com/max/800/1*Yhkh7jbS5Mx9uP96Y88pZg.gif)

Bit 帮助你了解组建的工作方式以及如何在实时渲染显示、自动文档和测试结果中使用它们。然后你就可以通过使用 NPM /Yarn 去安装这些组件。

![img](https://cdn-images-1.medium.com/max/800/1*ys-NwvhyZ20qYpg6ZD3kEg.gif)

最后，Bit 甚至可以让你使用 Bit 直接将组件导入任何项目，然后同时从多个项目开发它们。所有的改变都可以在代码库中被同步更新，这样可以更加促进相互协作。

Bit 是一个很好地平台，可以将组件的重用性转变为在更短时间内构建多应用程序的方法。快去试试吧！



## propTypes 和 defaultProps

在前面的部分中，我谈到了当我试图通过一个未经验证的 prop 时我的代码检测器是如何工作的。

```js
static propTypes = {
    userIsLoaded: PropTypes.boolean.isRequired,
    user: PropTypes.shape({
        _id: PropTypes.string,
    }).isRequired,
}
```

如果我们说不必须要传入 `userIsLoaded` ，那么我们需要将它添加到我们的代码中：

```js
static defaultProps = {
    userIsLoaded: false,
}
```

所以只要我们有一个在组件中使用到的 PropType ，我们就需要为它设置一个 prop 类型检测。在上面代码中，我们需要告诉 React `userIsLoaded` 总是一个布尔值。

还有如果我们不必须要传入 userIsLoaded 的话，我们就得给它设置一个默认的 prop 。如果它是必须传入的，我们就不用为它设置默认值。但是，规则也声明你不应该有像 `object` 或 `array`  这样模糊的类型检测。

这也是我们为什么会使用 `shape` 来验证 `user` 字段。上面代码表明了 `user` 中包含一个 `id` ，它的类型检测为 `String` ，并且整个 `user` 是一个必须传入的对象。

确保在每个 `props` 中使用 propTypes 和 defaultProps 还需要更多的学习和练习。

 当那些 props 没有得到它们期望的数据类型时，你的错误日志就会让你知道你传递了错误的 prop 或者是期望的数据类型不存在。这样使错误更容易被找到，特别是你编写了大量可重用组件的时候。并且使用 propTypes 还能使你的代码自文档化。



## 知道什么时候去提取新组件

```js
export default class Profile extends PureComponent {
    static propTypes = {
        userIsLoaded: PropTypes.bool,
        user: PropTypes.shape({
            _id: PropTypes.string,
        }).isRequired,
    }

	static defaultProps = {
    	userIsLoaded: false
	}

    render() {
		const { userIsLoaded, user } = this.props
        if (!userIsLoaded) return <Loaded />
        return (
        	<div>
                <div className="two-col">
                  <section>
                    <MyOrders userId={user.id} />
                    <My Downloads userId={user._id} />
                  </section>
                  <aside>
                    <MySubscriptions user={user} />
                    <MyVotes user={user} />
                  </aside>
                </div>
                <div className="one-col">
                  {isRole('affiliate', user={user._id} &&
                    <MyAffiliateInfo userId={user._id} />
                  }
                </div>
             </div>
        )
    }
}
```

这里我有一个叫做 `Profile` 的组件。在这个组件中我还有其他的像是 `MyOrder` 和 `MyDownloads` 这样的组件。现在我可以在这里编写所有的组件，因为我只从同一个地方（user）去提取数据，然后将所有的小组件转换为一个巨大的组件。

虽然没有任何关于何时将你的代码放入到你的组件中的硬性规定，但是你得问问自己：

* 你的代码功能是否变得臃肿？
* 它是代表了自己的东西么？(Dose it represent its own thing ?)
* 你是否需要重用其中的某些代码？

如果这些问题中的任何一个答案是肯定的话，你就需要将你的代码移动到一个组件中。

请记住，每个人看到一个组件有 200 - 300 行代码都是令人崩溃的。



## 组件 VS 纯组件 VS 无状态功能型组件

React 开发者必须要知道的一件很重要的事是知道何时在你的代码中使用**组件（Component）** 、**纯组件（PureComponent）** 和**无状态功能型组件（Stateless Functional Component）**。

#### 无状态功能组件

```js
const Billboard = () => (
  <ZoneBlack>
    <Heading>React</Heading>
    <div className="billboard_product">
      <Link className="billboard_product-image" to="/">
        <img alt="#" src="#">
      </Link>
      <div className="billboard_product-details">
        <h3 className="sub">React</h3>
        <p>Lorem Ipsum</p>
      </div>
    </div>
  </ZoneBlack>
)
```

无状态功能型组件是你代码中最常见的组件类型之一。它们为我们提供了一种简洁明了的方法来创建不使用  [**state**](https://reactjs.org/docs/faq-state.html) 、[**refs**](https://hackernoon.com/refs-in-react-all-you-need-to-know-fb9c9e2aeb81), or [**生命周期**](https://reactjs.org/docs/state-and-lifecycle.html) 的组件。

使用无状态功能组件的想法是它是完全无状态的，它只是一个功能。所以最棒的是你可以将组件定义为一个返回一些数据的常量函数。

简单来说，无状态功能型组件就是一个返回 JSX 的函数。最新的 React 版本给我们提供了 React hooks ，它让我们可以在函数型组建中使用 state 、effects 和 refs ， 而不是非要将它们转换为组件类。

#### 纯组件

通常，当组件获得新的 prop 时，React 将重新渲染该组件。但有的时候，一个组件获得没有改变的新 prop 时，React 仍然会触发重新渲染。

> 译者注： 如果一个组件只和 props 和 state 有关系，给定相同的 props 和 state 就会渲染出相同的结果，那么这个组件就叫做纯组件。

使用 `PureComponent` 将帮助你避免这种浪费的重新渲染。例如，如果一个 prop 是字符串或者布尔值并且它发生变化，`PureComponent` 将会识别到并重新渲染。如果是对象中的属性发生了变化，`PureComponent` 将不会发生重新渲染。

那如何才能知道 React 何时出发不必要的渲染呢？你可以试试使用这个令人惊喜的 React 包： [Why Did You Update](http://github.com/maicki/why-did-you-update) 。这个包的作用是：当发生不必要的重新渲染时，在控制台中通知你。

![img](https://cdn-images-1.medium.com/max/800/1*CL5jum98a0QxOWeIb9QRBg.png)

一旦识别出不必要的重新渲染，就可以使用 `PureComponent` 而不是 `Component` 来防止应用进行不必要的重新渲染。



## 使用 React 开发工具

如果你想要认真地想要成为一名专业的 React 开发者，那么在开发过程中使用 React Dev Tools 应该是常见的做法。

如果你使用过 React ，你的控制台可能不足以满足的需求，而是需要使用 React 开发工具 。

React 开发工具适用于所有主流浏览器，例如 [Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en) 和 [Firefox](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/) 。

通过 React 开发工具，你可以访问 React 应用程序的整个结构，并允许你查看应用程序中使用的所有 props 和 state 。

所以 React 开发工具是了解 React 组件并帮助调试应用问题的最佳方式。



## 使用内联条件语句

这个意见可能会引起一些争议，但是我发现使用内联条件语句的话可以大大减少我的 React 样板代码。

比如下面一段代码：

```jsx
<div className="one-col">
    {isRole('affiliate', user._id) &&
        <MyAffiliateInfo userId={user._id} />
    }
</div>
```

在这里我会用一个 `isRole`  函数检查一个人是否为联盟会成员，如果是的话渲染 `MyAffiliateInfo` 组件。

这样的好处是：

* 无需单独写一个函数。
* 无需再渲染函数中写一个 `if` 语句。
* 无需再组件中的其他位置创建一个 “链接”。

编写内联条件语句非常简单。首先你可以从编写一个条件语句开始，我们展示 `<MyAffiliateInfo />` 组件是在使用 `isRole`  判断身份为 `true` 时才显示出来。接下来我们使用 `&&` 将 `isRole` 和 `<MyAffiliateInfo />` 连接起来。这样只有在条件语句返回 `true` 时组件才会呈现。



## 尽可能地使用代码片段库

打开代码编辑器（以 VS code 为例），创建一个 a.js 文件。

在文件中当你输入 `rc` ，你可以看到如下情况：

![img](https://cdn-images-1.medium.com/max/800/1*DKVKG5IQB2XQ4GR1uEVDUw.png)

点击回车，你就马上可以得到这个：

![img](https://cdn-images-1.medium.com/max/800/1*ICQlmjGkoM_27Mz8tD1ZyA.png)



这些代码片段的优点在于他们不仅可以帮助你避免一些潜在的错误，还可以帮助你是用最新最好的语法。

VS code 里有许多不同的代码片段可以供你安装。我经常在 VS code 中使用的是 [ES7 React/Redux/React-Native/JS Snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets) 。



## [React Internals](http://www.mattgreer.org/articles/react-internals-part-one-basic-rendering/)——了解 React 是如何工作的

React Internals 是一个由 5 个部分组成的系列文章，帮助我理解了 React 的基础知识，并让我成为了一个更好地 React 开发者！

如果你遇到一些不能完全理解的问题，或者是你了解 React 的工作原理，那么 React Internals 将帮助你了解**何时**以及**如何**在 React 中做正确的事。

这些对那些有想法但又不知道在哪执行他们代码的人特别有用。

了解 React 如何工作的基础知识将帮助你成为更好地 React 开发者。



## 总结

1. 使用好的代码风格检测器（linting）。使用 ES Lint 、爱彼迎的 JavaScript 代码风格还有 React 的 ES Lint 插件。
2. 使用 propTypes 和 defaultProps 。
3. 知道何时该创建一个新组件。
4. 了解何时该去使用组件、纯组件和无状态函数（功能）型组件。
5. 使用 React 开发工具。
6. 在代码中使用内联条件语句。
7. 使用代码片段去创建样板代码。
8. 通过 React Internals 学习 React 工作原理。
9. 使用 Bit/StoryBook 改善你的组件开发工作流

