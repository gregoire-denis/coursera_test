# Coursera Web Dev Course : CSS Notes

## CSS Basics

CSS stands for cascading style sheets. 
Its purpose is to stylise the raw content provided by the HTML document
It is usually implemented in a separate document from the HTML file, the `<style>` tag and `style=""`attribute/value pair should be avoided to place styles, for consistency and redudancy purposes, i.e avoiding to have to rewrite similar styles (and open the code to mistakes) for every HTML page in our website. 

To place a CSS stylesheet externally, we should place a `<link>` tag in our `<head>` element.
The syntax looks like this : 

```html
<head>
<link rel="stylesheet", href="style.css">
</link>
</head>
```

We can see that we specify to the browser that it's about to read a CSS stylesheet with the `rel=""` attribute/value pair (which applies to  
`<a>`, `<link>`, and `<area>` elements, and indicates a relationship between the current document and the provided resources, i.e this resource stylises this document, this resource is refering to the author of the document, this resource holds the license for the document, etc...).
We then provide the path to the stylesheet as we would with any link, using the `href=""` attribute/value pair. 


Side note : a good resource to look at various CSS implementations around the same HTML file is <a href="https://csszengarden.com">csszengarden</a>, where users can submit their own stylesheets, while the core HTML document remains unchanged.


## Anatomy of a CSS Rule

CSS works by associating rules with HTML elements.
These rules govern how the content of specified elements should be displayed.

A CSS rule consists of : a **selector**, which indicates what elements are governed by the rule, followed by a pair of curly braces, inside of which are the **CSS declarations**. The declarations are composed of a **property/value** pair, with the property and the value being separated by a colon `:`. A declaration should (it is best practice) end with a semicolon `;`.

Example: 

```css
p {
	color: blue;
	font-size: 20px;
	width: 200px;
}
```

The collection of CSS rules is called a style sheet.

## Element, class and ID selectors 

CSS selectors are used to determine which HTML elements, or set of elements the browser should apply the CSS declarations to. 
The browser uses its selector API to traverse the DOM, and pick out the elements matching the selector. 

The element selector is just specifying the element name, and the browser will apply the declarations to all elements matching that name.
Ex : ` p {color: blue;}`

The class selector requires the HTML elements to have a class attribute specified, i.e `<p class="blue"></p>`.
The syntax for this selector is : a dot `.`, followed by the name of the class, i.e : ` .blue {color: blue;}`
This selector will target all elements with the specified class. 

The ID selector require the HTML elements to have id attributes specified, i.e : `<p id="name"></p>`
The syntax for this selector is : a pound `#`, followed by the value of the ID attribute we look to target, i.e : ` #name {color: blue;}`
This will exclusively target the element that holds the same value for their ID attribute as the one specified by the selector 

CSS allows us to use multiple selectors for declarations, i.e : `p, .blue, #name {color: blue;}`

## Combining selectors 

Selectors can also be combined with CSS, to allows for more control over which elements are targeted.

**Element with class selector**: 

```css 
p.big {font-size: 20px;}
```

This targets every `p` element that belongs to the class `big`. **No space between the element and the class in the selector**
This is useful when you have a class declaration, but want a slightly different styling for some elements of the same class.

**Child selector**

```css
article > p {color: blue;}
```

These declarations are read from right to left : in this instance, every `p` element that is a **direct** child (directly branching from) of any `article` element.
This isn't limited to element selectors : 

```css
article > .big {color: blue;}
```

This targets any element belonging to the `big` class that is a direct child of an `article` element

**Descendant selector**

```css
article p {color: blue;}
```

This targets any `p` element that is nested **anywhere** inside an `article` element. **A space is required between both selectors**
This isn't limited to element selectors : 

```css
.big p {color: blue;}
```

This targets any `p` element that is nested inside any element that belongs to the `big` class.

### Pseudo class selectors 

Pseudo class selectors ardess targeting only the structures that can be targeted with the combination of regular selectors, or targeting the ability to style based on user interaction.
For example, if we would want the styling of an element to change when the users hovers, clicks, etc, on it, we would use a pseudo class selector

