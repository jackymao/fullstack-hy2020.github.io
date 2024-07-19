---
mainImage: ../../../images/part-9.svg
part: 9
letter: d
lang: zh
---

<div class="content">

<!-- Before we start delving into how you can use TypeScript with React, we should first have a look at what we want to achieve. When everything works as it should, TypeScript will help us catch the following errors:-->
在我们开始深入研究如何将TypeScript与React结合使用之前，我们应该先看看我们想要达成的目标。当一切都按预期运行时，TypeScript将会帮助我们捕捉以下错误：

<!-- - Trying to pass an extra/unwanted prop to a component-->
尝试向组件传递额外/不需要的prop
<!-- - Forgetting to pass a required prop to a component-->
**忘记向组件传递一个必需的prop**
<!-- - Passing a prop with the wrong type to a component-->
传递一个类型错误的prop到组件

<!-- If we make any of these errors, TypeScript can help us catch them in our editor right away.-->
如果我们犯了任何这些错误，TypeScript 就可以帮助我们在编辑器中立即捕获它们。
<!-- If we didn''t use TypeScript, we would have to catch these errors later during testing.-->
如果我们不使用TypeScript，我们将不得不在测试过程中稍后捕获这些错误。
<!-- We might be forced to do some tedious debugging to find the cause of the errors.-->
我们可能被迫做一些乏味的调试来找出错误的原因。

<!-- That's enough reasoning for now. Let's start getting our hands dirty!-->
好了，现在讲到这里就够了，让我们开始动手吧！

### Create React App with TypeScript

