---
layout: post
title:  "BEM快速入门"
---

> * 原文地址： [Quick start](http://en.bem.info/methodology/quick-start/)
> * 译者：Yvan Yang


## 简介

BEM（块，元素，修饰符）是一种基于组件的Web开发方法。其背后的思想是将用户界面划分为独立的块。
即使是复杂的UI，这也使界面开发变得简单快捷，并且允许重复使用现有的代码，而无需复制和粘贴。

## 内容

* [块](#块)
* [元素](#元素)
* [我应该创建块还是元素呢？](#我应该创建块还是元素呢)
* [修饰符](#修饰符)
* [混合](#混合)
* [文件结构](#文件结构)

## 块

可以重复使用的功能独立的页面组件。在HTML中，块由类`class`属性表示。

特征： 

* 块名称描述了它的目的（“它是什么？” - `菜单`或`按钮`），而不是其状态（“它是什么样子”？ - `红色`或`大`）。

**范例**

```html
<!-- 正确。 `错误`块在语义上是有意义的 -->
<div class="error"></div>

<!-- 不正确。它描述了外观 -->
<div class="red-text"></div>
```

* 该块不应影响其环境，这意味着您不应设置块的外部几何（边距）或定位。
* 使用BEM时也不应使用CSS标签或`ID`选择器。

这确保了重新使用块或将它们从一个地方移动到另一个地方的必要独立性。

### 块使用指南

#### 嵌套 

* 块可以相互嵌套。 
* 您可以拥有任意数量的嵌套级别。

**范例**

```html
<!-- `header` 块 -->
<header class="header">
    <!-- 嵌套的 `logo` 块 -->
    <div class="logo"></div>

    <!-- 嵌套的 `search-form` 块 -->
    <form class="search-form"></form>
</header>
```

## 元素 

块的复合部件，不能与其分开使用。

特征： 

* 元素名称描述其目的（“这是什么？” - `项目`，`文本`等），而不是其状态（“什么类型，或什么样子”？ - `红色`，`大`等）。
* 元素全名的结构是`block-name__element-name`。元素名称与块名称以双下划线（`__`）分隔开。

**范例**

```html
<!-- `search-form` 块 -->
<form class="search-form">
    <!-- `search-form`块中的`input`元素 -->
    <input class="search-form__input">

    <!-- `search-form`块中的`button`元素 -->
    <button class="search-form__button">Search</button>
</form>
```

# 元素使用指南

* [嵌套](#嵌套)
* [成员关系](#成员关系)
* [可选性](#可选性)

## 嵌套 

* 元素可以彼此嵌套。 
* 您可以拥有任意数量的嵌套级别。 
* 元素总是块的一部分，而不是另一个元素。这意味着元素名称不能定义一个层次结构，比如block__elem1__elem2。

**范例**

```html
<!--
    正确。完整元素名称的结构遵循以下模式：
    `block-name__element-name`
-->
<form class="search-form">
    <div class="search-form__content">
        <input class="search-form__input">

        <button class="search-form__button">Search</button>
    </div>
</form>

<!--
    不正确。完整元素名的结构不符合模式：
    `block-name__element-name`
-->
<form class="search-form">
    <div class="search-form__content">
        <!-- 推荐：`search-form__input`或`search-form__content-input` -->
        <input class="search-form__content__input">

        <!-- 推荐：`search-form__button`或`search-form__content-button` -->
        <button class="search-form__content__button">Search</button>
    </div>
</form>
```

块名定义命名空间，它保证元素依赖于块（`block__elem`）。

块可以在DOM树中具有嵌套的元素结构：

**范例**

```html
<div class="block">
    <div class="block__elem1">
        <div class="block__elem2">
            <div class="block__elem3"></div>
        </div>
    </div>
</div>
```

但是，在BEM方法论中，块结构始终表示为元素的扁平列表：

**范例**

```css
.block {}
.block__elem1 {}
.block__elem2 {}
.block__elem3 {}
```

这允许您更改块的DOM结构，而不对每个单独元素的代码进行更改：

**范例**

```html
<div class="block">
    <div class="block__elem1">
        <div class="block__elem2"></div>
    </div>

    <div class="block__elem3"></div>
</div>
```

块的结构发生变化，但元素及其名称的规则保持不变。

#### 成员关系

元素总是**块的一部分**，不应该与块分开使用。

**范例**

```html
<!-- 正确。元素位于`search-form`块内 -->
<!-- `search-form` 块 -->
<form class="search-form">
    <!-- `search-form`块中的`input`元素 -->
    <input class="search-form__input">

    <!-- `search-form`块中的`button`元素 -->
    <button class="search-form__button">Search</button>
</form>

<!--
    不正确。元素位于`search-form`块的上下文之外     
-->
<!-- `search-form` 块 -->
<form class="search-form">
</form>

<!-- `search-form`块中的`input`元素 -->
<input class="search-form__input">

<!-- `search-form`块中的`button`元素 -->
<button class="search-form__button">Search</button>
```

#### 可选性 

元素是可选的块组件。不是所有的块都有元素。

**范例**

```html
<!-- `search-form` 块 -->
<div class="search-form">
    <!-- `input` 块 -->
    <input class="input">

    <!-- `button` 块 -->
    <button class="button">Search</button>
</div>
```

## 我应该创建块还是元素呢？

### 创建一个块

如果一段代码可能被重用，并且不依赖于正在实现的其他页面的组件。

### 创建一个元素

如果一段代码不能在没有父实体（块）的情况下单独使用。


## 修饰符 

定义块或元素的外观，状态或行为的实体。

特征：

* 修饰符名称描述其外观（“多少尺寸”或“哪个主题”等等 - `size_s`或`theme_islands`），其状态（“与其他的有什么不同？” - 禁用`disabled`，聚焦`focused`等）及其行为（“它如何表现？”或“如何响应用户？” - 如`directions_left-top`）。
* 修饰符名称通过单个下划线（`_`）与块或元素名称分隔开。

### 修饰符的类型

#### 布尔 

* 仅当修饰符的存在或不存在是重要的时才使用，其值是无关紧要的。例如，禁用`disabled`。如果存在布尔修饰符，则假定其值为`true`。 
* 修饰符的全名的结构遵循以下模式： 
    * `block-name_modifier-name`
    * `block-name__element-name_modifier-name`

**范例**

```html
<!-- `search-form`块具有`focused`布尔修饰符 -->
<form class="search-form search-form_focused">
    <input class="search-form__input">

    <!-- `button`元素具有`disabled`布尔修饰符 -->
    <button class="search-form__button search-form__button_disabled">Search</button>
</form>
```

#### 键-值

* 当修饰符值很重要时使用。例如，“具有`islands`设计主题的菜单”：`menu_theme_islands`。 
* 修饰符的全名的结构遵循以下模式：
    * `block-name_modifier-name_modifier-value`
    * `block-name__element-name_modifier-name_modifier-value`

**范例**

```html
<!-- `search-form`块的`主题'修饰符的值为`islands` -->
<form class="search-form search-form_theme_islands">
    <input class="search-form__input">

    <!-- `button`元素的`size`修饰符的值为`m` -->
    <button class="search-form__button search-form__button_size_m">Search</button>
</form>

<!-- 您不能同时使用两个具有不同值的相同修饰符 -->
<form class="search-form
             search-form_theme_islands
             search-form_theme_lite">

    <input class="search-form__input">

    <button class="search-form__button
                   search-form__button_size_s
                   search-form__button_size_m">
        Search
    </button>
</form>
```

### 修饰语使用指南

#### 修饰符不能单独使用 

从BEM的角度来看，修改器不能与修改的块或元素隔离使用。修饰符应该更改实体的外观，行为或状态，而不是替换它。

**范例**

```html
<!--
    正确。 `search-form`块有`theme`修饰符的值 `islands`
-->
<form class="search-form search-form_theme_islands">
    <input class="search-form__input">

    <button class="search-form__button">Search</button>
</form>

<!-- 不正确。缺少修改的类`search-form` -->
<form class="search-form_theme_islands">
    <input class="search-form__input">

    <button class="search-form__button">Search</button>
</form>
```

## 混合 

在单个DOM节点上使用不同BEM实体的技术。

混合使您可以： 
* 组合多个实体的行为和样式，而不需要重复代码。 
* 根据现有的UI创建语义上的新UI组件。

**范例**

```html
<!-- `header` 块 -->
<div class="header">
    <!--
        search-form`块与标题（`header`）块中的`search-form`元素被混合在一起         
    -->
    <div class="search-form header__search-form"></div>
</div>
```

在这个例子中，我们结合了搜索表单`search-form`块的行为和样式以及标题`header`块中的`search-form`元素。这种方法允许我们在`header__search-form`元素中设置外部几何和定位，而搜索表单块本身仍然是通用的。因此，我们可以在任何其他环境中使用该块，因为它没有指定任何填充。这就是为什么我们可以称之为独立的原因。

## 文件结构 
BEM方法中采用的组件方法也适用于文件结构中的项目。块，元素和修饰符的实现被分为独立的技术文件，这意味着我们可以单独连接它们。

特征： 

* 单个块对应于单个目录。 
* 块和目录具有相同的名称。例如，`标题`块位于`标题/`目录中，`菜单`块位于`菜单/`目录中。 
* 块的实现被分成单独的技术文件。例如，`header.css`和`header.js`。 
* 块目录是其元素和修饰符的子目录的根目录。 
* 元素目录的名称以双下划线（`__`）开头。例如，`header/__logo/`和`menu/__item/`。 
* 修饰符名称的名称以一个下划线（`_`）开头。例如，`header/_fixed/`和`menu/_theme_islands/`。 
* 元素和修饰符的实现分为单独的技术文件。例如，`header__input.js`和`header_theme_islands.css`。

**范例**

```bash
search-form/                           # Directory of the search-form

    __input/                           # Subdirectory of the search-form__input
        search-form__input.css         # CSS implementation of the
                                       # search-form__input element
        search-form__input.js          # JavaScript implementation of the
                                       # search-form__input element

    __button/                          # Subdirectory of the search-form__button
                                       # element
        search-form__button.css
        search-form__button.js

    _theme/                            # Subdirectory of the search-form_theme
                                       # modifier
        search-form_theme_islands.css  # CSS implementation of the search-form block
                                       # that has the theme modifier with the value
                                       # islands
        search-form_theme_lite.css     # CSS implementation of the search-form block
                                       # that has the theme modifier with the value
                                       # lite

    search-form.css                    # CSS implementation of the search-form block
    search-form.js                     # JavaScript implementation of the
                                       # search-form block
```

这个文件结构使得它很容易支持代码及重用它。

>分支文件结构假设在生产中将代码组合成共享项目文件。

您不需要遵循推荐的文件结构。您可以使用遵循BEM原则的任何其他项目结构来组织文件结构，例如：
* [Flat](http://en.bem.info/methodology/filestructure/#flat)
* [Flex](http://en.bem.info/methodology/filestructure/#flex)