The way we specify a pseudo class selector is by specifying one known selector, a colon `:` and a predefined pseudo class name.
There are many pseudoclass selectors that exist, such as : `:link`, `:visited`, `:active`, `:nth-child()`

For example, if we want to style an unordered list of links as a menu, which is common practice, we would use pseudo class selectors.
The reason for this is it isn't easy to style links with normal selectors, because links have **states** that can be targeted using pseudoclass selectors :

```css
a:link, a:visited {}
```

Here, `a:link` targets the link in its normal state, and `a:visited` targets links that have been visited already by the user. This example lumps the two together, so there is no difference in style once you've clicked the links.
Now, if we'd want some different styling upon hovering the links, and during the clicking (the state that occurs once the user clicks on the link, but hasn't released it yet) : 

```css
a:hover, a:active {}
```
Now if we want to target a specific element that is a child of another element, and adress it by its position relative to its parent element : 

```css
li:nth-child(3) {}
```
This targets the third element of a list element

```css
li:nth-child(odd) {}
```
This targets all odd elements of a list element

`:nth-child()` pseudo class selectors can get convoluted quickly, and it is good practice to keep them readable, for ease of comprehension purposes.

```css
li:nth-child(odd):hover {}
```
This targets all odd elements of a list element, in their hover state. 

### Conflict Resolution

Cascading is a fundamental feature of CSS. It's an algorithm defining how to combine properties' values originating from different sources. 
The cascade allows the browser to know which rule superseeds the others. 

To get a basic understanding of the cascading algorithm, some core concepts should be understood : origin/precedence, merge, inheritance, specificity.

*Origin/Precedence, Merge, inheritance* : 
In conflict:
When two declarations are in conflict, i.e when they specify the same property for the same target, the origin/precedence rules occurs.
It's simple : **last declaration wins**, in other words, the declaration further down the document has precedence over the earlier ones. It overwrites precedent declaration of the same nature.

No conflict: While there is no conflict, i.e when declarations have the same target, but don't specify the same properties, declarations **merge** : 

```css
p:hover {background-color: red;}
```
And 

```css
p:hover {font-size: 24px;}
```

These two declarations will be merged as one by the browser: 

```css
p:hover {
	background-color: red;
	font-size: 24px;
}
```

Elements also inherit CSS declarations from their parent elements, which unless overwritten, will apply to all children elements of the parent targeted by the declaration : 

```css
body {
	color: blue;
	font-size: 24px;
}

p {color: red;}
```
In this example, all `p` elements that are children of the `body` element (which is to say all `p` elements in the document), will have their text color set to blue, and their font size set to 24 px. However, because we overwrite the color declaration later down the line, `p` elements will have their text color set to red, but will retain their font size as inherited from their parent element.

*Specifity* :

This concept, and the rule associated to it look simple : the most specific selector has precedence (wins).
However, it is quite common to forget about this rule, and messes with the code. 
To keep track of our specificity precedence, we can use a score based system, i.e : 

```html 
<h2 style="color: green;"></h2>
```

Here, we have an example of the most specific selector, and the "highest scoring", which is the `style=""` attribute/value pair. This is considered the most specific because it is like pointing directly at a very specific element. We are not targeting all h2 elements with this, we are targeting **this** h2 element , and this one only. 
The second most specific selector is the ID selector.
The third most specific selector is the class, or pseudo class.
The least specific selector is the number of elements.

We can think of each of these categories as powers of 10 when it comes to the score : 
The first one equals 1000's, the second one 100's, the third one 10's, and the last one units (1,2,3, etc)
Examples of scores : 
```css
div p {color: green;}
```
Scores a 0002 : this selector only specifies two elements, not their ID, not their class, etc

```css
div #myP {color: blue;}
```
Scores a 101: this selector specifies one element, and specifies it's ID

```css
div.big p {color: green;}
```
Scores a 12 : this selector specifies two elements, and one class

**The more specific a selector is, the most precedence it has over other selectors that could target the same elements**

