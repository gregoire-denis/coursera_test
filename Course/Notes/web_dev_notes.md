#Coursera Web Dev course Notes

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

No space is allowed between the opening bracket and the tag name, and between the opening bracket and the forward slash of the closing tag : `< p>` and `< /p>` are invalid.
However you **must** have a space between the tag itself and any of its attributes : `<pid="myId">` is invalid.
Any and all others spaces are ignored : `<p id    =    "myId">` is valid and is read as `<p id="myId">