#Coursera Web Dev course Notes

##HTML Basics

###Anatomy of an HTML Tag

Tags are at the core of HTML.
They usually have an opening and a closing tag, which surround some content.

```html
<p>content</p>
```
In this case, the `<p>` tag, which stands for paragraph, is communicating to us that the content it surrounds should be treated as a paragraph.
Most opening tags have a matching closing tag (here `</p>`), but not all.
For example, the `<br>` and `<hr>` tags, which stand for line break and horizontal rule, have no closing tags, they are standalone.
Most tags can have a predefined **attribute**. Attribute is a **name/value pair** that serves as meta data about the element itself. 

```html
<p id="myId">content<p>
```
Here we define `myId` as the value of the `id` attribute.
No other element in the HTMl document is allowed to have the same value associated to it's `id` attribute. When this rule is not respected, it results in invalid HTML, which can potentially break some features.
Attributes can only be specified on the opening tag.
Technically, enclosing the value of an attribute in quotes `""` is not required. It is nevertheless best practice to do so. It doesnt matter what quotes are used, single or double, as long as they match each other : `<p id = 'myId">`.


No space is allowed between the opening bracket and the tag name, and between the opening bracket and the forward slash of the closing tag : `< p>` and `< /p>` are invalid.
However you **must** have a space between the tag itself and any of its attributes : `<pid="myId">` is invalid.
Any and all others spaces are ignored : `<p id    =    "myId">` is valid and is read as `<p id="myId">`.

Self closing tags are a XML shorthand way to write a tag that doesn't contain any content, as a placeholder for content we would add dynamically for an example : `</p>`
This is **forbidden** in HTML5 ! If a tag could contain content but doesn't, it must be written normally with no content between the tags : `<p></p>` 

###Basic HTML document structure

All HTML documents should start with the declaration `<!doctype html>`. This can be written in either lower or uppercase.
This declaration tells the browser that it should get ready to render HTML. It is however, mostly historical (now HTML is pretty much the only language used for web documents), but not specifying it can lead to dysfunctionality, as the browser will enter **quirks** mode to render the document.

Next comes the `<html>` tag, which encapsulates all the document.

Then the `<head>` tag,  which describes the content of the page : it's title, what character encoding is being used, and whatever resources are needed to render the page properly.
A good example is setting the character encoding with a `<meta>` tag : `<meta charset="utf-8">`. This tag is a standalone tag.

After comes the `<body>` tag, which contains all content visible to the user. It is often referred to as a viewport

We are nesting tags within one another. You have to close the last openend tag before you close its parent tag. If not, the document is invalid.
The browser interprets the document from top to bottom, until it hits the last closing `</html>` tag.

###HTML content models

The term content model refers to the behaviour the browser applies to the elements belonging to that content model, and to the nesting rules of those elements : which elements are allowed to be nested within others.
Prior to HTML5 specifications, HTML elements were either block level elements, or inline level elements.
Block level elements render to begin on a new line by default, and are allowed to contain inline elements, or other block level elements. This is in contrast to inline elements, which render on the same line by default, and may only contain other inline elements. 
While HTML5 replaces these with more complex content categories, this distinction remains practical because it aligns well with existing CSS rules. 
Block-level roughly translates to Flow Content category in HMTL5, and inline to Phrasing Content.

##Essential HTML tags

###Heading

A semantic HMTL is an element that implies some meaning to it's content. 
Heading tags are semantic tags, because they have a hierarchy to them : `<h1>` is above `<h2>` in terms of importance. 
In strict HTML, they will be stylised to mirror that hierarchy. However, it is never recommended to use this hierarchy to apply styles to their content. This should always be done via CSS.
Nonetheless, using proper heading tags in order, instead of using divs is best practice, because it indicates to both the browser and potential readers of the code the meaning and the structure of the content at a glance.

`<header>` contains header information about the page: company logo, some tag line, sometimes navigation.
`<nav>` stands for navigation : usually contains links to different parts of the website.
`<section>`, `<article>` are usually related to one another : it is recommended (but no a hard rule) to nest article tags inside section tags.
`<aside>` carries some information that relates to the main content, but not directly (i.e related posts)
`<footer>` contains the footer information

These are all block-level elements, so why not use divs ? Because it is so much easier to comprehend the structure of the page at a glance with proper semantic elements. 

Well chosen content of h1 element is crucial to S.E.O (search engine optimization).

###Lists

Lists are a useful HTML structure that allows to group related content.
`<ul>` stands for unordered list, and will contain `<li>` list elements in an unordered fashion.
`<ol>` stands for an ordered list, and will contain `<li>` list elements in an ordered fashion.
Lists can be nested within one another : a `<li>` element can contain either and `<ul>` or `<ol>` element.

###HTML Character entity references

Since HTML uses certain character for syntax, we need to differentiate between such characters when they are used as HTML syntax or content.
If we want the browser to render such characters as content, we need a way to **escape** them, to tell the browser NOT to interpret these as HTML.

There are three characters that should ALWAYS be escaped to avoid rendering issues : the `<` and `>` characters ,and `&`.
`<` is escaped using `&lt;`, `>` using `&gt;` , `&` using `&amp;`.

A common entity is also the copyright symbol, because it isn't easily found on most keyboards, and can be rendered using `&copy;`

Another common, but largely misused entity is the non breaking space entity, used to indicate wether or not a group of content should wrap as one single string : `&nbsp;`
This entity should NEVER be used to manipulate spaces between content, as this can be done using margins and spans. 

Entities can also be used to avoid rendering issues depending on character encoding, i.e when trying to write an HTML based email : `&quot;` for quotation marks, for an example, which may not render the same depending on encoding. 

 