Using `!important` on a selector overwrites all precedence rules stated before. However, its isn't good practice to use it to bypass complicated precedence relations, it is better to understand them completely and be able to work with normal precedence rules. **Avoid using `!important` unless we absolutely have to**.

### Styling Text

There are a great many ways to stylise text in CSS. 
Here we will look at examples that illustrate the core concepts behind stylising text in CSS. 

The `font-family` property allows us to set a specific font, and alternative fonts if the user doesn't have the specified font installed : 

```css
.text {
	font-family: "Times New Roman", Times, serif;
}
```

Is read from left to right : we would like the Times New Roman font, if not available then the Times font, and if not available a standard serif font. 

The `color` property specifies the color of the text, with several types of values being allowed : 

```css 
.text {
	color: #0000ff
}
```

We can see we're using hexadecimal values, but we can also use RGB, and preset values : 

```css
.text {
	color: rgb(0,0,255);
}
```
And 

```css
.text {
	color: red;
}
```

Note: hexadecimal values are actually RGB values expressed as hexadecimal numbers : for `#0000ff` , the first 00 indicates the amount of red, the middle 00 the amount of green, and ff the amount of blue.

The `font-style` property specifies whether we want our font to be normal, italic, or oblique (but generally normal or italic are sufficient)

```css
.text {
	font-style: italic
}
```

The `font-weight` property specifices the thickness (the weight) of the font: it can be attributed values such as `normal`, `bold`, and numerical values.

```css
.text {
	font-weight: 300
}
```
or

```css
.text {
	font-weight: bold;
}
```
In practice, people mostly use `normal` or `bold`.

The `font-size` property specifies the size of the font:

```css
.text {
	font-size: 24px;
}
```
Most if not all modern browsers have a **default font size of 16px**. These pixel values are not to be confused with typographic points, which are used by text editors i.e Microsoft Word (Ariel 12pts).
Although pixels values are somewhat relative to the DPI (Dots per Inch) of the device, they are still considered an absolute value. 

This property can be attributed values expressed in pixels, em, percentages, absolutes (xx-small, x-small, small, etc), relatives (larger, smaller), and global (inherit, initial, unset)

When using relative values, we are using values relative to the default size of the font, as defined in the parent element . These relative values, when used with subsequent elements, don't overwrite themselves, they allow a calculation based on the previous size that has been set.
Values expressed in ems are basing the calculation relative to the *width of an m letter in current font*: 1 em is equivalent to the width of one letter m in the font we're using. 

```html
<body style="font-size: 120%;">
<p style="font-size": 2em;"">
</p>
</body>
```
In this example, the 120% value in the `body` element is relative to the parent `html` element font size, which is by default 16px so the value would be (16px * 120%), and the `p` element font size is relative to the font size defined in its parent `body` element, therefore also relative to the `html` element font-size, which would be (16px * 120% * 2em)

A common practice is to set the font size for the entire document using `%` or absolute values, and then setting specific font sizes within the document using em values.

The `text-transform` property allows us to control how the text looks in some aspects, such as whether it should be capitalized, uppercased, lowercased:

```css
.text {
	text-transform: capitalize
}
```

The `text-align` property allows us to control where the text should be positioned in the viewport, as in centered, left/right justified: 

```css
.text {
	text-align: center;
}
```

### The Box Model

[Course Example](examples/Lecture19/box-model-before.html)

In HTML, every elements is considered a box. 
However, there is more to this box than whatever content the element wraps around. 
Besides the actual content, each box consists of : padding, border,	and margin.

The Box Model refers to the components that make an HTML box, as well as the rules that govern how these box components affect the layout, and how width and height of the box are calculated.

In this example : 

```html 
<html>
<head>
<style>

body {
	background-color: gray;
}

#box {
	background-color: blue;
}

#content {
	background-color: #90EE90: //green
}

h1 {
	margin-bottom: 30px;
}

</style>
</head>
<body>
<h1> Box Model </h1>
<div id="box">
	<div id="content">Filler Text</div>
</div>
</body>
</html>
```
Since `div` elements are block level elements, they try to fill up the space in which they are nested.
But we can see that the `#box` div doesn't quite fill all the space in the parent `body` element.
Although there isn't any spacing specified in the div, there is a default margin inherent to the `body`element, with a default value of 8px (this default setting is located in the **user agent stylesheet**, which is the default stylesheet for the browser).
We can however *reset* these default values : 