<!-- We can use [create-react-app](https://create-react-app.dev) to create a TypeScript app by adding a-->
few flags

我们可以通过[create-react-app](https://create-react-app.dev)添加几个标志来创建一个TypeScript应用程序。
<i>template</i> argument to the initialization script. So in order to create a TypeScript Create React App, run the following command:

```shell
npx create-react-app my-app --template typescript
```

<!-- After running the command, you should have a complete basic React app that uses TypeScript.-->
在运行命令之后，您应该有一个使用TypeScript的完整的基本React应用程序。
<!-- You can start the app by running *npm start* in the application's root.-->
你可以在应用程序的根目录下运行*npm start*来启动应用。

<!-- If you take a look at the files and folders, you''ll notice that the app is not that different from-->
other apps.

如果你看一下这些文件和文件夹，你会发现这个应用程序跟其他应用程序并没有太大的不同。
<!-- one using pure JavaScript. The only differences are that the <i>.js</i> and <i>.jsx</i> files are now  <i>.ts</i> and <i>.tsx</i> files, they contain some type annotations, and the root directory contains a <i>tsconfig.json</i> file.-->
使用纯JavaScript，唯一的区别是<i>.js</i> 和 <i>.jsx</i> 文件现在是<i>.ts</i> 和 <i>.tsx</i> 文件，它们包含一些类型注释，根目录包含一个<i>tsconfig.json</i> 文件。

<!-- Now, let's take a look at the <i>tsconfig.json</i> file that has been created for us:-->
现在，让我们来看看为我们创建的<i>tsconfig.json</i>文件：

```js
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  },
  "include": [
    "src"
  ]
}
```

<!-- Notice *compilerOptions* now has the key [lib](https://www.typescriptlang.org/tsconfig#lib) that includes "type definitions for things found in browser environments (like *document*)."-->
**通知** *compilerOptions* 现在具有键 [lib](https://www.typescriptlang.org/tsconfig#lib)，其中包括“浏览器环境中发现的东西的类型定义（如*document*）。

<!-- Everything else should be more or less fine except that, at the moment, the configuration allows compiling JavaScript files because *allowJs* is set to *true*.-->
除此之外，其他的应该都比较正常，但目前，由于`allowJs`被设置为`true`，配置允许编译JavaScript文件。
<!-- That would be fine if you need to mix TypeScript and JavaScript (e.g. if you are in the process of transforming a JavaScript project into TypeScript or something like that), but we want to create a pure TypeScript app, so let's change that configuration to *false*.-->
如果你需要混合TypeScript和JavaScript（例如，如果你正在将一个JavaScript项目转换为TypeScript或类似的东西），那将是很好的，但我们想要创建一个纯TypeScript应用程序，所以让我们将该配置更改为* false *。

<!-- In our previous project, we used ESlint to help us enforce a coding style, and we''ll do the same with this app. We do not need to install any dependencies, since create-react-app has taken care of that already.-->
在我们之前的项目中，我们使用ESLint来帮助我们强制执行编码风格，本应用也会这么做。我们不需要安装任何依赖项，因为create-react-app已经处理好了。

<!-- We configure ESlint in <i>.eslintrc</i> with the following settings:-->
我们在<i>.eslintrc</i>中使用以下设置配置ESlint：

```js
{
  "env": {
    "browser": true,
    "es6": true,
    "jest": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "plugins": ["react", "@typescript-eslint"],
  "settings": {
    "react": {
      "pragma": "React",
      "version": "detect"
    }
  },
  "rules": {
    "@typescript-eslint/explicit-function-return-type": 0,
    "@typescript-eslint/explicit-module-boundary-types": 0,
    "react/react-in-jsx-scope": 0
  }
}
```

<!-- Since the return type of most React components is generally either *JSX.Element* or *null*, we have loosened up the default linting rules a bit by disabling the rules [explicit-function-return-type](https://github.com/typescript-eslint/typescript-eslint/blob/master/packages/eslint-plugin/docs/rules/explicit-function-return-type.md) and [explicit-module-boundary-types](https://github.com/typescript-eslint/typescript-eslint/blob/master/packages/eslint-plugin/docs/rules/explicit-module-boundary-types.md).-->
由于大多数 React 组件的返回类型通常是 *JSX.Element* 或 *null*，我们已经放宽了默认的 linting 规则，通过禁用[explicit-function-return-type](https://github.com/typescript-eslint/typescript-eslint/blob/master/packages/eslint-plugin/docs/rules/explicit-function-return-type.md)和[explicit-module-boundary-types](https://github.com/typescript-eslint/typescript-eslint/blob/master/packages/eslint-plugin/docs/rules/explicit-module-boundary-types.md)规则来实现。
<!-- Now we don''t need to explicitly state our function return types everywhere. We will also disable [react/react-in-jsx-scope](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/react-in-jsx-scope.md) since importing React is [no longer needed](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html) in every file.-->
现在我们不需要在每个地方都明确声明函数的返回类型。由于[不再需要](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)在每个文件中导入React，我们也将禁用[react/react-in-jsx-scope](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/react-in-jsx-scope.md)。

<!-- Next, we need to get our linting script to parse <i>*.tsx </i> files, which are the TypeScript equivalent of React's JSX files.-->
下一步，我们需要让我们的 linting 脚本解析 <i>*.tsx </i> 档案，它们是 TypeScript 的 React 的 JSX 档案的等价物。
<!-- We can do that by altering our lint command in <i>package.json</i> to the following:-->
我们可以通过更改<i>package.json</i>中的lint命令来实现：

```json
{
  // ...
    "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "lint": "eslint './src/**/*.{ts,tsx}'" // highlight-line
  },
  // ...
}
```

<!-- If you are using Windows, you may need to use double quotes for the linting path:-->
如果你正在使用Windows，你可能需要使用双引号来为linting路径：

```json
"lint": "eslint \"./src/**/*.{ts,tsx}\""
```

### React components with TypeScript

<!-- Let us consider the following JavaScript React example:-->
让我们来看看以下JavaScript React示例：

```jsx
import ReactDOM from 'react-dom/client'
import PropTypes from "prop-types";

const Welcome = props => {
  return <h1>Hello, {props.name}</h1>;
};

Welcome.propTypes = {
  name: PropTypes.string
};

ReactDOM.createRoot(document.getElementById('root')).render(
  <Welcome name="Sarah" />
)
```

<!-- In this example, we have a component called *Welcome* to which we pass a *name* as a prop. It then renders the name to the screen.  We know that the *name* should be a string, and we use the [prop-types](https://www.npmjs.com/package/prop-types) package introduced in [part 5](/en/part5/props_children_and_proptypes#prop-types) to receive hints about the desired types of a component's props and warnings about invalid prop types.-->
在这个例子中，我们有一个叫做*Welcome*的组件，我们向它传递一个*name*作为prop。然后它将name渲染到屏幕上。我们知道*name*应该是一个字符串，我们使用在[第五章节](/en/part5/props_children_and_proptypes#prop-types)中介绍的[prop-types](https://www.npmjs.com/package/prop-types)包来获得有关组件prop期望类型的提示和有关无效prop类型的警告。

<!-- With TypeScript, we don''t need the <i>prop-types</i> package anymore. We can define the types with the help of TypeScript just like we define types for a regular function as React components are nothing but mere functions. We will use an interface for the parameter types (i.e., props) and *JSX.Element* as the return type for any react component:-->
随着TypeScript的出现，我们不再需要<i>prop-types</i> 这个包了。我们可以像定义普通函数类型一样，用TypeScript定义React组件类型。我们将使用接口来定义参数类型（即props），以及*JSX.Element*作为任何React组件的返回类型：

```jsx
import ReactDOM from 'react-dom/client'

interface WelcomeProps {
  name: string;
}

const Welcome = (props: WelcomeProps): JSX.Element => {
  return <h1>Hello, {props.name}</h1>;
};

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
  <Welcome name="Sarah" />
)
```

<!-- We defined a new type, *WelcomeProps*, and passed it to the function's parameter types.-->
我们定义了一个新类型，*WelcomeProps*，并将其传递给函数的参数类型。

```jsx
const Welcome = (props: WelcomeProps): JSX.Element => {
```

<!-- You could write the same thing using a more verbose syntax:-->
你可以使用更加冗长的句法来写同样的东西：

```jsx
const Welcome = ({ name }: { name: string }): JSX.Element => (
  <h1>Hello, {name}</h1>
);
```

<!-- Now our editor knows that the *name* prop is a string.-->
现在我们的编辑知道*name* prop 是一个字符串。

<!-- There is actually no need to define the return type of a React component since the TypeScript compiler infers the type automatically, and we can just write-->
the code without specifying the return type.

实际上，由于TypeScript编译器会自动推断类型，因此无需定义React组件的返回类型，我们可以在不指定返回类型的情况下编写代码。

```jsx
interface WelcomeProps {
  name: string;
}

const Welcome = (props: WelcomeProps)  => { // highlight-line
  return <h1>Hello, {props.name}</h1>;
};

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
  <Welcome name="Sarah" />
)
```

<!-- You probably noticed that we used a [type assertion](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions) for the return value of the function *document.getElementById*-->
你可能已经注意到，我们为函数*document.getElementById*的返回值使用了[类型断言](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions)。

```ts
ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(  // highlight-line
  <Welcome name="Sarah" />
)
```

<!-- We need to do this since the *ReactDOM.createRoot* takes an HTMLElement as a parameter but the return value of function *document.getElementById* has the following type-->
:

我们需要这样做，因为*ReactDOM.createRoot*需要一个HTMLElement作为参数，但是函数*document.getElementById*的返回值的类型是：

```js
HTMLElement | null
```

<!-- since if the function does not find the searched element, it will return null.-->
若函数没有找到被搜索的元素，它将返回null。

<!-- Earlier in this part we [warned](https://fullstackopen.com/en/part9/first_steps_with_type_script#type-assertion) about the dangers of type assertions, but in our case the assertion is ok since we are sure that the file <i>index.html</i> indeed has this particular id and the function is always returning a HTMLElement.-->
在本部分的早些时候，我们[警告](https://fullstackopen.com/en/part9/first_steps_with_type_script#type-assertion)关于类型断言的危险，但是在我们的例子中，断言是可以的，因为我们确信文件<i>index.html</i>确实具有这个特定的id，而且该函数总是返回一个HTMLElement。

</div>

<div class="tasks">

### Exercise 9.14

#### 9.14

<!-- Create a new Create React App with TypeScript, and set up ESlint for the project similarly to how we just did.-->
创建一个新的带有TypeScript的Create React App，并像我们刚才做的那样为该项目设置ESLint。

<!-- This exercise is similar to the one you have already done in [Part 1](/en/part1/java_script#exercises-1-3-1-5) of the course, but with TypeScript and some extra tweaks. Start off by modifying the contents of <i>index.tsx</i> to the following:-->
这个练习与课程[第一章节](/en/part1/java_script#exercises-1-3-1-5)中你已经完成的练习类似，但使用TypeScript并做了一些额外的调整。首先，将<i>index.tsx</i>的内容修改为以下内容：

```jsx
import ReactDOM from 'react-dom/client'
import App from './App';

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
  <App />
)
```

<!-- and <i>App.tsx</i> to the following:-->
# App.tsx

```
App.tsx
```

```jsx
const App = () => {
  const courseName = "Half Stack application development";
  const courseParts = [
    {
      name: "Fundamentals",
      exerciseCount: 10
    },
    {
      name: "Using props to pass data",
      exerciseCount: 7
    },
    {
      name: "Deeper type usage",
      exerciseCount: 14
    }
  ];

  return (
    <div>
      <h1>{courseName}</h1>
      <p>
        {courseParts[0].name} {courseParts[0].exerciseCount}
      </p>
      <p>
        {courseParts[1].name} {courseParts[1].exerciseCount}
      </p>
      <p>
        {courseParts[2].name} {courseParts[2].exerciseCount}
      </p>
      <p>
        Number of exercises{" "}
        {courseParts.reduce((carry, part) => carry + part.exerciseCount, 0)}
      </p>
    </div>
  );
};

export default App;
```

<!-- and remove the unnecessary files.-->
# 移除不必要的文件

移除不必要的文件是一项重要的任务，可以确保系统的有效运行。定期清理系统可以帮助提高系统的性能，减少系统可能出现的问题。清理系统时，最好先备份重要的文件，然后再删除不必要的文件。

<!-- The whole app is now in one component. That is not what we want, so refactor the code so that it consists of three components: *Header*,  *Content* and *Total*. All data is still kept in the *App* component, which passes all necessary data to each component as props. <i>Be sure to add type declarations for each component's props!</i>-->
整个应用现在只有一个组件。这不是我们想要的，所以重构代码，使其由三个组件组成：*Header*、*Content* 和 *Total*。所有数据仍然保留在*App*组件中，它将所有必要的数据作为props传递给每个组件。<i>务必为每个组件的props添加类型声明！</i>

<!-- The *Header* component should take care of rendering the name of the course. *Content* should render the names of the different parts and the number of exercises in each part, and *Total* should render the total sum of exercises in all parts.-->
# 组件*Header*应该负责渲染课程的名称。*Content*应该渲染不同部分的名称以及每个部分的练习数量，而*Total*应该渲染所有部分练习总数。

<!-- The *App* component should look somewhat like this:-->
*应用*组件应该看起来有点像这样：

```jsx
const App = () => {
  // const-declarations

  return (
    <div>
      <Header name={courseName} />
      <Content ... />
      <Total ... />
    </div>
  )
};
```

</div>

<div class="content">

### Deeper type usage

<!-- In the previous exercise, we had three parts of a course, and all parts had the same attributes *name* and *exerciseCount*. But what if we needed additional attributes for the parts where all parts do not have the same attributes? How would this look, codewise? Let's consider the following example:-->
在前一个练习中，我们有三个课程的部分，所有部分都具有相同的属性*名称*和*练习计数*。但是，如果我们需要为所有部分都不相同属性的部分添加额外属性时，代码会是什么样子？让我们考虑以下示例：

```js
const courseParts = [
  {
    name: "Fundamentals",
    exerciseCount: 10,
    description: "This is an awesome course part"
  },
  {
    name: "Using props to pass data",
    exerciseCount: 7,
    groupProjectCount: 3
  },
  {
    name: "Basics of type Narrowing",
    exerciseCount: 7,
    description: "How to go from unknown to string"
  },
  {
    name: "Deeper type usage",
    exerciseCount: 14,
    description: "Confusing description",
    backgroundMaterial: "https://type-level-typescript.com/template-literal-types"
  },
];
```

<!-- In the above example, we have added some additional attributes to each course part.-->
在上面的例子中，我们给每个课程部分添加了一些额外的属性。
<!-- Each part has the *name* and *exerciseCount* attributes, but the first, the third  and fourth also have an attribute called *description*, and the second and fourth parts also have some distinct additional attributes.-->
每部分都有*name*和*exerciseCount*属性，但是第一、第三和第四章节还有一个叫做*description*的属性，而第二和第四章节还有一些不同的附加属性。

<!-- Let's imagine that our application just keeps on growing, and we need to pass the different course parts around in our code. On top of that, there are also additional attributes and course parts added to the mix. How can we know that our code is capable of handling all the different types of data correctly, and we are not for example forgetting to render a new course part on some page? This is where TypeScript comes in handy!-->
让我们想象一下，我们的应用程序不断增长，我们需要在代码中传递不同的课程部分。此外，还添加了其他属性和课程部分。我们怎么知道我们的代码能够正确处理所有不同类型的数据，而不是例如忘记在某些页面上渲染新课程部分？这就是TypeScript发挥作用的地方！

<!-- Let's start by defining types for our different course parts. We notice that the first and third have the same set of attributes. The second and fourth are a bit different so we have three different kinds of course part elements.-->
让我们从定义我们不同课程部分的类型开始。我们注意到第一和第三有相同的属性集。第二和第四有点不同，所以我们有三种不同类型的课程部分元素。

<!-- So let us define a type for each of the different kind of course parts:-->
所以让我们为不同类型的课程部分定义一种类型：

```js
interface CoursePartBasic {
  name: string;
  exerciseCount: number;
  description: string;
  kind: "basic"
}

interface CoursePartGroup {
  name: string;
  exerciseCount: number;
  groupProjectCount: number;
  kind: "group"
}

interface CoursePartBackground {
  name: string;
  exerciseCount: number;
  description: string;
  backgroundMaterial: string;
  kind: "background"
}
```

<!-- Besides the attributes that are found in the various course parts, we have now introduced an additional attribute called <i>kind</i> that has a [literal](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types) type, it is a "hard coded" string, distinct for each course part. We shall soon see where the attribute kind is used!-->
除了在各个课程部分中发现的属性之外，我们现在引入了一个名为<i>kind</i>的附加属性，它具有[文字](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types)类型，它是一个“硬编码”的字符串，每个课程部分都是不同的。我们很快就会看到属性kind的用途！

<!-- Next, we will create a type [union](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types) of all these types. We can then use it to define a type for our array, which should accept any of these course part types:-->
接下来，我们将创建一种[联合](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types)类型的所有这些类型。然后，我们可以使用它来为我们的数组定义一种类型，它应该接受以下任何课程部分类型：

```js
type CoursePart = CoursePartBasic | CoursePartGroup | CoursePartBackground;
```

<!-- Now we can set the type for our *courseParts* variable:-->
现在我们可以为我们的*courseParts*变量设置类型：

```js
const App = () => {
  const courseName = "Half Stack application development";
  const courseParts: CoursePart[] = [
    {
      name: "Fundamentals",
      exerciseCount: 10,
      description: "This is an awesome course part",
      kind: "basic" // highlight-line
    },
    {
      name: "Using props to pass data",
      exerciseCount: 7,
      groupProjectCount: 3,
      kind: "group" // highlight-line
    },
    {
      name: "Basics of type Narrowing",
      exerciseCount: 7,
      description: "How to go from unknown to string",
      kind: "basic" // highlight-line
    },
    {
      name: "Deeper type usage",
      exerciseCount: 14,
      description: "Confusing description",
      backgroundMaterial: "https://type-level-typescript.com/template-literal-types",
      kind: "background" // highlight-line
    },
  ]

  // ...
}
```

<!-- Note that we have now added the attribute *kind* with a proper value to each element of the array.-->
注意，我们现在已经为数组的每个元素添加了带有适当值的属性*kind*。

<!-- Our editor will automatically warn us if we use the wrong type for an attribute, use an extra attribute, or forget to set an expected attribute. If we eg. try to add the following to the array-->
:

我们的编辑器会自动警告我们，如果我们使用错误的类型为属性，使用额外的属性，或忘记设置预期的属性。如果我们例如试图添加以下到数组：

```js
{
  name: "TypeScript in frontend",
  exerciseCount: 10,
  kind: "basic",
},
```

<!-- We will immediately see an error in the editor:-->
我们将立即在编辑器中看到一个错误：

![vscode exerciseCoutn not assignable to type CoursePart - description missing](../../images/9/63new.png)

<!-- Since our new entry has the attribute *kind* with value *"basic"* TypeScript knows that the entry does not only have the type *CoursePart* but it is actually meant to be a *CoursePartBasic*. So here the attribute *kind* "narrows" the type of the entry from a more general to a more specific type that has a certain set of attributes. We shall soon see this style of type narrowing in action in the code!-->
因为我们的新项目有属性*kind*，值为*"basic"*，TypeScript知道该项目不仅具有类型*CoursePart*，而且它实际上应该是一个*CoursePartBasic*。因此，属性*kind*将条目的类型从更一般的类型窄化为具有一定属性集的更具体的类型。我们很快就会在代码中看到这种类型窄化的风格！

<!-- But we''re not satisfied yet! There is still a lot of duplication in our types, and we want to avoid that. We start by identifying the attributes all course parts have in common, and defining a base type that contains them. Then we will [extend](https://www.typescriptlang.org/docs/handbook/2/objects.html#extending-types) that base type to create our kind-specific types:-->
但我们还不满意！我们的类型中仍然有很多重复，我们想避免这种情况。我们首先确定所有课程部分具有的属性，并定义一个包含它们的基本类型。然后，我们将[扩展](https://www.typescriptlang.org/docs/handbook/2/objects.html#extending-types)该基本类型以创建我们特定类型的类型：

```js
interface CoursePartBase {
  name: string;
  exerciseCount: number;
}

interface CoursePartBasic extends CoursePartBase {
  description: string;
  kind: "basic"
}

interface CoursePartGroup extends CoursePartBase {
  groupProjectCount: number;
  kind: "group"
}

interface CoursePartBackground extends CoursePartBase {
  description: string;
  backgroundMaterial: string;
  kind: "background"
}

type CoursePart = CoursePartBasic | CoursePartGroup | CoursePartBackground;
```

### More type narrowing

<!-- How should we now use these types in our components?-->
怎么样现在我们在组件中使用这些类型？

<!-- If we try to access the objects in the array *courseParts: CoursePart[]* we notice that it is possible to only access the attributes that are common to all the types in the union:-->
如果我们尝试访问数组*courseParts：CoursePart []*中的对象，我们会注意到只能访问联合中所有类型共有的属性：

![vscode showing part.exerciseCou](../../images/9/65new.png)

<!-- And indeed, the TypeScript [documentation](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#working-with-union-types) says this:-->
而且，TypeScript [文档](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#working-with-union-types) 也是这么说的：

<!-- > <i>TypeScript will only allow an operation (or attribute access) if it is valid for every member of the union.</i>-->
> <i>TypeScript 只有在操作（或属性访问）对联合中的每个成员都有效时才允许。</i>

<!-- The documentation also mentions the following:-->
文档还提到：

<!-- > <i>The solution is to narrow the union with code... Narrowing occurs when TypeScript can deduce a more specific type for a value based on the structure of the code.</i>-->
> <i>解决方案是通过代码缩小联盟... 缩小发生时，TypeScript可以根据代码的结构推断出更具体的值类型。</i>

<!-- So once again the [type narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html) is the rescue!-->
所以，[类型缩小](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)又一次拯救了我们！

<!-- One handy way to narrow these kinds of types in TypeScript is to use *switch case* expressions. Once TypeScript has inferred that a variable is of union type and that each type in the union contain a certain literal attribute (in our case *kind*), we can use that as a type identifier. We can then build a switch case around that attribute and TypeScript will know which attributes are available within each case block:-->
一个方便的方法来缩小这种类型在TypeScript中是使用*switch case*表达式。一旦TypeScript推断出一个变量是联合类型，并且联合中的每个类型包含一个特定的文字属性（在我们的例子中是*kind*），我们可以将其作为类型标识符。然后我们可以在该属性周围构建一个switch case，TypeScript将知道每个case块中可用的属性：

![vscode showing part. and then the attributes](../../images/9/64new.png)

<!-- In the above example, TypeScript knows that a *part* has the type *CoursePart* and it can then infer that *part* is of either type *CoursePartBasic*, *CoursePartGroup* or *CoursePartBackground* based on the value of the attribute *kind*.-->
在上面的例子中，TypeScript知道一个*部分*具有类型*CoursePart*，然后它可以根据属性*kind*的值推断*part*是*CoursePartBasic*、*CoursePartGroup*或*CoursePartBackground*中的一种。

<!-- The specific technique of type narrowing where a union type is narrowed based on literal attribute value is called [discriminated union](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#discriminated-unions).-->
具体的类型缩小技术，即基于文字属性值缩小联合类型，被称为[鉴别联合](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#discriminated-unions)。

<!-- Note that the narrowing can naturally be also done with *if* clause. We could eg. do the following:-->
注意，缩小范围也可以使用*if*子句来实现。例如，我们可以这样做：

```js
  courseParts.forEach(part => {
    if (part.kind === 'background') {
      console.log('see the following:', part.backgroundMaterial)
    }

    // can not refer to part.backgroundMaterial here!
  });
```

<!-- What about adding new types? If we were to add a new course part, wouldn''t it be nice to know if we had already implemented handling that type in our code? In the example above, a new type would go to the *default* block and nothing would get printed for a new type. Sometimes this is wholly acceptable. For instance, if you wanted to handle only specific (but not all) cases of a type union, having a default is fine. Nonetheless, it is recommended to handle all variations separately in most cases.-->
如果我们要添加一个新的课程部分，不知道我们已经在代码中实现了处理那种类型会不会更好？在上面的例子中，一个新类型会进入*默认*块，并且对于一个新类型不会打印任何内容。有时这是完全可以接受的。例如，如果你只想处理特定（但不是所有）类型联合的情况，有一个默认值是可以的。但是，在大多数情况下，建议单独处理所有变化。

<!-- With TypeScript, we can use a method called [exhaustive type checking](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#exhaustiveness-checking). Its basic principle is that if we encounter an unexpected value, we call a function that accepts a value with the type [never](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#the-never-type) and also has the return type *never*.-->
使用TypeScript，我们可以使用一种叫做[穷尽性类型检查](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#exhaustiveness-checking)的方法。它的基本原理是，如果我们遇到意外的值，就调用一个接受类型为[never](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#the-never-type)的值并且返回类型为*never*的函数。

<!-- A straightforward version of the function could look like this:-->
一个直接的函数版本可能如下：

```js
/**
 * Helper function for exhaustive type checking
 */
const assertNever = (value: never): never => {
  throw new Error(
    `Unhandled discriminated union member: ${JSON.stringify(value)}`
  );
};
```

<!-- If we now were to replace the contents of our *default* block to:-->
如果我们现在把我们的*默认*块替换为：

```js
default:
  return assertNever(part);
```

<!-- and remove the case that handles the type *CoursePartBackground*, we would see the following error:-->
如果我们删除处理类型*CoursePartBackground*的情况，我们会看到以下错误：

![vscode error Argument of Ttype CoursePart not assignable to type never](../../images/9/66new.png)

<!-- The error message says that-->
the file is corrupted

错误信息表明文件已损坏

```text
'CoursePartBackground' is not assignable to parameter of type 'never'.
```

<!-- which tells us that we are using a variable somewhere where it should never be used. This tells us that something needs to be fixed.-->
这告诉我们，我们在一个不该使用变量的地方使用了变量，这告诉我们有些东西需要修复。

</div>

<div class="tasks">

### Exercise 9.15

#### 9.15

<!-- Let us now continue extending the app created in exercise 9.14. First, add the type information and replace the variable *courseParts* with the one from the example below.-->
让我们继续扩展在练习9.14中创建的应用程序。首先，添加类型信息，并用下面的变量*courseParts*替换它。

```js
interface CoursePartBase {
  name: string;
  exerciseCount: number;
}

interface CoursePartBasic extends CoursePartBase {
  description: string;
  kind: "basic"
}

interface CoursePartGroup extends CoursePartBase {
  groupProjectCount: number;
  kind: "group"
}

interface CoursePartBackground extends CoursePartBase {
  description: string;
  backgroundMaterial: string;
  kind: "background"
}

type CoursePart = CoursePartBasic | CoursePartGroup | CoursePartBackground;

const courseParts: CoursePart[] = [
  {
    name: "Fundamentals",
    exerciseCount: 10,
    description: "This is an awesome course part",
    kind: "basic"
  },
  {
    name: "Using props to pass data",
    exerciseCount: 7,
    groupProjectCount: 3,
    kind: "group"
  },
  {
    name: "Basics of type Narrowing",
    exerciseCount: 7,
    description: "How to go from unknown to string",
    kind: "basic"
  },
  {
    name: "Deeper type usage",
    exerciseCount: 14,
    description: "Confusing description",
    backgroundMaterial: "https://type-level-typescript.com/template-literal-types",
    kind: "background"
  },
  {
    name: "TypeScript in frontend",
    exerciseCount: 10,
    description: "a hard part",
    kind: "basic",
  },
];
```

<!-- Now we know that both interfaces *CoursePartBasic* and *CoursePartBackground* share not only the base attributes but also an attribute called *description*, which is a string in both interfaces.-->
现在我们知道，*CoursePartBasic*和*CoursePartBackground*两个接口不仅共享基本属性，还共享一个叫做*description*的属性，它在两个接口中都是一个字符串。

<!-- Your first task is to declare a new interface that includes the *description* attribute and extends the *CoursePartBase* interface. Then modify the code so that you can remove the *description* attribute from both *CoursePartBasic* and *CoursePartBackground*  without getting any errors.-->
你的第一个任务是定义一个新的接口，该接口包括*description*属性，并扩展*CoursePartBase*接口。然后修改代码，以便可以不出现任何错误的情况下从*CoursePartBasic*和*CoursePartBackground*中删除*description*属性。

<!-- Then create a component *Part* that renders all attributes of each type of course part. Use a switch case-based exhaustive type checking! Use the new component in component *Content*.-->
然后创建一个组件*Part*，渲染每种课程部分的所有属性。使用基于开关情况的详尽类型检查！在组件*Content*中使用新组件。

<!-- Lastly, add another course part interface with the following attributes: *name*, *exerciseCount*, *description* and *requirements*, the latter being a string array. The objects of this type look like the following:-->
最后，添加另一个具有以下属性的课程部分界面：*名称*，*练习次数*，*描述*和*要求*，其中后者是字符串数组。此类型的对象如下所示：

```js
{
  name: "Backend development",
  exerciseCount: 21,
  description: "Typing the backend",
  requirements: ["nodejs", "jest"],
  kind: "special"
}
```

<!-- Then add that interface to the type union *CoursePart* and add corresponding data to the *courseParts* variable. Now, if you have not modified your *Content* component correctly, you should get an error, because you have not yet added support for the fourth course part type. Do the necessary changes to *Content*, so that all attributes for the new course part also get rendered and that the compiler doesn''t produce any errors.-->
然后将该接口添加到类型联合*CoursePart*中，并将相应的数据添加到*courseParts*变量中。现在，如果您没有正确修改*Content*组件，您应该会得到一个错误，因为您尚未添加对第四个课程部分类型的支持。做必要的更改*Content*，以便渲染新课程部分的所有属性，并且编译器不会产生任何错误。

<!-- The result might look like the following:-->
结果可能如下：

![browser showing half stack application development](../../images/9/45.png)

</div>

<div class="content">

### React app with state

<!-- So far, we have only looked at an application that keeps all the data in a typed variable but does not have any state. Let us once more go back to the note app, and build a typed version of it.-->
到目前为止，我们只看了一个将所有数据都存储在类型变量中但没有任何状态的应用程序。让我们再回到笔记应用程序，构建一个类型版本。

<!-- We start with the following code:-->
我们从以下代码开始：

```js
import { useState } from 'react';

const App = () => {
  const [newNote, setNewNote] = useState('');
  const [notes, setNotes] = useState([]);

  return null
}
```

<!-- When we hover over the *useState* calls in the editor, we notice couple of interesting things.-->
当我们在编辑器中悬停在*useState*调用上，我们注意到几件有趣的事情。

<!-- The type of the first call *useState('')* looks like the following:-->
第一次调用*useState('')*的类型看起来像下面这样：

```ts
useState<string>(initialState: string | (() => string)):
  [string, React.Dispatch<React.SetStateAction<string>>]
```

<!-- The type is somewhat challenging to decipher. It has the following "form":-->
这种类型有点难以解读。它有以下“形式”：

```ts
functionName(parameters): return_value
```

<!-- So we notice that TypeScript compiler has inferred that the initial state is either a string or a function that returns a string:-->
因此，我们注意到TypeScript编译器推断初始状态可能是一个字符串或者一个返回字符串的函数：

```ts
initialState: string | (() => string)
```

<!-- The type of the returned array is the following:-->
返回的数组类型如下：

```ts
[string, React.Dispatch<React.SetStateAction<string>>]
```

<!-- So the first element, assigned to *newNote* is a string and the second element that we assigned *setNewNote* has a slightly more complex type. We notice that there is a string mentioned there, so we know that it must be the type of a function that sets a valued data. See [here](https://codewithstyle.info/Using-React-useState-hook-with-TypeScript/) if you want to learn more about the types of useState function.-->
所以，我们分配给*newNote*的第一个元素是一个字符串，而我们分配给*setNewNote*的第二个元素类型稍微复杂一些。我们注意到那里提到了一个字符串，因此我们知道它必须是设置有价值数据的函数的类型。如果你想了解更多关于useState函数类型的信息，请参见[这里](https://codewithstyle.info/Using-React-useState-hook-with-TypeScript/)。

<!-- From this all we see that TypeScript has indeed [inferred](https://www.typescriptlang.org/docs/handbook/type-inference.html#handbook-content) the type of the first useState quite right, it is creating a state with type string.-->
从这一切我们可以看出，TypeScript确实[推断](https://www.typescriptlang.org/docs/handbook/type-inference.html#handbook-content)了第一个useState的类型，它正在创建一个类型为字符串的状态。

<!-- When we look at the second useState that has the initial value *[]* the type looks quite different-->
当我们看到第二个useState，它的初始值为*[]*，类型看起来就有些不同了。

```ts
useState<never[]>(initialState: never[] | (() => never[])):
  [never[], React.Dispatch<React.SetStateAction<never[]>>]
```

<!-- TypeScript can just infer that the state has type *never[]*, it is an array but it has no clue what are the elements stored to array, so we clearly need to help the compiler and provide the type explicitly.-->
TypeScript可以推断出状态的类型是*never[]*，它是一个数组，但它不知道数组中存储的是什么元素，因此我们明确需要帮助编译器并提供明确的类型。

<!-- One of the best sources for information about typing React is the [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/).-->
一个最佳的关于输入React的信息来源是[React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)。

<!-- The chapter about [useState](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/hooks#usestate) hook instructs to use a <i>type parameter</i> in situations where the compiler can not infer the type.-->
第关于[useState](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/hooks#usestate) hook的章节指示在编译器无法推断类型的情况下使用<i>类型参数</i>。

<!-- Let us now define a type for notes:-->
让我们现在为笔记定义一种类型：

```js
interface Note {
  id: number,
  content: string
}
```

<!-- The solution is now simple:-->
现在的解决方案很简单：

```js
const [notes, setNotes] = useState<Note[]>([]);
```

<!-- And indeed, the type is set quite right:-->
而且，字体设置的确很正确：

```ts
useState<Note[]>(initialState: Note[] | (() => Note[])):
  [Note[], React.Dispatch<React.SetStateAction<Note[]>>]
```

<!-- So in technical terms useState is [a generic function](https://www.typescriptlang.org/docs/handbook/2/generics.html#working-with-generic-type-variables), where the type has to be specified as a type parameter in those cases when the compiler can not infer the type.-->
所以从技术角度来讲，useState是[一个泛型函数](https://www.typescriptlang.org/docs/handbook/2/generics.html#working-with-generic-type-variables)，在编译器无法推断出类型的情况下，必须将类型作为类型参数指定。

<!-- Rendering the notes is now easy. Let us just add some data to the state so that we can see that the code works:-->
渲染笔记现在很容易。让我们只添加一些数据到状态中，这样我们就可以看到代码是否有效：

```js
interface Note {
  id: number,
  content: string
}

import { useState } from "react";

const App = () => {
  const [notes, setNotes] = useState<Note[]>([
    { id: 1, content: 'testing' } // highlight-line
  ]);
  const [newNote, setNewNote] = useState('');

  return (
    // highlight-start
    <div>
      <ul>
        {notes.map(note =>
          <li key={note.id}>{note.content}</li>
        )}
      </ul>
    </div>
    // highlight-end
  )
}
```

<!-- The next task is to add a form that makes it possible to create new notes:-->
下一个任务是添加一个表单，使其可以创建新的笔记：

```js
const App = () => {
  const [notes, setNotes] = useState<Note[]>([
    { id: 1, content: 'testing' }
  ]);
  const [newNote, setNewNote] = useState('');

  return (
    <div>
      // highlight-start
      <form>
        <input
          value={newNote}
          onChange={(event) => setNewNote(event.target.value)}
        />
        <button type='submit'>add</button>
      </form>
      // highlight-end
      <ul>
        {notes.map(note =>
          <li key={note.id}>{note.content}</li>
        )}
      </ul>
    </div>
  )
}
```

<!-- It just works! When we hover over the *event.target.value*, we see that it is indeed a string, just what is the expected parameter of the *setNewNote*:-->
当我们悬停在*event.target.value*上，我们看到它确实是一个字符串，正是*setNewNote*所期望的参数，它就是这么简单！

![vscode showing variable is a string](../../images/9/67new.png)

<!-- So we still need the event handler for adding the new note. Let us try the following:-->
所以我们仍然需要事件处理器来添加新笔记。让我们尝试一下：

```js
const App = () => {
  // ...

   // highlight-start
  const noteCreation = (event) => {
    event.preventDefault()
    // ...
  };
   // highlight-end

  return (
    <div>
      <form onSubmit={noteCreation}> // highlight-line
        <input
          value={newNote}
          onChange={(event) => setNewNote(event.target.value)}
        />
        <button type='submit'>add</button>
      </form>
      // ...
    </div>
  )
}
```

<!-- It does not quite work, there is an Eslint error complaining about implicit any:-->
它不太好用，有一个 ESLint 错误抱怨隐式的 any:

![vscode error event implicitly has any type](../../images/9/68new.png)

<!-- TypeScript compiler has now no clue what is the type of the parameter, so that is why the type is the infamous implicit any that we want to [avoid](/en/part9/first_steps_with_type_script#the-horrors-of-any) at all costs. The React TypeScript cheatsheet comes again to rescue, the chapter about-->
[props](/en/part9/first_steps_with_type_script#props) tells us that the type of the props parameter is an object with some fields.

TypeScript 编译器现在不知道参数的类型是什么，所以这就是为什么我们想要[避免](/en/part9/first_steps_with_type_script#the-horrors-of-any)的那种恶名昭彰的隐式 any 类型。React TypeScript 小抄又派上用场了，关于[props](/en/part9/first_steps_with_type_script#props)的章节告诉我们，props 参数的类型是一个带有一些字段的对象。
<!-- [forms and events](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/forms_and_events) reveals that the right type of event handler is *React.SyntheticEvent*.-->
[表单和事件](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/forms_and_events) 揭示了正确类型的事件处理程序是*React.SyntheticEvent*。

<!-- The code becomes-->
more complex

代码变得更加复杂了

```js
interface Note {
  id: number,
  content: string
}

const App = () => {
  const [notes, setNotes] = useState<Note[]>([]);
  const [newNote, setNewNote] = useState('');

// highlight-start
  const noteCreation = (event: React.SyntheticEvent) => {
    event.preventDefault()
    const noteToAdd = {
      content: newNote,
      id: notes.length + 1
    }
    setNotes(notes.concat(noteToAdd));

    setNewNote('')
  };
// highlight-end

  return (
    <div>
      <form onSubmit={noteCreation}>
        <input value={newNote} onChange={(event) => setNewNote(event.target.value)} />
        <button type='submit'>add</button>
      </form>
      <ul>
        {notes.map(note =>
          <li key={note.id}>{note.content}</li>
        )}
      </ul>
    </div>
  )
}
```

<!-- And that's it, our app is ready and perfectly typed!-->
而且就是这样，我们的应用程序已经准备好了，而且完美地打字！

### Communicating with the server

<!-- Let us modify the app so that the notes are saved in a JSON server backend in url <http://localhost:3001/notes>-->
让我们修改应用程序，以便将笔记保存在JSON服务器后端中的URL <http://localhost:3001/notes> 中。

<!-- As usual, we shall use Axios and the useEffect hook to fetch the initial state from the server.-->
通常，我们将使用Axios和useEffect钩子从服务器获取初始状态。

<!-- Let us try the following-->
让我们试试下面的

```js
const App = () => {
  // ...
  useEffect(() => {
    axios.get('http://localhost:3001/notes').then(response => {
      console.log(response.data);
    })
  }, [])
  // ...
}


```

<!-- When we hover over the *response.data* we see that is has the type *any*-->
当我们悬停在 *response.data* 上时，我们可以看到它的类型是 *any*。

![vscode response.data showing the any type](../../images/9/69new.png)

<!-- To set the data to the state with function *setNotes* we must type it properly.-->
使用函数 *setNotes* 将数据设置到状态时，我们必须正确地输入它。

<!-- With a little [help from internet](https://upmostly.com/typescript/how-to-use-axios-in-your-typescript-apps), we find a clever trick:-->
通过[网络的帮助](https://upmostly.com/typescript/how-to-use-axios-in-your-typescript-apps)，我们发现了一个聪明的技巧：

```js
  useEffect(() => {
    axios.get<Note[]>('http://localhost:3001/notes').then(response => { // highlight-line
      console.log(response.data);
    })
  }, [])
```

<!-- When we hover over the response.data we see that it has the correct type:-->
当我们悬停在response.data上时，我们发现它具有正确的类型：

![vscode showing response.data has Note array type](../../images/9/70new.png)

<!-- We can now set the data in the state *notes* to get the code working:-->
我们现在可以将数据设置在状态 *notes* 中以获得代码的正常运行：

```js
  useEffect(() => {
    axios.get<Note[]>('http://localhost:3001/notes').then(response => {
      setNotes(response.data) // highlight-line
    })
  }, [])
```

<!-- So just like with *useState*, we gave a type  parameter to *axios.get* to instruct it how the typing should be done. Just like *useState* also *axios.get* is a [generic function](https://www.typescriptlang.org/docs/handbook/2/generics.html#working-with-generic-type-variables). Unlike some generic functions, the type parameter of *axios.get* has a default value *any* so, if the function is used without defining the type parameter, the type of the response data will be any.-->
所以就像*useState*一样，我们给了一个型别参数给*axios.get*来指示它如何完成类型定义。就像*useState*一样，*axios.get*也是一个[泛型函数](https://www.typescriptlang.org/docs/handbook/2/generics.html#working-with-generic-type-variables)。不像某些泛型函数，*axios.get*的型别参数有一个默认值*any*，所以如果函数在不定义型别参数的情况下使用，响应数据的类型将是任何类型。

<!-- The code works, compiler and Eslint are happy and remain quiet. However, giving a type parameter to *axios.get* is a potentially dangerous thing to do. The response body can contain data in an arbitrary form, and when giving a type parameter we are essentially just telling to TypeScript compiler to trust us that the data has type *Note[]*.-->
代码运行正常，编译器和Eslint都很满意并保持安静。但是，给*axios.get*提供类型参数可能是一件危险的事情。响应体可以以任意形式包含数据，而当我们提供类型参数时，我们本质上只是告诉TypeScript编译器相信我们，数据具有类型*Note[]*。

<!-- So our code is essentially as safe as it would be if a [type assertion](/en/part9/first_steps_with_type_script#type-assertion) would be used:-->
所以我们的代码本质上与使用[类型断言](/zh-cn/part9/first_steps_with_type_script#type-assertion)一样安全：

```js
  useEffect(() => {
    axios.get('http://localhost:3001/notes').then(response => {
      // response.body is of type any
      setNotes(response.data as Note[]) // highlight-line
    })
  }, [])
```

<!-- Since the TypeScript types do not even exist in runtime, our code does not give us any "safety" against situations where the request body contains data in a wrong form.-->
因为TypeScript类型甚至在运行时都不存在，我们的代码不能为我们提供任何“安全性”来防止请求体包含错误形式的数据的情况。

<!-- Giving type variable to *axios.get* might be ok if we are <i>absolutely sure</i> that the backend behaves correctly and returns always the data in correct form. If we want to build a robust system we should prepare for surprises and parse the response data in the frontend similarly that we did [in the previous section](/en/part9/typing_an_express_app#proofing-requests) for the requests to the backend.-->
如果我们<i>绝对确定</i>后端行为正确，并且总是以正确的形式返回数据，那么给*axios.get*提供类型变量可能是可以的。如果我们想要构建一个强大的系统，我们应该为意外情况做好准备，并在前端与我们[在上一节](/en/part9/typing_an_express_app#proofing-requests)对后端请求所做的相似地解析响应数据。

<!-- Let us now wrap up our app by implementing the new note addition:-->
让我们现在通过实现新的笔记添加来结束我们的应用程序：

```js
  const noteCreation = (event: React.SyntheticEvent) => {
    event.preventDefault()
    // highlight-start
    axios.post<Note>('http://localhost:3001/notes', { content: newNote })
      .then(response => {
        setNotes(notes.concat(response.data))
      })
    // highlight-end

    setNewNote('')
  };
```

<!-- We are again giving *axios.post* a type parameter. We know that the server response is added note so the proper type parameter is *Note*.-->
我们再次给*axios.post*一个类型参数。我们知道服务器响应添加了注释，因此适当的类型参数是*Note*。

<!-- Let us clean up the code a bit. For the type definitions, we create a file *types.ts* with the following content:-->
让我们把代码收拾一下。对于类型定义，我们创建一个名为*types.ts*的文件，内容如下：

```js
export interface Note {
  id: number,
  content: string
}

export type NewNote = Omit<Note, 'id'>
```

<!-- We have added a new type for a <i>new note</i>, one that does not yet have the *id* field assigned.-->
我们为一个<i>新笔记</i>增加了一种新类型，它尚未指定*id*字段。

<!-- The code that communicates with the backend is also extracted to a module in the file *noteService.tsx*-->
在文件*noteService.tsx*中把与后端通信的代码也抽取到一个模块中。

```js
import axios from 'axios';
import { Note, NewNote } from "./types";

const baseUrl = 'http://localhost:3001/notes'

export const getAllNotes = () => {
  return axios
    .get<Note[]>(baseUrl)
    .then(response => response.data)
}

export const createNote = (object: NewNote) => {
  return axios
    .post<Note>(baseUrl, object)
    .then(response => response.data)
}
```

<!-- The component *App* is now much cleaner:-->
组件*App*现在更加干净了：

```js
import { useState, useEffect } from "react";
import { Note } from "./types"; // highlight-line
import { getAllNotes, createNote } from './noteService'; // highlight-line

const App = () => {
  const [notes, setNotes] = useState<Note[]>([]);
  const [newNote, setNewNote] = useState('');

  useEffect(() => {
    // highlight-start
    getAllNotes().then(data => {
      setNotes(data)
    })
    // highlight-end
  }, [])

  const noteCreation = (event: React.SyntheticEvent) => {
    event.preventDefault()
    // highlight-start
    createNote({ content: newNote }).then(data => {
      setNotes(notes.concat(data))
    })
    // highlight-end

    setNewNote('')
  };

  return (
    // ...
  )
}
```

<!-- The app is now nicely typed and ready for further development!-->
应用程序现在已经完美地打字，准备好进一步开发了！

<!-- The code of the typed notes can be found [here](https://github.com/fullstack-hy2020/typed-notes).-->
代码可以在[这里](https://github.com/fullstack-hy2020/typed-notes)找到。

### A note about defining object types

<!-- We have used [interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#interfaces) to define object types, e.g. diary entries, in the previous section-->
.

我们在前一节中使用[接口](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#interfaces)来定义对象类型，例如日记条目。

```js
interface DiaryEntry {
  id: number;
  date: string;
  weather: Weather;
  visibility: Visibility;
  comment?: string;
}
```

<!-- and in the course part of this section-->
,

在这一部分的课程部分，

```js
interface CoursePartBase {
  name: string;
  exerciseCount: number;
}
```

<!-- We actually could have had the same effect by using a [type alias](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases)-->
我们实际上可以通过使用[类型别名](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases)来达到同样的效果。

```js
type DiaryEntry = {
  id: number;
  date: string;
  weather: Weather;
  visibility: Visibility;
  comment?: string;
}
```

<!-- In most cases, you can use either *type* or *interface*, whichever syntax you prefer. However, there are a few things to keep in mind.-->
在大多数情况下，您可以使用*类型*或*界面*，无论您更喜欢哪种语法。但是，需要牢记几件事。
<!-- For example, if you define multiple interfaces with the same name, they will result in a merged interface, whereas if you try to define multiple types with the same name, it will result in an error stating that a type with the same name is already declared.-->
例如，如果你定义多个具有相同名称的接口，它们将会导致合并的接口，而如果你尝试定义多个具有相同名称的类型，它将会导致一个错误，表明已经声明了一个具有相同名称的类型。

<!-- TypeScript documentation [recommends using interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces) in most cases.-->
 TypeScript文档[建议在大多数情况下使用接口](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)。

### Working with an existing codebase

<!-- When diving into an existing codebase for the first time, it is good to get an overall view of the conventions and structure of the project. You can start your research by reading the <i>README.md</i> in the root of the repository. Usually, the README contains a brief description of the application and the requirements for using it, as well as how to start it for development.-->
当第一次潜入一个现有的代码库时，最好能对项目的惯例和结构有一个整体的认识。你可以通过阅读版本库根目录下的<i>README.md</i>开始你的研究。通常，README包含对应用的简要描述和使用要求，以及如何启动它进行开发。
<!-- If the README is not available or someone has "saved time" and left it as a stub, you can take a peek at the <i>package.json</i>.-->
 如果README不可用，或者有人 "节省时间"，把它作为一个存根，你可以偷看一下<i>package.json</i>。
<!-- It is always a good idea to start the application and click around to verify you have a functional development environment.-->
启动应用并点击一下以验证你有一个功能性的开发环境总是一个好主意。

<!-- You can also browse the folder structure to get some insight into the application's functionality and/or the architecture used. These are not always clear, and the developers might have chosen a way to organize code that is not familiar to you. The [sample project](https://github.com/fullstack-hy2020/patientor) used in the rest of this part is organized, feature-wise. You can see what pages the application has, and some general components, e.g. modals and state. Keep in mind that the features may have-->
你也可以浏览文件夹结构，以了解该应用的功能和/或使用的架构。这些并不总是清晰的，开发者可能选择了一种你不熟悉的方式来组织代码。本章节其余部分使用的[示例项目](https://github.com/fullstack-hy2020/patientor)是按功能组织的。你可以看到应用有哪些页面，以及一些一般的组件，例如模态和状态。请记住，这些功能可能有
<!-- different scopes. For example, modals are visible UI-level components whereas the state is comparable to business logic and keeps the data organized under the hood for the rest of the app to use.-->
不同的范围。例如，模态是可见的UI级组件，而状态则与业务逻辑相当，并将数据组织在引擎盖下供应用的其他部分使用。

<!-- TypeScript provides you types which tell you what kind of data structures, functions, components and state to expect.  You can try to look for <i>types.ts</i> or something similar to get you started. VSCode is a big help and just highlighting variables and parameters can give you quite a lot of insight. All this naturally depends on how types are used in the project.-->
 TypeScript为你提供了类型，告诉你可以期待什么样的数据结构、函数、组件和状态。  你可以尝试寻找<i>types.ts</i>或类似的东西来让你开始。VSCode是一个很大的帮助，仅仅突出变量和参数就可以给你相当多的启示。所有这些自然取决于项目中是如何使用类型的。

<!-- If the project has unit, integration or end-to-end tests, reading those is most likely beneficial. Test cases are your most important tool when refactoring or creating new features to the application. You want to make sure not to break any existing features when hammering around the code. TypeScript can also give you guidance with argument and return types when changing the code.-->
 如果项目有单元、集成或端到端的测试，阅读这些测试很可能是有益的。当重构或创建应用的新功能时，测试案例是你最重要的工具。你要确保在敲打代码时不破坏任何现有功能。在改变代码时，TypeScript也可以在参数和返回类型方面给你指导。

<!-- Do remember that reading code is a skill in itself, and don't worry if you don't understand the code on your first readthrough.  Code may have a lot of corner cases, and pieces of logic may have been added here and there throughout its development cycle. It is hard to imagine what kind of troubles the previous developer has been wrestling with. Think of it all like [growth rings in trees](https://en.wikipedia.org/wiki/Dendrochronology#Growth_rings). Understanding all of it requires digging deep into the code and business domain requirements. The more code you read, the better you're going to be at it. You will read more code than you're going to produce.-->
 请记住，阅读代码本身就是一种技能，如果你在第一次阅读时不理解代码，也不要担心。  代码可能有很多角落的情况，而且在整个开发周期中，可能有一些逻辑片段被添加到这里或那里。很难想象以前的开发者会遇到什么样的麻烦。把这一切想象成[树木的生长环](https://en.wikipedia.org/wiki/Dendrochronology#Growth_rings)。了解这一切需要深入挖掘代码和业务领域的需求。你读的代码越多，你就越能做到这一点。你会读到比你要生产的更多的代码。

### Patientor frontend

<!-- It's time to get our hands dirty finalizing the frontend for the backend we built in [exercises 9.8.-9.13](/en/part9/typing_the_express_app).-->
 是时候动手为我们在[练习9.8.-9.13](/en/part9/typing_the_express_app)中建立的后端完成前端了。

<!-- Before diving into the code, let us start both the frontend and the backend.-->
 在深入研究代码之前，让我们同时启动前端和后端。

<!-- If all goes well, you should see a patient listing page. It fetches a list of patients from our backend, and renders it to the screen as a simple table. There is also a button for creating new patients to the backend. As we are using mock data instead of a database, the data will not persist - closing the backend will delete all the data we have added. UI design has clearly not been a strong point of the creators, so let's disregard the UI for now.-->
 如果一切顺利，你应该看到一个病人列表页面。它从我们的后端获取一个病人列表，并以一个简单的表格形式渲染在屏幕上。还有一个按钮用于在后端创建新的病人。由于我们使用的是模拟数据而不是数据库，所以数据不会持续存在--关闭后端将删除我们添加的所有数据。UI设计显然不是创建者的强项，所以我们现在先不考虑UI。

<!-- After verifying that everything works, we can start studying the code. All the interesting stuff resides in the <i>src</i> folder. For your convenience, there is already a <i>types.ts</i> file for basic types used in the app, which you will have to extend or refactor in the exercises.-->
 在验证了一切正常后，我们可以开始研究代码。所有有趣的东西都在<i>src</i>文件夹中。为了你的方便，已经有一个<i>types.ts</i>文件用于应用中使用的基本类型，你将不得不在练习中扩展或重构它。

<!-- In principle, we could use the same types for both backend and frontend, but usually the frontend has different data structures and use cases for the data, which causes the types to be different.-->
 原则上，我们可以在后端和前端使用相同的类型，但通常前端有不同的数据结构和数据用例，这导致类型不同。
<!-- For example, the frontend has a state, and may want to keep data in objects or maps whereas the backend uses an array. The frontend might also not need all the fields of a data object saved in the backend, and it may need to add some new fields to use for rendering.-->
 例如，前端有一个状态，可能想把数据保存在对象或地图中，而后端使用数组。前端也可能不需要保存在后端的数据对象的所有字段，它可能需要添加一些新的字段来用于渲染。

<!-- The folder structure looks as follows:-->
 文件夹结构看起来如下。

![](../../images/9/34a.png)

<!-- As you would expect, there are currently two main components: <i>AddPatientModal</i> and <i>PatientListPage</i>. The <i>state</i> folder contains state handling for the frontend.-->
 如你所料，目前有两个主要组件。<i>AddPatientModal</i>和<i>PatientListPage</i>。<i>state</i>文件夹包含前端的状态处理。
<!-- The main functionality of the code in the <i>state</i> folder is to keep our data in one place and offer simple actions to alter the state of our app.-->
 <i>state</i>文件夹中的代码的主要功能是将我们的数据保存在一个地方，并提供简单的操作来改变我们应用的状态。
### State handling

<!-- Let's study the state handling a bit closer as a lot of stuff seems to be happening under the hood and it differs a bit from the methods used in the course so far.-->
 让我们更仔细地研究一下状态处理，因为很多东西似乎是在引擎盖下发生的，它与迄今为止课程中使用的方法有一些不同。

<!-- The state management is built using the React Hooks [useContext](https://reactjs.org/docs/hooks-reference.html#usecontext) and [useReducer](https://reactjs.org/docs/hooks-reference.html#usereducer). This is quite a good setup because we know the app will be rather small and we don't want to use <i>redux</i> or other similar libraries for the state management.-->
 状态管理是使用React Hooks [useContext](https://reactjs.org/docs/hooks-reference.html#usecontext) 和 [useReducer](https://reactjs.org/docs/hooks-reference.html#usereducer)建立的。这是一个很好的设置，因为我们知道这个应用将是相当小的，我们不想使用<i>redux</i>或其他类似的库来进行状态管理。
<!-- There are a lot of good material, for example  [this article](https://medium.com/@seantheurgel/react-hooks-as-state-management-usecontext-useeffect-usereducer-a75472a862fe), about this approach to state management.-->
 有很多好的材料，例如[这篇文章](https://medium.com/@seantheurgel/react-hooks-as-state-management-usecontext-useeffect-usereducer-a75472a862fe)，关于这种状态管理的方法。

<!-- The approach taken in this app uses the React [context](https://reactjs.org/docs/context.html) that, according to its documentation:-->
 在这个应用中采取的方法使用了React [context](https://reactjs.org/docs/context.html)，根据其文档。

<!-- > <i>... is designed to share data that can be considered "global" for a tree of React components, such as the current authenticated user, theme, or preferred language.</i>-->
 > <i>......被设计用来共享那些可以被认为是React组件树的 "全局 "数据，例如当前的认证用户、主题或首选语言。</i>

<!-- In our case, the "global", shared data is the application state <i>and</i> the dispatch function that is used to make changes to data. In many ways our code works much like the Redux-based state management we used in [part 6](/en/part6), but is more lightweight since it does not require the use of any external libraries. This part assumes that you are at least familiar with the way Redux works, e.g. you should have covered at least [the first section](/en/part6/flux_architecture_and_redux) of part 6.-->
 在我们的例子中，"全局"、共享的数据是应用状态<i>和</i>调度函数，用于对数据进行更改。在许多方面，我们的代码很像我们在[第6章节](/en/part6)中使用的基于Redux的状态管理，但由于它不需要使用任何外部库，所以更加轻量级。这一部分假设你至少熟悉Redux的工作方式，例如，你至少应该涵盖第六章节的[第一节](/en/part6/flux_architecture_and_redux)。

<!-- The [context](https://reactjs.org/docs/context.html) of our application has a tuple containing the app state and the dispatcher for changing the state.-->
 我们应用的[上下文](https://reactjs.org/docs/context.html)有一个元组，包含应用的状态和改变状态的调度器。
<!-- The application state is typed as follows:-->
 应用状态的类型如下。

```js
export type State = {
  patients: { [id: string]: Patient };
};
```

<!-- The state is an object with one key, <i>patients</i>, which has a [dictionary](https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html) or simply put an object with string keys and with a <i>Patient</i> objects as values. The index can only be  a <i>string</i> or a <i>number</i> as you can access the object values using those. This enforces that the state conforms to the form we want, and prevents developers from misusing the state.-->
 状态是一个有一个键的对象，<i>patients</i>，它有一个[字典](https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html)，或者简单说是一个有字符串键的对象，有一个<i>Patient</i>对象作为值。索引只能是一个<i>字符串</i>或一个<i>数字</i>，因为你可以用这些来访问对象的值。这就强制要求状态符合我们想要的形式，并防止开发者误用状态。

<!-- But be aware of one thing! When a type is declared like the type for <i>patients</i>, TypeScript does not actually have any way of knowing if the key you are trying to access actually exists or not. So if we were to try to access a patient by a non-existing id, the compiler would think that the returned value is of type <i>Patient</i> and no error would be thrown when trying to access its properties:-->
 但是要注意一件事!当一个类型被声明为<i>patients</i>的类型时，TypeScript实际上没有办法知道你试图访问的键是否真的存在。因此，如果我们试图通过一个不存在的id来访问一个病人，编译器会认为返回的值是<i>Patient</i>类型，并且在试图访问其属性时不会抛出错误。

```js
const myPatient = state.patients['non-existing-id'];
console.log(myPatient.name); // no error, TypeScript believes that myPatient is of type Patient
```

<!-- To fix this, we could define the type for patient values to be a union of <i>Patient</i> and <i>undefined</i> in the following way:-->
 为了解决这个问题，我们可以将病人值的类型定义为<i>Patient</i>和<i>undefined</i>的联合，方法如下。

```js
export type State = {
  patients: { [id: string]: Patient | undefined };
};
```

<!-- That would cause the compiler to give the following warning:-->
 这将导致编译器给出以下警告。

```js
const myPatient = state.patients['non-existing-id'];
console.log(myPatient.name); // error, Object is possibly 'undefined'
```

<!-- This type of additional type security is always good to implement if you e.g. use data from external sources or use the value of a user input to access data in your code. But if you are sure that you only handle data that actually exists, then there is no one stopping you from using the first presented solution.-->
 如果你使用来自外部的数据或使用用户输入的值来访问代码中的数据，这种额外的类型安全总是很好实现的。但如果你确信你只处理实际存在的数据，那么没有人阻止你使用第一个提出的解决方案。

<!-- Even though we are not using them in this course part, it is good to mention that a more type-strict way would be to use [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) objects, to which you can declare a type for both the key and the content. The Map's accessor function <i>get()</i> always returns a union of the declared value type and undefined, so TypeScript automatically requires you to perform validity checks on data retrieved from a map:-->
 尽管我们在这个课程部分没有使用它们，但值得一提的是，一个更严格的类型方式是使用[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)对象，你可以为它的键和内容都声明一个类型。Map's accessor函数<i>get()</i>总是返回声明的值类型和未定义的联合体，所以TypeScript自动要求你对从Map检索的数据执行有效性检查。

```js
interface State {
  patients: Map<string, Patient>;
}
...
const myPatient = state.patients.get('non-existing-id'); // type for myPatient is now Patient | undefined
console.log(myPatient.name); // error, Object is possibly 'undefined'

console.log(myPatient?.name); // valid code, but will log 'undefined'
```

<!-- Just like with redux, all state manipulation is done by a reducer. It is defined in the file <i>reducer.ts</i> along with the type <i>Action</i>, which looks as follows:-->
 就像redux一样，所有的状态操作都是由一个reducer完成的。它在文件<i>reducer.ts</i>中与<i>Action</i>类型一起定义，看起来如下。

```js
export type Action =
  | {
      type: "SET_PATIENT_LIST";
      payload: Patient[];
    }
  | {
      type: "ADD_PATIENT";
      payload: Patient;
    };
```

<!-- The reducer looks quite similar to the ones we wrote in [part 6](/en/part6) before we started to use the Redux Toolkit. It changes the state for each type of action:-->
 减速器看起来和我们在开始使用Redux工具包之前在[第6章节](/en/part6)写的那些很相似。它为每种类型的动作改变状态。

```js
export const reducer = (state: State, action: Action): State => {
  switch (action.type) {
    case "SET_PATIENT_LIST":
      return {
        ...state,
        patients: {
          ...action.payload.reduce(
            (memo, patient) => ({ ...memo, [patient.id]: patient }),
            {}
          ),
          ...state.patients
        }
      };
    case "ADD_PATIENT":
      return {
        ...state,
        patients: {
          ...state.patients,
          [action.payload.id]: action.payload
        }
      };
    default:
      return state;
  }
};
```

<!-- The main difference is  that the state is now a dictionary (or an object), instead of the array that we used in [part 6](/en/part6).-->
 主要区别在于，现在的状态是一个字典（或一个对象），而不是我们在[第六章节](/en/part6)中使用的数组。

<!-- There are a lot of things happening in the file <i>state.ts</i>, which takes care of setting up the context.-->
 在文件<i>state.ts</i>中发生了很多事情，它负责设置上下文。
<!-- The main ingredient is the [useReducer](https://reactjs.org/docs/hooks-reference.html#usereducer) hook-->
主要内容是[useReducer](https://reactjs.org/docs/hooks-reference.html#usereducer)钩子
<!-- used to create the state and the dispatch function, and pass them on to the [context provider](https://reactjs.org/docs/context.html#contextprovider):-->
用来创建状态和调度函数，并把它们传递给[上下文提供者](https://reactjs.org/docs/context.html#contextprovider)。

```js
export const StateProvider = ({
  reducer,
  children
}: StateProviderProps) => {
  const [state, dispatch] = useReducer(reducer, initialState); // highlight-line
  return (
    <StateContext.Provider value={[state, dispatch]}>  // highlight-line
      {children}
    </StateContext.Provider>
  );
};
```

<!-- The provider makes the <i>state</i> and the <i>dispatch</i> functions available in all of the components, thanks to the setup in <i>index.ts</i>:-->
 由于<i>index.ts</i>中的设置，提供者使<i>state</i>和<i>dispatch</i>函数在所有组件中可用。

```js
import { reducer, StateProvider } from "./state";

ReactDOM.render(
  <StateProvider reducer={reducer}>
    <App />
  </StateProvider>,
  document.getElementById('root')
);
```

<!-- It also defines the <i>useStateValue</i> hook:-->
 它还定义了<i>useStateValue</i>钩。

```js
export const useStateValue = () => useContext(StateContext);
```

<!-- and the components that need to access the state or dispatcher use it to get hold of those:-->
 需要访问状态或调度器的组件使用它来获得这些。

```js
import { useStateValue } from "../state";

// ...

const PatientListPage = () => {
  const [{ patients }, dispatch] = useStateValue();
  // ...
}
```

<!-- Don't worry if this seems confusing; it will be until you have studied the [context's documentation](https://reactjs.org/docs/context.html) and its use in [state management](https://medium.com/@seantheurgel/react-hooks-as-state-management-usecontext-useeffect-usereducer-a75472a862fe). You do not need to understand all this completely to do the exercises!-->
 如果这看起来很混乱，请不要担心；在你研究了[context's documentation](https://reactjs.org/docs/context.html)和它在[state management](https://medium.com/@seantheurgel/react-hooks-as-state management-usecontext-useeffect-usereducer-a75472a862fe)中的使用之前，都是如此。你不需要完全理解这一切来做练习!

<!-- It is actually quite common that when you start working on an existing codebase, you do not understand 100% of what happens under the hood in the beginning. If the app has been properly structured (and it has a proper set of tests), you can trust that if you make careful modifications, the app still works despite the fact that you did not understand  all the internal mechanisms. Over time, you will get a grasp on the more unfamiliar parts, but it does not happen overnight when working with a large codebase.-->
 实际上，当你开始在一个现有的代码库上工作时，一开始你并不完全理解引擎盖下发生的事情，这是非常普遍的。如果应用的结构正确（并且有一套适当的测试），你可以相信，如果你仔细修改，尽管你不了解所有的内部机制，应用仍然可以工作。随着时间的推移，你会掌握更多不熟悉的部分，但在处理大型代码库时，这不会在一夜之间发生。

### Patient listing page

<!-- Let's go through the <i>PatientListPage/index.ts</i> as you can take inspiration from there to help you fetch data from the backend and update the application's state.-->
 我们来看看<i>PatientListPage/index.ts</i>，因为你可以从那里得到启发，帮助你从后端获取数据并更新应用的状态。
<i>PatientListPage</i> uses our custom hook to inject the state, and the dispatcher for updating it.
<!-- When we list the patients, we only need to destructure the <i>patients</i> property from the state:-->
当我们列出病人时，我们只需要从状态中解除<i>patients</i>属性的结构。

```js
import { useStateValue } from "../state";

const PatientListPage = () => {
  const [{ patients }, dispatch] = useStateValue();
  // ...
}
```

<!-- We also use the app state created with the <i>useState</i> hook for managing modal visibility and form error handling:-->
 我们也使用用<i>useState</i>钩子创建的应用状态来管理模式的可见性和表单错误处理。


```js
const [modalOpen, setModalOpen] = React.useState<boolean>(false);
const [error, setError] = React.useState<string | undefined>();
```

<!-- We give the <i>useState</i> hook a type parameter, which is then applied to the actual state. So <i>modalOpen</i> is a <i>boolean</i> and <i>error</i> has the type <i>string | undefined</i>.-->
 我们给<i>useState</i>钩子一个类型参数，然后将其应用于实际状态。所以<i>modalOpen</i>是一个<i>boolean</i>，<i>error</i>的类型是<i>string | undefined</i>。
<!-- Both set functions returned by the <i>useState</i> hook are functions that accept only arguments according to the type parameter given, eg. the exact type for <i>setModalOpen</i> function is <i>React.Dispatch<React.SetStateAction&lt;boolean&gt;></i>.-->
由<i>useState</i>钩子返回的两个设置函数都是根据给定的类型参数只接受参数的函数，例如，<i>setModalOpen</i>函数的确切类型是<i>React.Dispatch<React.SetStateAction&lt;boolean&gt;</i>。

<!-- We also have the <i>openModal</i> and <i>closeModal</i> helper functions for better readability and convenience:-->
 我们还有<i>openModal</i>和<i>closeModal</i>辅助函数，以提高可读性和便利性。

```js
const openModal = (): void => setModalOpen(true);

const closeModal = (): void => {
  setModalOpen(false);
  setError(undefined);
};
```

<!-- The frontend's types are based on what you have created when developing the backend in the previous part.-->
 前端的类型是基于你在前一部分开发后端时创建的。

<!-- When the component <i>App</i> mounts, it fetches patients from the backend using [Axios](https://github.com/axios/axios). Note how we are giving the <i>axios.get</i> function a type parameter to describe the type of the response data:-->
 当组件<i>App</i>挂载时，它使用[Axios](https://github.com/axios/axios)从后端获取病人。注意我们是如何给<i>axios.get</i>函数一个类型参数来描述响应数据的类型。

````js
React.useEffect(() => {
  axios.get<void>(`${apiBaseUrl}/ping`);

  const fetchPatientList = async () => {
    try {
      const { data: patients } = await axios.get<Patient[]>(
        `${apiBaseUrl}/patients`
      );
      dispatch({ type: "SET_PATIENT_LIST", payload: patients });
    } catch (error: unknown) {
      let errorMessage = 'Something went wrong.'
      if(axios.isAxiosError(error) && error.response) {
        errorMessage += ' Error: ' + error.response.data.message;
      }
      console.error(errorMessage);
    }
  };
  fetchPatientList();
}, [dispatch]);
````

<!-- **A word of warning!** Passing a type parameter to Axios will not validate any data. It is quite dangerous especially if you are using external APIs. You can create custom validation functions which take in the whole payload and return the correct type, or you can use a type guard. Both are valid options. There are also many libraries that provide validation through a different kind of schemas, for example [io-ts](https://gcanti.github.io/io-ts/). For simplicity's sake, we will continue to trust our own work and trust that we will get data of the correct form from the backend.-->
 **一个警告！**向Axios传递一个类型参数不会验证任何数据。这是相当危险的，特别是当你使用外部API的时候。你可以创建自定义的验证函数，接收整个有效载荷并返回正确的类型，或者你可以使用类型保护。两者都是有效的选择。也有很多库通过不同的模式提供验证，例如[io-ts](https://gcanti.github.io/io-ts/)。为了简单起见，我们将继续相信我们自己的工作，并相信我们将从后端获得正确形式的数据。

<!-- As our app is quite small, we will update the state by simply calling the <i>dispatch</i> function provided to us by the <i>useStateValue</i> hook.-->
 由于我们的应用相当小，我们将通过简单地调用<i>dispatch</i>函数来更新状态，该函数由<i>useStateValue</i>钩提供给我们。
<!-- The compiler helps by making sure that we dispatch actions according to our <i>Action</i> type with a predefined type string and payload:-->
编译器通过确保我们根据我们的<i>Action</i>类型和预定义的类型字符串和有效载荷来调度行动。

```js
dispatch({ type: "SET_PATIENT_LIST", payload: patients });
```

</div>

<div class="tasks">

### Exercises 9.16-9.19

<!-- Let us now build a frontend for the Ilari's flight diaries that was developed in [the previous section](/en/part9/typing_an_express_app). The source code of the backend can be found in [this GitHub repository](https://github.com/fullstack-hy2020/flight-diary).-->
让我们现在为在[上一节](/en/part9/typing_an_express_app)中开发的Ilari的飞行日记构建一个前端。后端的源代码可以在[这个GitHub存储库](https://github.com/fullstack-hy2020/flight-diary)中找到。

#### Exercise 9.16

<!-- Create a TypeScript React app with similar configurations as the apps of this section. Fetch the diaries from the backend and render those to screen. Do all the required typing and ensure that there are no Eslint errors.-->
在本节的应用程序的类似配置下创建一个TypeScript React应用程序。 从后端获取日记并将其渲染到屏幕上。 执行所有必要的键入，并确保没有Eslint错误。

<!-- Remember to keep the network tab open. It might give you a valuable hint...-->
记得保持网络选项开启，它可能会给你一个宝贵的提示...

<!-- You can decide how the diary entries are rendered. If you wish, you may take inspiration form the figure below. Note that the backend API does not return the diary comments, you may modify it to return also those on a GET request.-->
你可以决定日记条目的呈现方式。如果你愿意，可以从下面的图中获取灵感。请注意，后端API不会返回日记评论，你可以修改它以在 GET 请求中也返回这些评论。

#### Exercise 9.17

<!-- Make it possible to add new diary entries from the frontend. In this exercise you may skip all validations and assume that the user just enters the data in a correct form.-->
使得从前端添加新日记条目成为可能。在这个练习中，你可以跳过所有验证，假设用户以正确的形式输入数据。

#### Exercise 9.18

<!-- Notify the user if the the creation of a diary entry fails in the backend, show also the reason for the failure.-->
如果后端创建日记条目失败，请通知用户，并显示失败的原因。

<!-- See eg. [this](https://dev.to/mdmostafizurrahaman/handle-axios-error-in-typescript-4mf9) how you can narrow the Axios error so that you can get hold of the error message.-->
参考[这个](https://dev.to/mdmostafizurrahaman/handle-axios-error-in-typescript-4mf9)，了解如何缩小 Axios 错误的范围，以便获取错误消息。

<!-- Your solution may look like this:-->
你的解决方案可能看起来像这样：

![browser showing error incorrect visibility best ever](../../images/9/71new.png)

#### Exercise 9.19

<!-- Addition of a diary entry is now very error prone since user can type anything to the input fields. The situation must be improved.-->
添加日记条目现在非常容易出错，因为用户可以在输入字段中输入任何内容。这种情况必须得到改善。

<!-- Modify the input form so that the date is set with a HTML [date](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/date) input element, and the weather and visibility are set with HTML [radio buttons](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio). We have already used radio buttons in [part 6](/en/part6/many_reducers#store-with-complex-state), that material may or may not be useful...-->
修改输入表单，使日期使用HTML [date](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/date) 输入元素设置，而天气和能见度使用HTML [radio buttons](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio) 设置。我们已经在[第6章节](/en/part6/many_reducers#store-with-complex-state) 中使用了单选按钮，这些材料可能有用也可能没用...

<!-- The Application also uses [React Router](https://reactrouter.com/en/main/start/tutorial)  to control which view is visible in the frontend. You might want to have a look at [part 7](/en/part7/react_router) if you don't yet have a grasp on how the router works.-->
 应用还使用[React Router](https://reactrouter.com/en/main/start/tutorial)来控制哪个视图在前端可见。如果你还没有掌握路由器的工作原理，你可能想看一下[第7章节](/en/part7/react_router)。

<!-- The result could look like this:-->
 结果可能是这样的。

![](../../images/9/39x.png)

<!-- Example uses [Material UI Icons](https://mui.com/components/material-icons/) to represent genders.-->
 示例使用[Material UI Icons](https://mui.com/components/material-icons/)来表示性别。

<!-- **Note** that in order to access the id in the url, you need to give [useParams](https://reactrouter.com/en/main/hooks/use-params) a proper type argument:-->
 **注意**，为了访问url中的id，你需要给[useParams](https://reactrouter.com/en/main/hooks/use-params)一个合适的类型参数。

```js
const { id } = useParams<{ id: string }>();
```

#### 9.18: patientor, step3

<!-- Currently, we create <i>action</i> objects wherever we dispatch actions, e.g. the <i>App</i> component has the following:-->
 目前，我们在分派动作的地方创建<i>action</i>对象，例如，<i>App</i>组件有以下内容。

```js
dispatch({
  type: "SET_PATIENT_LIST", payload: patientListFromApi
});
```

<!-- Define [action creator functions](/en/part6/flux_architecture_and_redux#action-creators) in the file <i>src/state/reducer.ts</i> and refactor the code to use them.-->
 在文件<i>src/state/reducer.ts</i>中定义[动作创建者函数](/en/part6/flux_architecture_and_redux#action-creators)并重构代码以使用它们。

<!-- For example, the <i>App</i> should become like the following:-->
 例如，<i>App</i>应该变成下面的样子。

```js
import { useStateValue, setPatientList } from "./state";

// ...

dispatch(setPatientList(patientListFromApi));
```


</div>

<div class="content">

### Full entries

<!-- In the [exercise 9.10.](/en/part9/typing_the_express_app#exercises-9-10-9-11) we implemented an endpoint for fetching information about various diagnoses, but we are still not using that endpoint at all.-->
 在[练习9.10.](/en/part9/typing_the_express_app#exercises-9-10-9-11)中，我们实现了一个用于获取各种诊断信息的端点，但我们仍然完全没有使用这个端点。
<!-- Since we now have a page for viewing a patient's information, it would be nice to expand our data a bit.-->
 既然我们现在有了一个查看病人信息的页面，那么把我们的数据扩展一下就好了。
<!-- Let's add an <i>Entry</i> field to our patient data so that a patient's data contains their medical entries, including possible diagnoses.-->
 让我们为我们的病人数据添加一个<i>Entry</i>字段，这样一个病人的数据就包含了他们的医疗条目，包括可能的诊断。

<!-- Let's ditch our old patient seed data from the backend and start using [this expanded format](https://github.com/fullstack-hy/misc/blob/master/patients.ts).-->
 让我们从后端抛弃旧的病人种子数据，开始使用[这个扩展格式](https://github.com/fullstack-hy/misc/blob/master/patients.ts)。

<!-- **Notice:** This time, the data is not in the .json format but instead in the .ts format. You should already have the complete <i>Gender</i> and <i>Patient</i> types implemented, so only correct the paths where they are imported from if needed.-->
 **注意：**这次，数据不是以.json格式，而是以.ts格式。你应该已经实现了完整的<i>Gender</i>和<i>Patient</i>类型，所以如果需要的话，只需纠正它们被导入的路径。

<!-- Let us now create a proper <i>Entry</i> type based on the data we have.-->
 现在让我们根据我们拥有的数据创建一个适当的<i>条目</i>类型。

<!-- If we take a closer look at the data, we can see that the entries are actually quite different from one another. For example, let's take a look at the first two entries:-->
 如果我们仔细看一下这些数据，我们可以看到这些条目实际上是非常不同的。例如，让我们看一下前两个条目。

```js
{
  id: 'd811e46d-70b3-4d90-b090-4535c7cf8fb1',
  date: '2015-01-02',
  type: 'Hospital',
  specialist: 'MD House',
  diagnosisCodes: ['S62.5'],
  description:
    "Healing time appr. 2 weeks. patient doesn't remember how he got the injury.",
  discharge: {
    date: '2015-01-16',
    criteria: 'Thumb has healed.',
  }
}
...
{
  id: 'fcd59fa6-c4b4-4fec-ac4d-df4fe1f85f62',
  date: '2019-08-05',
  type: 'OccupationalHealthcare',
  specialist: 'MD House',
  employerName: 'HyPD',
  diagnosisCodes: ['Z57.1', 'Z74.3', 'M51.2'],
  description:
    'Patient mistakenly found himself in a nuclear plant waste site without protection gear. Very minor radiation poisoning. ',
  sickLeave: {
    startDate: '2019-08-05',
    endDate: '2019-08-28'
  }
}
```

<!-- Immediately, we can see that while the first few fields are the same, the first entry has a <i>discharge</i> field and the second entry has <i>employerName</i> and <i>sickLeave</i> fields.-->
 随即，我们可以看到，虽然前几个字段是相同的，但第一个条目有一个<i>discharge</i>字段，第二个条目有<i>employerName</i>和<i>sickLeave</i>字段。
<!-- All the entries seem to have some fields in common, but some fields are entry-specific.-->
所有条目似乎都有一些共同的字段，但有些字段是条目特有的。

<!-- When looking at the <i>type</i>, we can see that there are actually three kinds of entries: <i>OccupationalHealthcare</i>, <i>Hospital</i> and <i>HealthCheck</i>.-->
当查看<i>类型</i>时，我们可以看到，实际上有三种条目。<i>职业保健</i>、<i>医院</i>和<i>健康检查</i>。
<!-- This indicates we need three separate types. Since they all have some fields in common, we might just want to create a base entry interface that we can extend with the different fields in each type.-->
这表明我们需要三种独立的类型。由于它们都有一些共同的字段，我们可能只想创建一个基本的条目接口，我们可以用每个类型中的不同字段来扩展。

<!-- When looking at the data, it seems that the fields <i>id</i>, <i>description</i>, <i>date</i> and <i>specialist</i> are something that can be found in each entry. On top of that, it seems that the <i>diagnosisCodes</i> is only found in one <i>OccupationalHealthCare</i> and one <i>Hospital</i> type entry. Since it is not always used even in those types of entries, it is safe to assume that the field is optional. We could consider adding it to the <i>HealthCheck</i> type as well,-->
 在看数据时，似乎字段<i>id</i>、<i>description</i>、<i>date</i>和<i>specialist</i>都是可以在每个条目中找到的东西。除此之外，<i>diagnosisCodes</i>似乎只出现在一个<i>OccupationalHealthCare</i>和一个<i>Hospital</i>类型的条目中。由于即使在这些类型的条目中也并不总是使用它，因此可以认为该字段是可选的。我们可以考虑在<i>HealthCheck</i>类型中也添加它。
<!-- since it might just not be used in these specific entries.-->
因为它可能只是在这些特定的条目中没有被使用。

<!-- So our <i>BaseEntry</i> from which each type could be extended from would be the following:-->
 所以我们的<i>BaseEntry</i>可以从每个类型中扩展出来，如下。

```js
interface BaseEntry {
  id: string;
  description: string;
  date: string;
  specialist: string;
  diagnosisCodes?: string[];
}
```

<!-- If we want to finetune it a bit further, since we already have a <i>Diagnosis</i> type defined in the backend, we might just want to refer to the code field of the <i>Diagnosis</i> type directly in case its type ever changes.-->
 如果我们想进一步调整，因为我们已经在后端定义了一个<i>诊断</i>类型，我们可能只想直接引用<i>诊断</i>类型的代码字段，以防其类型发生变化。
<!-- We can do that like so:-->
 我们可以这样做。

```js
interface BaseEntry {
  id: string;
  description: string;
  date: string;
  specialist: string;
  diagnosisCodes?: Array<Diagnosis['code']>;
}
```

<!-- As you might remember, <i>Array&lt;Type&gt;</i> is just an alternative way to say <i>Type[]</i>. In cases like this, it is just much clearer to use the array convention since the other option would be to define the type by saying <i>Diagnosis['code'][]</i> which starts to look a bit strange.-->
 正如你可能记得的，<i>Array&lt;Type&gt;</i>只是说<i>Type[]</i>的一种替代方式。在这样的情况下，使用数组约定更清楚，因为另一种选择是通过说<i>Diagnosis[''code'][]</i>来定义类型，这看起来有点奇怪。

<!-- Now that we have the <i>BaseEntry</i> defined, we can start creating the extended entry types we will actually be using. Let's start by creating the <i>HealthCheckEntry</i> type.-->
 现在我们已经定义了<i>BaseEntry</i>，我们可以开始创建我们将实际使用的扩展条目类型。让我们从创建<i>HealthCheckEntry</i>类型开始。

<!-- Entries of type <i>HealthCheck</i> contain the field <i>HealthCheckRating</i>, which is an integer from 0 to 3, zero meaning <i>Healthy</i> and 3 meaning <i>CriticalRisk</i>. This is a perfect case for an enum definition.-->
类型<i>HealthCheck</i>的条目包含字段<i>HealthCheckRating</i>，它是一个从0到3的整数，0表示<i>健康</i>，3表示<i>严重风险</i>。这是一个枚举定义的完美案例。
<!-- With these specifications we could write a <i>HealthCheckEntry</i> type definition like so:-->
 有了这些规范，我们可以这样写一个<i>HealthCheckEntry</i>类型定义。

```js
export enum HealthCheckRating {
  "Healthy" = 0,
  "LowRisk" = 1,
  "HighRisk" = 2,
  "CriticalRisk" = 3
}

interface HealthCheckEntry extends BaseEntry {
  type: "HealthCheck";
  healthCheckRating: HealthCheckRating;
}
```

<!-- Now we only need to create the <i>OccupationalHealthcareEntry</i> and <i>HospitalEntry</i> types so we can combine them in a union and export them as an Entry type like this:-->
 现在我们只需要创建<i>OccupationalHealthcareEntry</i>和<i>HospitalEntry</i>类型，这样我们就可以把它们结合在一个联盟中，并像这样把它们输出为Entry类型。

```js
export type Entry =
  | HospitalEntry
  | OccupationalHealthcareEntry
  | HealthCheckEntry;
```
<!-- An important point concerning unions is that, when you use them with _Omit_ to exclude a property, it works in a possibly unexpected way. Suppose we want to remove the _id_ from each _Entry_. We could think of using _Omit&lt;Entry, 'id'&gt;_, but [it wouldn't work as we might expect](https://github.com/microsoft/TypeScript/issues/42680). In fact, the resulting type would only contain the common properties, but not the ones they don't share. A possible workaround is to define a special Omit-like function to deal with such situations:-->
 关于联合的一个重要观点是，当你用它们和_Omit_来排除一个属性时，它的工作方式可能是意想不到的。假设我们想从每个_Entry_中删除_id_。我们可以考虑使用_Omit&lt;Entry, ''id'&gt;_，但是[它不会像我们期望的那样工作](https://github.com/microsoft/TypeScript/issues/42680)。事实上，产生的类型将只包含共同的属性，而不包含它们不共享的属性。一个可能的变通方法是定义一个特殊的类似于Omit的函数来处理这种情况。

```ts
// Define special omit for unions
type UnionOmit<T, K extends string | number | symbol> = T extends unknown ? Omit<T, K> : never;
// Define Entry without the 'id' property
type EntryWithoutId = UnionOmit<Entry, 'id'>;
```

</div>

<div class="tasks">

### Exercises 9.19.-9.22.

#### 9.19: patientor, step4

<!-- Define the types <i>OccupationalHealthcareEntry</i> and <i>HospitalEntry</i> so that those conform with the example data. Ensure that your backend returns the entries properly when you go to an individual patient's route:-->
 定义类型<i>OccupationalHealthcareEntry</i>和<i>HospitalEntry</i>，使其符合示例数据。确保你的后端在进入单个病人的路线时正确返回条目。

![](../../images/9/40.png)

<!-- Use types properly in the backend! For now, there is no need to do a proper validation for all the fields of the entries in the backend, it is enough e.g. to check that the field <i>type</i> has a correct value.-->
 在后端正确使用类型!现在，没有必要对后端的所有条目字段进行适当的验证，只需检查字段<i>类型</i>是否有正确的值即可。

#### 9.20: patientor, step5

<!-- Extend a patient's page in the frontend to list the <i>date</i>, <i>description</i> and <i>diagnose codes</i> of the patient's entries.-->
 在前端扩展一个病人的页面，列出病人条目的<i>日期</i>、<i>描述</i>和<i>诊断代码</i>。

<!-- You can use the same type definition for an <i>Entry</i> in the frontend. For these exercises, it is enough to just copy/paste the definitions from the backend to the frontend.-->
 你可以在前端的<i>条目</i>中使用相同的类型定义。对于这些练习，只需将定义从后端复制/粘贴到前端即可。

<!-- Your solution could look like this:-->
你的解决方案可能看起来像这样：

![](../../images/9/41.png)

#### 9.21: patientor, step6

<!-- Fetch and add diagnoses to the application state from the <i>/api/diagnoses</i> endpoint. Use the new diagnosis data to show the descriptions for patient's diagnosis codes:-->
 从<i>/api/diagnoses</i>端点获取并添加诊断到应用状态。使用新的诊断数据来显示病人的诊断代码的描述。

![](../../images/9/42.png)

#### 9.22: patientor, step7

<!-- Extend the entry listing in the patient's page to include the Entry's details with a new component that shows the rest of the information of the patient's entries distinguishing different types from each other.-->
 在病人的页面中扩展条目列表，包括条目的细节，用一个新的组件显示病人条目的其他信息，区分不同类型。

<!-- You could use eg. [Icons](https://mui.com/components/material-icons/) or some other [Material UI](https://mui.com/) component to get appropriate visuals for your listing.-->
你可以使用例如[图标](https://mui.com/components/material-icons/)或其他一些[材料用户界面](https://mui.com/)组件来为你的列表获得适当的视觉效果。

<!-- You should use a _switch case_-based rendering and <i>exhaustive type checking</i> so that no cases can be forgotten.-->
你应该使用一个基于切换案例的渲染和<i>详尽的类型检查</i>，这样就不会有案例被遗忘。

<!-- Like this:-->
 像这样。

![](../../images/9/35c.png)

<!-- The resulting entries in the listing <i>could</i> look something like this:-->
 列表中的结果条目<i>可以</i>看起来是这样的。

![](../../images/9/36x.png)

</div>

<div class="content">

### Add patient form

<!-- Form handling can sometimes be quite a nuisance in React. That's why we have decided to utilize the [Formik](https://jaredpalmer.com/formik/docs/overview) package for our app's add patient form. Here's a small intro from the Formik's documentation:-->
 在React中，表单处理有时是一个相当烦人的问题。这就是为什么我们决定利用[Formik](https://jaredpalmer.com/formik/docs/overview)包来处理我们应用的添加病人表单。下面是Formik文档中的一个小介绍。

<!-- > Formik is a small library that helps you with the 3 most annoying parts:-->
 > Formik是一个小库，它可以帮助你解决3个最烦人的部分。
<!-- >-->
 >
<!-- > - Getting values in and out of form state-->
 > - 在表单状态中获取数值和离开表单状态
<!-- > - Validation and error messages-->
 > - 验证和错误信息
<!-- > - Handling form submission-->
 > - 处理表单提交
<!-- >-->
 >
<!-- > By colocating all of the above in one place, Formik will keep things organized - making testing, refactoring, and reasoning about your forms a breeze.-->
 > 通过将上述所有内容集中在一个地方，Formik将保持事情的条理性--使测试、重构和推理你的表单变得轻而易举。

<!-- The code for the form can be found from <i>src/AddPatientModal/AddPatientForm.tsx</i> and some form field helpers can be found from <i>src/AddPatientModal/FormField.tsx</i>.-->
 表单的代码可以从<i>src/AddPatientModal/AddPatientForm.tsx</i>中找到，一些表单字段的帮助器可以从<i>src/AddPatientModal/FormField.tsx</i>中找到。

<!-- Looking at the top of the <i>AddPatientForm.tsx</i> you can see we have created a type for our form values, which we have simply called <i>PatientFormValues</i>. The type is a modified version of the <i>Patient</i> type with the <i>id</i> and <i>entries</i> properties omitted. We don't want the user to be able to submit those when creating a new patient. The <i>id</i> is created by the backend and <i>entries</i> can only be added for existing patients.-->
 看看<i>AddPatientForm.tsx</i>的顶部，你可以看到我们已经为我们的表单值创建了一个类型，我们简单地称之为<i>PatientFormValues</i>。这个类型是<i>Patient</i>类型的修改版，省略了<i>id</i>和<i>entries</i>属性。我们不希望用户在创建一个新病人时能够提交这些属性。<i>id</i>是由后端创建的，<i>entries</i>只能为现有病人添加。

```js
export type PatientFormValues = Omit<Patient, "id" | "entries">;
```

<!-- Next, we declare the props for our form component:-->
 接下来，我们为我们的表单组件声明prop。

```js
interface Props {
  onSubmit: (values: PatientFormValues) => void;
  onCancel: () => void;
}
```

<!-- As you can see, the component requires two props: <i>onSubmit</i> and <i>onCancel</i>.-->
 你可以看到，这个组件需要两个prop。<i>onSubmit</i>和<i>onCancel</i>。
<!-- Both are callback functions that return <i>void</i>. The <i>onSubmit</i> function should receive an-->
 这两个都是回调函数，返回<i>void</i>。<i>onSubmit</i>函数应该接收一个
<!-- object of type <i>PatientFormValues</i> as an argument, so that the callback can handle our form values.-->
类型为<i>PatientFormValues</i>的对象作为参数，这样回调就可以处理我们的表单值。

<!-- Looking at the <i>AddPatientForm</i> function component, you can see we have bound the <i>Props</i> as our component's props, and we destructure <i>onSubmit</i> and <i>onCancel</i> from those props.-->
 看一下<i>AddPatientForm</i>函数组件，你可以看到我们已经绑定了<i>Props</i>作为我们组件的props，并且我们从这些props中解构<i>onSubmit</i>和<i>onCancel</i>。

```js
export const AddPatientForm = ({ onSubmit, onCancel }: Props) => {
  // ...
}
```

<!-- Now before we continue, let's take a look at our form helpers in <i>FormField.tsx</i>.-->
 在我们继续之前，让我们看一下我们在<i>FormField.tsx</i>中的表单助手。
<!-- If you check what is exported from the file, you'll find the type <i>GenderOption</i> and the function components <i>SelectField</i> and <i>TextField</i>.-->
 如果你检查从文件中导出的内容，你会发现类型<i>GenderOption</i>和功能组件<i>SelectField</i>和<i>TextField</i>。

<!-- Let's take a closer look at <i>SelectField</i> and the types around it.-->
 让我们仔细看看<i>SelectField</i>和它周围的类型。
<!-- First, we create a generic type for each option object that contains a value and a label for that value. These are the kind of option objects we want to allow on our form in the select field.-->
 首先，我们为每个选项对象创建一个通用类型，它包含一个值和一个标签。这些是我们想在表单的选择字段中允许的选项对象的类型。
<!-- Since the only options we want to allow are different genders, we set that the <i>value</i> should be of type <i>Gender</i>.-->
 由于我们想允许的唯一选项是不同的性别，我们设定<i>value</i>应该是<i>Gender</i>的类型。

```js
export type GenderOption = {
  value: Gender;
  label: string;
};
```

<!-- In <i>AddPatientForm.tsx</i>, we use the <i>GenderOption</i> type for the <i>genderOptions</i> variable, declaring it to be an array containing objects of type <i>GenderOption</i>:-->
 在<i>AddPatientForm.tsx</i>中，我们为<i>genderOptions</i>变量使用<i>GenderOption</i>类型，声明它是一个包含<i>GenderOption</i>类型对象的数组。

```js
const genderOptions: GenderOption[] = [
  { value: Gender.Male, label: "Male" },
  { value: Gender.Female, label: "Female" },
  { value: Gender.Other, label: "Other" }
];
```

<!-- Next, look at the type <i>SelectFieldProps</i>. It defines the type for the props for our <i>SelectField</i> component. There, you can see that <i>options</i> is an array of <i>GenderOption</i> types.-->
 接下来，看一下<i>SelectFieldProps</i>类型。它定义了我们的<i>SelectField</i>组件的prop的类型。在那里，你可以看到，<i>options</i>是一个<i>GenderOption</i>类型的数组。

```js
type SelectFieldProps = {
  name: string;
  label: string;
  options: GenderOption[];
};
```

<!-- The function component <i>SelectField</i> in itself looks a bit cryptic but it just renders the label, a select element, and all given option elements (or, actually, their labels and values).-->
 函数组件<i>SelectField</i>本身看起来有点神秘，但它只是渲染了标签，一个选择元素，和所有给定的选项元素（或者，实际上，它们的标签和值）。

```jsx
const FormikSelect = ({ field, ...props }: FieldProps) =>
  <Select {...field} {...props} />;

export const SelectField = ({ name, label, options }: SelectFieldProps) => (
  <>
    <InputLabel>{label}</InputLabel>
    <Field
      fullWidth
      style={{ marginBottom: "0.5em" }}
      label={label}
      component={FormikSelect}
      name={name}
    >
      {options.map((option) => (
        <MenuItem key={option.value} value={option.value}>
          {option.label || option.value}
        </MenuItem>
      ))}
    </Field>
  </>
);
```

<!-- Now let's move on to the <i>TextField</i> component. The component renders a TextFieldMUI that is a [Material UI TextField](https://mui.com/components/text-fields/) with a label:-->
 现在让我们继续看<i>TextField</i>组件。这个组件渲染了一个TextFieldMUI，它是一个带有标签的[Material UI TextField](https://mui.com/components/text-fields/)。

```jsx
interface TextProps extends FieldProps {
  label: string;
  placeholder: string;
}

export const TextField = ({ field, label, placeholder }: TextProps) => (
  <div style={{ marginBottom: "1em" }}>
    <TextFieldMUI
      fullWidth
      label={label}
      placeholder={placeholder}
      {...field}
    />
    <Typography variant="subtitle2" style={{ color: "red" }}>
      <ErrorMessage name={field.name} />
    </Typography>
  </div>
);
```


<!-- Note that we use the Formik [ErrorMessage](https://jaredpalmer.com/formik/docs/api/errormessage) component to render an error message for the input when needed.-->
 注意我们使用Formik [ErrorMessage](https://jaredpalmer.com/formik/docs/api/errormessage)组件，在需要时为输入的内容渲染一个错误信息。
<!-- The component does everything under the hood, and we don't need to specify what it should do.-->
 该组件在引擎盖下做了所有事情，我们不需要指定它应该做什么。

<!-- It would also be possible to get hold of the error messages within the component by using the prop <i>form</i>:-->
通过使用prop<i>form</i>，我们也可以在该组件中获得错误信息。

```jsx
export const TextField = ({ field, label, placeholder, form }: TextProps) => {
  console.log(form.errors);
  // ...
}
```

<!-- Now, back to the  actual form component in <i>AddPatientForm.tsx</i>.-->
 现在，回到<i>AddPatientForm.tsx</i>中的实际表单组件。
<!-- The function component <i>AddPatientForm</i> renders a [Formik component](https://jaredpalmer.com/formik/docs/api/formik). The Formik component is a wrapper, which requires two props: <i>initialValues</i> and <i>onSubmit</i>. The role of the props is quite self-explanatory.-->
 函数组件<i>AddPatientForm</i>渲染了一个[Formik组件](https://jaredpalmer.com/formik/docs/api/formik)。Formik组件是一个包装器，它需要两个props。<i>initialValues</i>和<i>onSubmit</i>。这些prop的作用是不言而喻的。
<!-- The Formik wrapper keeps a track of your form's state, and then exposes it and a few reusable methods and event handlers to your form via props.-->
 Formik包装器会跟踪你的表单的状态，然后通过props将它和一些可接受的方法和事件处理程序暴露给你的表单。

<!-- We are also using an optional <i>validate</i> prop that expects a validation function and returns an object containing possible errors. Here, we only check that our text fields are not falsy, but it could easily contain e.g. some validation for the social security number format or something like that. The error messages defined by this function can then be displayed on the corresponding field's ErrorMessage component.-->
 我们还使用了一个可选的<i>validate</i>prop，它期望一个验证函数并返回一个包含可能错误的对象。在这里，我们只检查我们的文本字段是不是虚假的，但它可以很容易地包含例如社会安全号码格式的一些验证或类似的东西。由这个函数定义的错误信息可以显示在相应字段的ErrorMessage组件上。

<!-- First, have a look at the entire component. We will later discuss the different parts in detail.-->
 首先，看一下整个组件。我们以后会详细讨论不同的部分。

```jsx
interface Props {
  onSubmit: (values: PatientFormValues) => void;
  onCancel: () => void;
}

export const AddPatientForm = ({ onSubmit, onCancel }: Props) => {
  return (
    <Formik
      initialValues={{
        name: "",
        ssn: "",
        dateOfBirth: "",
        occupation: "",
        gender: Gender.Other
      }}
      onSubmit={onSubmit}
      validate={values => {
        const requiredError = "Field is required";
        const errors: { [field: string]: string } = {};
        if (!values.name) {
          errors.name = requiredError;
        }
        if (!values.ssn) {
          errors.ssn = requiredError;
        }
        if (!values.dateOfBirth) {
          errors.dateOfBirth = requiredError;
        }
        if (!values.occupation) {
          errors.occupation = requiredError;
        }
        return errors;
      }}
    >
      {({ isValid, dirty }) => {
        return (
          <Form className="form ui">
            <Field
              label="Name"
              placeholder="Name"
              name="name"
              component={TextField}
            />
            <Field
              label="Social Security Number"
              placeholder="SSN"
              name="ssn"
              component={TextField}
            />
            <Field
              label="Date Of Birth"
              placeholder="YYYY-MM-DD"
              name="dateOfBirth"
              component={TextField}
            />
            <Field
              label="Occupation"
              placeholder="Occupation"
              name="occupation"
              component={TextField}
            />
            <SelectField
              label="Gender"
              name="gender"
              options={genderOptions}
            />
            <Grid>
              <Grid item>
                <Button
                  color="secondary"
                  variant="contained"
                  style={{ float: "left" }}
                  type="button"
                  onClick={onCancel}
                >
                  Cancel
                </Button>
              </Grid>
              <Grid item>
                <Button
                  style={{ float: "right" }}
                  type="submit"
                  variant="contained"
                  disabled={!dirty || !isValid}
                >
                  Add
                </Button>
              </Grid>
            </Grid>
          </Form>
        );
      }}
    </Formik>
  );
};

export default AddPatientForm;
```

<!-- As a child of our Formik wrapper, we have a <i>function</i> which returns the form contents.-->
作为我们的Formik包装器的一个子节点，我们有一个<i>函数</i>来返回表单内容。
<!-- We use Formik's [Form](https://jaredpalmer.com/formik/docs/api/form) to render the actual form element. Inside of the Form element, we use our <i>TextField</i> and <i>SelectField</i> components that we created in <i>FormField.tsx</i>.-->
 我们使用Formik's [Form](https://jaredpalmer.com/formik/docs/api/form) 来渲染实际的表单元素。在表单元素中，我们使用我们在<i>FormField.tsx</i>中创建的<i>TextField</i>和<i>SelectField</i>组件。

<!-- Lastly, we create two buttons: one for cancelling the form submission and one for submitting the form. The cancel button calls the <i>onCancel</i> callback straight away when clicked.-->
 最后，我们创建两个按钮：一个用于取消表单提交，一个用于提交表单。取消按钮在被点击时直接调用<i>onCancel</i>回调。
<!-- The submit button triggers Formik's onSubmit event, which in turn uses the <i>onSubmit</i> callback from the component's props. The submit button is enabled only if the form is <i>valid</i> and <i>dirty</i>, which means that the user has edited some of the fields.-->
 提交按钮会触发Formik的onSubmit事件，它反过来使用组件prop中的<i>onSubmit</i>回调。只有当表单是<i>valid</i>和<i>dirty</i>时，提交按钮才会被启用，这意味着用户已经编辑了一些字段。

<!-- We handle form submission through Formik, because it allows us to call the validation function before  performing the actual submission. If the validation function returns any errors, the submission is cancelled.-->
 我们通过Formik处理表单提交，因为它允许我们在执行实际提交之前调用验证函数。如果验证函数返回任何错误，提交就被取消了。

<!-- The buttons are set inside a Material UI [Grid](https://mui.com/components/grid/#main-content) to set them next to each other easily.-->
按钮被设置在Material UI [Grid](https://mui.com/components/grid/#main-content)内，以便轻松地将它们挨在一起。

```jsx
<Grid>
  <Grid item>
    <Button
      color="secondary"
      variant="contained"
      style={{ float: "left" }}
      type="button"
      onClick={onCancel}
    >
      Cancel
    </Button>
  </Grid>
  <Grid item>
    <Button
      style={{ float: "right" }}
      type="submit"
      variant="contained"
      disabled={!dirty || !isValid}
    >
      Add
    </Button>
  </Grid>
</Grid>
```

<!-- The <i>onSubmit</i> callback has been passed down all the way from our patient list page.-->
 <i>onSubmit</i>回调已经从我们的病人列表页一路传递下来。
<!-- Basically, it sends an HTTP POST request to our backend, adds the patient returned from the backend to our app's state and closes the modal.-->
 基本上，它向我们的后端发送HTTP POST请求，将从后端返回的病人添加到我们应用的状态中，并关闭模态。
<!-- If the backend returns an error, the error is displayed on the form.-->
 如果后端返回一个错误，错误就会显示在表单上。

<!-- Here is our submit function:-->
这是我们的提交函数。

```js
const submitNewPatient = async (values: FormValues) => {
  try {
    const { data: newPatient } = await axios.post<Patient>(
      `${apiBaseUrl}/patients`,
      values
    );
    dispatch({ type: "ADD_PATIENT", payload: newPatient });
    closeModal();
  } catch (error: unknown) {
    let errorMessage = 'Something went wrong.'
    if(axios.isAxiosError(error) && error.response) {
      console.error(error.response.data);
      errorMessage = error.response.data.error;
    }
    setError(errorMessage);
  }
};
```

<!-- With this material, you should be able to complete the rest of this part's exercises. When in doubt, try reading the existing code to find clues on how to proceed!-->
 有了这个材料，你应该能够完成这部分的其余练习。当有疑问时，可以尝试阅读现有的代码来寻找如何进行的线索!

</div>

<div class="tasks">

### Exercises 9.23.-9.27.

#### 9.23: patientor, step8

<!-- We have established that patients can have different kinds of entries. We don't yet have any way of adding entries to patients in our app, so, at the moment, it is pretty useless as an electronic medical record.-->
 我们已经确定病人可以有不同种类的条目。我们还没有办法在我们的应用中为病人添加条目，所以，目前，它作为一个电子医疗记录是非常没用的。

<!-- Your next task is to add endpoint <i>/api/patients/:id/entries</i> to your backend, through which you can POST an entry for a patient.-->
 你的下一个任务是在你的后端添加端点<i>/api/patients/:id/entries</i>，通过这个端点你可以为一个病人发布条目。

<!-- Remember that we have different kinds of entries in our app, so our backend should support all those types and check that at least all required fields are given for each type.-->
 记住，我们的应用中有不同类型的条目，所以我们的后端应该支持所有这些类型，并检查每一种类型至少有所有必要的字段。

#### 9.24: patientor, step9

<!-- Now that our backend supports adding entries, we want to add the corresponding functionality to the frontend. In this exercise, you should add a form for adding an entry to a patient. An intuitive place for accessing the form would be on a patient's page.-->
 现在我们的后端支持添加条目，我们要在前端添加相应的功能。在这个练习中，你应该添加一个为病人添加条目的表单。访问该表单的一个直观的地方是在病人的页面上。

<!-- In this exercise, it is enough to **support <i>one</i> entry type**, and you do not have to handle any errors. It is enough if a new entry can be created when the form is filled with valid data.-->
 在这个练习中，只需**支持<i>一个</i>条目类型**，你不必处理任何错误。如果在表单中填入有效的数据时，可以创建一个新的条目，这就足够了。

<!-- Upon a successful submit, the new entry should be added to the correct patient and the patient's entries on the patient page should be updated to contain the new entry.-->
 成功提交后，新条目应被添加到正确的病人中，病人页面上的条目应被更新以包含新条目。

<!-- If you like, you can re-use some of the code from the <i>Add patient</i> form for this exercise, but this is not a requirement.-->
 如果你愿意，你可以在这个练习中重新使用<i>添加病人</i>表单中的一些代码，但这并不是一个要求。

<!-- Note that the file [FormField.tsx](https://github.com/fullstack-hy2020/patientor/blob/master/src/AddPatientModal/FormField.tsx#L58) has a ready-made component called _DiagnosisSelection_ that can be used for setting the field <i>diagnoses</i>.-->
 注意文件[FormField.tsx](https://github.com/fullstack-hy2020/patientor/blob/master/src/AddPatientModal/FormField.tsx#L58)有一个现成的组件叫_DiagnosisSelection_，可以用来设置字段<i>diagnoses</i>。

<!-- It can be used as follows:-->
它可以如下使用。

```js
const AddEntryForm = ({ onSubmit, onCancel }: Props) => {
  const [{ diagnoses }] = useStateValue() // highlight-line

  return (
    <Formik
    initialValues={{
      /// ...
    }}
    onSubmit={onSubmit}
    validate={values => {
      /// ...
    }}
  >
    {({ isValid, dirty, setFieldValue, setFieldTouched }) => { // highlight-line

      return (
        <Form className="form ui">
          // ...

          // highlight-start
          <DiagnosisSelection
            setFieldValue={setFieldValue}
            setFieldTouched={setFieldTouched}
            diagnoses={Object.values(diagnoses)}
          />
          // highlight-end

          // ...
        </Form>
      );
    }}
  </Formik>
  );
};
```

<!-- With small tweaks on types, the  readily made component <i>SelectField</i> can be used for the heath check rating.-->
 在类型上做一些小的调整，现成的组件<i>SelectField</i>可以用于健康检查等级。

#### 9.25: patientor, step10

<!-- Extend your solution so that it displays an error message if some required values are missing or formatted incorrectly.-->
 扩展你的解决方案，使其在某些所需的值丢失或格式不正确时显示错误信息。

#### 9.26: patientor, step11

<!-- Extend your solution so that it supports <i>two</i> entry types and displays an error message if some required values are missing or formatted incorrectly. You do not need to care about the possible errors in the server's response.-->
 扩展你的解决方案，使其支持<i>两个</i>条目类型，并在某些必需的值丢失或格式不正确时显示错误信息。你不需要关心服务器响应中可能出现的错误。

<!-- The easiest but surely not the most elegant way to do this exercise is to have a separate form for each different entry type. Getting the types to work properly might be a slight challenge if you use just a single form.-->
 做这个练习的最简单但肯定不是最优雅的方法是为每个不同的条目类型设置一个单独的表单。如果你只用一个表单，让这些类型正常工作可能是一个小小的挑战。

<!-- Note that that if you need alter the shown form based on user selections, you can access the form values using the parameter _values_ of the rendering function:-->
 注意，如果你需要根据用户的选择改变显示的表单，你可以使用渲染函数的参数_values_访问表单的值。

```js
<Formik
  initialValues={}
  onSubmit={onSubmit}
  validate={}
>
  {({ isValid, dirty, setFieldValue, setFieldTouched, values }) => { // highlight-line
    console.log(values); // highlight-line
    return (
      <Form className="form ui">
      </Form>
    );
  }}
</Formik>
```

#### 9.27: patientor, step12

<!-- Extend your solution so that it supports <i>all the entry types</i> and displays an error message if some required values are missing or formatted incorrectly. You do not need to care about the possible errors in the server's response.-->
 扩展你的解决方案，使其支持<i>所有的条目类型</i>，并在某些必需的值丢失或格式不正确时显示错误信息。你不需要关心服务器响应中可能出现的错误。

### Submitting exercises and getting the credits

<!-- Exercises of this part are submitted via [the submissions system](https://studies.cs.helsinki.fi/stats/courses/fs-typescript) just like in the previous parts, but unlike previous parts, the submission goes to a different "course instance". Remember that you have to finish at least 24 exercises to pass this part!-->
 这一部分的练习是通过[提交系统](https://studies.cs.helsinki.fi/stats/courses/fs-typescript)提交的，就像前几部分一样，但与前几部分不同的是，提交到一个不同的 "课程实例"。请记住，你必须完成至少24道练习才能通过这一部分!

<!-- Once you have completed the exercises and want to get the credits, let us know through the exercise submission system that you have completed the course:-->
 一旦你完成了练习并想获得学分，请通过练习提交系统告诉我们你已经完成了课程。

![Submissions](../../images/11/21.png)

<!-- Note that the "exam done in Moodle" note refers to the [Full Stack Open course's exam](/en/part0/general_info#sign-up-for-the-exam), which has to be completed before you can earn credits from this part.-->
 注意 "在Moodle中完成的考试 "是指[全栈开放课程的考试](/en/part0/general_info#sign-up-for-the-exam)，在你从这部分获得学分之前必须完成。

<!-- **Note** that you need a registration to the corresponding course part for getting the credits registered, see [here](/en/part0/general_info#parts-and-completion) for more information.-->
 **注意**你需要注册相应的课程部分以获得注册的学分，更多信息见[这里](/en/part0/general_info#parts-and-completion)。

<!-- You can download the certificate for completing this part by clicking one of the flag icons. The flag icon corresponds to the certificate's language.-->
 你可以通过点击其中一个旗子图标下载完成这部分的证书。旗帜图标与证书的语言相对应。

</div>
