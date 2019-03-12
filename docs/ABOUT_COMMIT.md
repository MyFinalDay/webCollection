# Commit规范

在对项目作出更改后，我们需要生成 commit 来记录自己的更改。以下是参照 Angular 对 commit 格式的规范：

## (1) 格式

提交信息包括三个部分：`Header`，`Body` 和 `Footer`。

```
<Header>

<Body>

<Footer>
```

其中，Header 是必需的，Body 和 Footer 可以省略。

#### 1> Header

Header 部分只有一行，包括俩个字段：`type`（必需）和`subject`（必需）。

```
<type>: <subject>
```

**type**

type 用于说明 commit 的类别，可以使用如下类别：

- feat：新功能（feature）
- fix：修补bug
- doc：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改bug的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动

**subject**

subject 是 commit 目的的简短描述。

- 以动词开头，使用第一人称现在时，比如改变，而不是改变了。
- 结尾不加句号（。）

#### 2> Body

Body 部分是对本次 commit 的详细描述，可以分成多行。下面是一个范例。

```
More detailed explanatory text, if necessary.  Wrap it to 
about 72 characters or so. 

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
```

**注意：**应该说明代码变动的动机，以及与以前行为的对比。

#### 3> Footer

​	Footer 部分应该包含：(1)Breaking Changes



#### 4> 例子

下面是一个完整的例子：

```
feat: 添加了分享功能

给每篇文章添加了分享功能

- 添加分享到微信功能
- 添加分享到朋友圈功能

```