```css
body {
	margin: 0;
	padding: 0;
	background-color: gray;
}
```
Our content is now flush with the browser window.

If we look at the div itself, we can see that the background is entirely green. We're not seeing any blue, although we specified it to be the background color for the `#box` div. 
That is because the inner `#content` div is covering up the parent `#box` div entirely, because they are the same size, they both try to fill up their parent element entirely, so `#box` tries to fill up `<body>`, and `#content` tries to fill up `#box` afterwards.

```css
#box {
	background-color: blue;
	padding: 10px 10px 10px 10px;
}
```
We can now see our `#box` background color behind the `#content`. 
Note that padding syntax is as follows : `element {padding : top right bottom left}`, and if our padding is consistent between all sides, we can simply write it's value once, and it will be applied to all sides.

Now if we want to give our box some border, and margins : 

```css
#box {
	background-color: blue;
	padding: 10px 10px 10px 10px;
	border: 5px solid black;
	margin: 40px;
}
```
Note that the margin here is written using the same shortcut as for the padding. We only write one value, which means we want a margin of this value on all sides.

We can actually set the width of the element ourselves as well : 

```css
#box {
	background-color: blue;
	padding: 10px 10px 10px 10px;
	border: 5px solid black;
	margin: 40px;
	width: 300px;
}
```
The box is now smaller. If we examine the element, we can see several things.
The height as been adjusted around the content, because we didn't restrain it as we did for the width.
We specified that we wanted our box to be 300px wide, but on closer inspection, we can see this width actually excludes both padding and border, which makes the box itself be a different width from 300px, here (300 + (10 * 2) + (5 * 2)) 330px.

This is because by default, the `box-sizing` property is actually set to `content-box` value.
We want to change that in most cases, for consistency and convenience.
CSS3 offers a new value for this property, which is `border-box`.

```css
#box {
	background-color: blue;
	padding: 10px 10px 10px 10px;
	border: 5px solid black;
	margin: 40px;
	width: 300px;
	box-sizing: border-box;
}
```
With this declaration , we can now set the height and width of the whole box including padding and border directly. So the real width of the `#box` div is now indeed 300px.
Most contemporary bootstraps use this as the default box sizing model.

Because we generally want the `box-sizing` property to be set to `border-box` at all times, we don't want to have to declare it for each element. 
But the problem is this property is **non heritable**.
To get around that, we can use a different selector from those we've seen before : the `*` selector: 

```css
* {
	box-sizing: border-box;
}
```
The `*` means select every element, select all. 
This differs from inheritance, because we are directly selecting all elements at the same time with this selector. 
This is generally how we want to apply non heritable properties across the entire page.

Left and right margins are cumulative, meaning if we have two elements side to side, and the leftmost one has a right margin of 40px, while the rightmost one has a left margin of 50px, the cumulative margin between the two elements is 90px.
But with top and bottom margins, the rule differs, and instead of adding on top of each other, they collapse, with the largest margin taking priority. 
So if we have two elements on top of each other, and the topmost one has a bottom margin of 20px, while the bottom-most one has a top margin of 30px, the margin between the two becomes 30px.

Because the user agent stylesheet has some default margins set for some specific elements (such as `<h1>`), this will overwrite the reset we declared in the rules apllying to `<body>`. 
If we want to completely reset margins and paddings accross all elements, so we can manipulate them precisely and consistently, we can use our `*` selector: 

```css 
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
```

Now what happens if we add more content to our `#box` div, but constrain the height as well as the width ?
If the height isn't set high enough, our content will spill out of the box, and onto our layout (if we add another element right below the spilling element, it will spill onto it).
To avoid this, we can use the `overflow` property. 
This can be set to `hidden`, which will simply crop out the spilling text, `auto` which will add scrollbars where needed, `scroll` which will add both scrollbars, even if they're not both needed.
It is to be noted that users frown upon having several scrollbars in the same page, as navigation becomes tedious. 
It is therefore generally best practice to tailor the layout to fit the content. 

