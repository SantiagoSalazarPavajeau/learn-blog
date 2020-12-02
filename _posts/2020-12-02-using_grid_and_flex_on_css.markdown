---
layout: post
title:      "Using grid and flex on CSS"
date:       2020-12-02 20:54:22 +0000
permalink:  using_grid_and_flex_on_css
---


I am going to go through some CSS techniques I worked on while doing a frontend challenge for an internship. The internship was unpaid so I could not make an all-in commitment with my time, but I found the challenge pretty interesting and it motivated me to take a deeper dive into CSS. 

Before this challenge, I got good at using libraries like bootstrap, material-ui, or semantic-ui, which allowed me to survive styling and design. But exploring CSS properties in more depth has helped me see styling as less 'tedious-y' to actually become more fun.

## CSS display property

The main CSS property to control the way HTML tags are set on the page is `display`. I will show some practical ways I explored to set a layout where the tags can be controlled and positioned effectively.  
 
The way containers are distributed on the page can change depending on if we use `grid` or `flex` property values on the display property. For example, to achieve the following design:

![alt-text](https://i.imgur.com/x2WvVOl.png "Layout with grid/flex")

We can set a parent div to `{display: grid;}` for the horizontal sections (beige and gold), and the child divs are set to `{display: flex;}` for the inner alignment.

## Parent and child CSS classes

```css
.grid-app{
 display: grid;
 grid-template-columns: [left] 1fr [right];
}
```

This `.grid-app` parent class gives us control of the child divs that will serve as horizontal headers where content can be divided by different topics or styles.

```css
.flexbox-light {
 display: flex;
 justify-content: center;
 height: 500px;
 background-color: #fff1e6;
}

.flexbox-dark {
 display: flex;
 justify-content: center;
 height: 500px;
 background-color: #eddcd2;
}
```

The big pattern is that setting the display property to `grid` or `flex` will affect the tags that are nested as children. In this case, the parent `grid` affects the `flex` children.

```html
<div className="grid-app">

 <div className="flexbox-light">

    <div className="width-control-container">

    </div>

 </div>

 <div className="flexbox-dark">

 </div>

</div>
```

But now using `display: flex;` and `justify-content: center;` the child `flex` tags become parents. So we can set a div tag with class `width-control-container` that controls the margins of the text as follows and to center the content.

```css
.width-control-container{
 width: 70%;
}
```

And this would allow us to add another level of nested flex div tags where the text can be justified independently on each one by using flex.

```css
.nav{
 display: flex;
 justify-content: space-between;
}

.title{
 display: flex;
 justify-content: flex-start;
}

.subtitle{
 display: flex;
 justify-content: center;
}

.search{
 display: flex;
 justify-content: flex-end;
}
```

Using flexbox the content can be set to positions like: distributed evenly over the page (`space-between`), to the left (`flex-start`), center, and to the right (`flex-end`). 

```html
<div className="grid-app">

 <div className="flexbox-light">

    <div className="width-control-container">

       <div className="nav"></div>
       <div className="title"></div>
       <div className="subtitle"></div>
       <div className="search"></div>

    </div>

 </div>

 <div className="flexbox-dark">

 </div>

</div>
```

[Code in sandbox](https://codesandbox.io/s/grid-and-flexbox-css-njsyd?file=/src/styles.css)

Feel more than welcome to reach out with any comments/ideas!

[LinkedIn](https://www.linkedin.com/in/santiago-salazar-pavajeau/)
[Twitter](https://twitter.com/santispavajeau)


References:

[CSS display](https://www.w3schools.com/cssref/pr_class_display.asp)

[One line layouts](http://1linelayouts.glitch.me/)