### The background property

[Course Example]("examples/Lecture20/background-after.html")

Looking at this example : 

```css
#bg {
	width: 500px;
	height: 500px;
	background-color: blue;
}
```
```html 
<div id="bg">Background properties</div>
```
If we wanted an image as a background, we can use the `background-image` property : 

```css
#bg {
	width: 500px;
	height: 500px;
	background-color: blue;
	background-image: url("examples/Lecture20/yaakov.png")
}
```
The url that we supply is a relative URL, but is has to be relative to our CSS file.

We can see that using this declaration , the image appears in the background, but is repeated. 
We can change that with another property, `background-repeat`, which has 4 values : `no-repeat`, `repeat`, `repeat-x`, `repeat-y`.
The first two switch the repeating algorithm on and off, while the last two make it repeat the image either vertically or horizontally only.

```css
#bg {
	width: 500px;
	height: 500px;
	background-color: blue;
	background-image: url("examples/Lecture20/yaakov.png");
	background-repeat: no-repeat; 
}
```

We can also see that while using both `background-color` and `background-image` properties, the image takes priority over the color, and sits on top of it. 

We can also set the position of the image within the background, using the `background-position` property, which takes in horizontal and vertical values. Omitting one of these values will default it as center:

```css
#bg {
	width: 500px;
	height: 500px;
	background-color: blue;
	background-image: url("examples/Lecture20/yaakov.png");
	background-repeat: no-repeat; 
	background-position: top right;
}
```
The image is now sitting in the top right.

We can use the `background` property as a shortcut to include all these properties in one : 

```css
#bg {
	width: 500px;
	height: 500px;
	background-color: blue;
	background: url("examples/Lecture20/yaakov.png") no-repeat right center;
}
```
But by doing this, we loose the color property. This is because we declared `background` after `background-color`, and since `background-color` is a sub property of `background`, every sub property that we haven't specified in the `background` property has been overwritten. 
So in this case we either should declare `background-color` after `background`, or better yet assign the color value directly in the `background` declaration: 

```css
#bg {
	width: 500px;
	height: 500px;
	background: url("examples/Lecture20/yaakov.png") no-repeat right center blue;
}
```

The background property can get pretty complex, and can be used for adjusting image size/resolution based on viewport size. 

### Position elements by floating 

[Course Example](examples/Lecture21/floating-after.html)

```html
<h1>Floating Elements</h1>

<div>
  <p id="p1"></p>
  <p id="p2"></p>
  <p id="p3"></p>
  <p id="p4"></p>
  <section>This is regular content continuing after the the paragraph boxes.</section>
</div>
```

```css
div {
  background-color: #00FFFF;
}
p {
  width: 50px;
  height: 50px;
  border: 1px solid black;
}

#p1 {
  background-color: #A52A2A;
}
#p2 {
  background-color: #DEB887;
}
#p3 {
  background-color: #5F9EA0;
}
#p4 {
  background-color: #FF7F50;
}
```

If we append, for example, this declaration to `#p1` : 

```css
#p1 {
	background-color: #A52A2A
	float: right;
}
```

We see that `#p1` jumps to the right of the page, and the rest of the `p` elements move up as if `#p1` wasn't there anymore.
This is because **when you float elements, the browser takes them out of the regular document flow**.
When it comes to floated elements and their margins, **they never collapse**.
Because floated elements are taken out of the document flow, we see that the div surrounding our `p` elements is now smaller, since it is wrapping around the last element in the document flow, and ignores the floated elements.

We can use the `clear` property to make sure our div wraps around both our `p` elements and our `section` element:

```css
section {
	clear: left;
}
```
By doing this, we indicate that no elements should be allowed to float to the left of our `section` element, so it will be placed under the floating elements (under because it follows normal flow).
We can also apply that to elements that are floating themselves: 

```css
#p3 {
  background-color: #5F9EA0;
  clear: left;
}
```
We now see our `#p3` placed itself under the first two `p` elements, along with the `#p4` element. 

If we need to prevent elements from floating on either side of other elements, we can use the `clear` property and give it value of `both`. This prevents overlapping in case we have elements floating both left and right and we want to clear other elements from them. 

### Relative and absolute element positioning
[Course Examples]("examples/Lecture22/positioning-after.html")

Contrary to floating positoning schemes, which disturb the normal flow of the document, relative and absolute positioning schemes do not. 

**Static positioning** is the normal document flow, and is the default positioning for all elements in a web documents, except the `html` element itself. 
If we apply offsets to elements which are static, they are ignored. 

**Relative positioning** changes the positioning of the element **relative to its normal position in the document flow**.
If we apply offsets to that element, they will be applied from the normal position of the element in the document. 
The element is **not** taken out of the normal document flow, its original position is "reserved".

The positioning offset properties are : `top`, `bottom`, `left`, `right`. 
We can also use negative values for these properties. 

**Absolute positioning:**	
All offsets are relative to the position of the nearest ancestor which has positioning set on it, other than static.
By default, `html` is the only element that has non static positioning set to it (relative).
Unlike relative positioning, an element which has its position set to absolute is taken out of the normal document flow. 
If a container element is offset, everything inside is offset with it

## Introduction to responsive design

### Media Queries
[Course Examples]("examples/Lecture23/media-queries-after.html")

Media queries allow us to group styles together and target them on devices, based on some criteria, i.e screen size, which is a very common criteria, because it can allow us to distinguish between a mobile device or a desktop browser. 

**Syntax:**
A media query starts with the `@media` keyword, a media feature, which resolves to either true or false, and curly braces which wrap around css styles: 

```css
@media (max-width: 767px) {
	p {
		color: blue;
	}
}
```
If the media feature resolves to true, the css it contains is applied.
These are common media features, among the many available:

```css
@media (max-width:) {}
@media (min-width:) {}
@media (orientation: portrait) {}
@media screen {}
@media print {}
```

The W3C defines standard device sizes as such : 
Extra Small devices (small phones) : 600px and under
Small devices (phones, portrait tablets) : 600 px and up 
Medium devices (landscape tablets) : 768 px and up
Large devices (laptops/desktops with smaller screens) : 992 px and up 
Extra Large devices (large laptops and desktops) : 1200px and up

We can combine more than one media feature by using logical operators.

```css
@media (min-width: 768px) and (max-width: 991px) {}
```

The comma `,` is equivalent to the operator `or` in other languages

```css
@media (min-width: 768px) , (max-width: 991px) {}
```
Practically speaking, when approaching responsive design, the most common operator is the `and` operator. 

When it comes to writing our stylesheets, it is recommended to start with the "base styles", and then append them with the media queries.
When writing media queries, it is important not to overlap range boundaries.

### Responsive layout

A responsive website is designed to adapt its layout to the viewing environment by using fluid, proportion based grids, flexible images and media queries.
The site's layout is supposed to adapt to the size of the device it's being viewed on. Therefore, content verbosity, or its visual delivery may vary depending on the viewport.

The most common approach to a responsive layout is **a 12 column grid**, which is called a **framework**, specifically **bootstrap** with these proportions.
We use 12 because of it's factorization : 12 is evenly divisible by 1,2,3,4,6, and 12 itself. This allows us to split our content into even, cleanly divided cells.
So for example we can split our content into 4 columns, in 3 units of size, or 3 columns of 4, etc...

To approximate the size of one column in the 12, we can simply divide 100 (the full width of the viewport) by 12, and get **8.33** approximately. 

We are not limited to the viewport in this approach : each cell can be subdivided in 12 columns, it is simply a matter of proportions.

If we combine these notions with media queries, we can tailor our website with different layouts depending on the device's screensize. 
So for example, we could have a website which uses a 4x3 layout for our menu buttons when using a large device, and a 2x6 layout when using a medium device. 

A useful tool to see how our layout responds to different screen sizes is within the Chrome Developer Tools, the Device Toolbar (CMD + SHIFT + M/ CTRL + SHIFT + M)

Browsers will look to zoom out automatically on smaller devices, so they can try to fit the content in the viewport,  and we can prevent that using the `<meta>` tag : 

```html
<meta name="viewport" content="width=device-width initial-scale=1">
```

By doing so, the browser will consider the size of the viewport to be the size of the device, and the scale factor to be 1, meaning it won't scale anything up or down by itself.


## Introduction to Twitter Bootstrap

Bootstrap is the most popular HTML, CSS and JS framework for developing responsive, mobile first projects. 
It provides premade CSS classes, but requires slight modifications to HTML, so that bootstrap can apply the CSS to these classes (adding divs here and there, and providing specific classes to our elements).
Mobile first means we PLAN for mobile right from the start, and also means the CSS framework should be mobile ready. 

Some claim there are disadvantages to using bootstrap:

It can be too big, too bloated, because it has a LOT of features, many of which will never be used by our website. 
We can use *selective download* on the bootstrap website to only obtain whichever feature we want to use for our project

We can write our own framework, that's much more targeted and smaller in scale. 
While that may be true, it is much longer (especially for beginners) to write, and slows our project down considerably.

When downloading bootstrap, we get several folders, namely CSS, fonts, and JS, and a index.html file.
These contain all the code used by bootstrap. We can note that the JS folder must contain a file named jquery. 
This is a JS library, and the JS used by bootstrap is dependent on that library to function. So we must add that to our JS folder.

If we look inside index.html, we can see that it links to both bootstrap.min.css, and styles.css. The first link is our boostrap framework, and the second one is the personalized styles we want to apply to our project. This file is, obviously empty before we write anything ourselves. 
We declare bootstrap.min.css first because we want our styles.css to overwrite whatever is defined in bootstrap.min.css. 

At the very end of the body tag, we declare some javascript files and libraries that we are going to depend on : 

```html 
<script src="js/jquery-1.11.3.min.js"></script>
<script src="js/bootstrap.min.js"></script>
<script src="js/script.js"></script>
```
We declare these in this specific order because: bootstrap.min.js depends on jquery.X.min.js (X stands for whichever version of jquery we are working with), and script.js is declared last because it will use either the jquery or bootstrap files, or both.

By default, our index.html page is blank. If we simply add a `<h1>` tag with some content, we can clearly see some differences with the default HTML settings right away. The font family is different, and the color of the text is slightly different as well (the black is less opaque and intense). 
When inspecting the element, we see some styles defined by bootstrap have been applied.

### The bootstrap grid system

```html
<div class="container">
	<div class="row">
		<div class="col-md-4">Col 1</div>
		...
	</div>
</div>
```

This is a snippet of html code complying to bootstrap's grid system structure. 
We can see it does impose a bit of structure to our html code, but it is a structure we would need to write ourselves most of the time anyways.

First of all, a bootstrap grid must always be inside a `container` class element, or `container-fluid` class element. 
The `container-fluid` class stretches the grid to the full width of the browser, and adds consistent padding to the grid's content (15px by default). 
The `container` class provides a fixed width that is still responsive to the width of the browser : it has a certain width for a breakpoint, another width for another breakpoint, etc.
The content inside a `container`/`container-fluid` element isn't necessarily a grid, but the grid **must be placed inside a container element**.

The `row` class creates horizontal groups of columns, which means that the columns collapse and interact with each other as a group.
It also creates a negative margin to counteract the padding that the `container` class sets ups. This is done because each column has it's own padding, and if we didn't add negative margins and padding, the individual padding of the columns would add itself to the padding of the container.

Every bootstrap `col` class (column) is defined using this template : 
`col-SIZE-SPAN`
The SIZE is a screen width range identifier : md for medium, lg for large, etc. Columns will collapse (**stack**) below that width, unless another rule applies.
SPAN indicates how many columns the element should span, with values from 1 to 12. If we specifiy more than 12 rows to our element, the element that exceeds 12 will automatically wrap to the next line. 


