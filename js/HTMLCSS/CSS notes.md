**References**

[CSS Tutorial (w3schools.com)](https://www.w3schools.com/css/default.asp)





### Color

[CSS Colors (w3schools.com)](https://www.w3schools.com/css/css3_colors.asp)



#### **▼transparent keyword**

The `transparent` keyword is used to make a color transparent. This is often used to make a transparent background color for an element.



Let‘s compare transparent 

 <img src="CSS notes.assets/image-20210827195302697.png" alt="image-20210827195302697" style="zoom:50%;" />

```html
<html>
<head>
<style>
body {
  background-image: url("paper.gif");
}

div.ex1 { 
  background-color: lightgreen;
  border: 2px solid black;
  padding: 15px;
}

div.ex2 { 
  background-color: transparent;
  border: 2px solid black;
  padding: 15px;
} 
</style>
</head>
<body>

<h2>Transparent Background</h2>

<div class="ex1">This div has a light green background.</div>
<br>
<div class="ex2">This div has a transparent background.</div>

</body>
</html>
```



<div style="background-color: #fff7d3; border-left: 10px solid #ffe564;padding: 10px;">    <h5>注意⚠️</h5>    <span>The `transparent` keyword is equivalent to rgba(0,0,0,0). RGBA color values are an extension of RGB color values with an alpha channel - which specifies the opacity for a color. </span></div>





#### ▼currentcolor keyword



The `currentcolor` keyword is like a variable that holds the current value of the color property of an element.

★ This keyword can be useful if you want a specific color to be consistent in an element or a page.



```html
<style>
div {
  color: blue;
  border: 10px solid currentcolor;
  padding: 15px;  
}
</style>

<body>

<h2>The currentcolor Keyword</h2>
<p>The currentcolor keyword refers to the current value of the color property of an element.</p>

<div>
This div element has a blue text color and a blue border.
</div>
```

 <img src="CSS notes.assets/image-20210827195917245.png" alt="image-20210827195917245" style="zoom:47%;" />







### Image



#### property



##### object-fit

控制图片的自适应效果 [object-fit - CSS: Cascading Style Sheets | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit)



The **`object-fit`** [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) property sets how the content of a [replaced element](https://developer.mozilla.org/en-US/docs/Web/CSS/Replaced_element), such as an [``](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img) or [``](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video), should be resized to fit its container.



syntax：

```css
object-fit: contain;
object-fit: cover;
object-fit: fill;
object-fit: none;
object-fit: scale-down;

/* Global values */
object-fit: inherit;
object-fit: initial;
object-fit: revert;
object-fit: revert-layer;
object-fit: unset;
```



Value

- `contain`

  The replaced content is scaled to maintain its aspect ratio while fitting within the element's content box. The entire object is made to fill the box, while preserving its aspect ratio, so the object will be ["letterboxed"](https://en.wikipedia.org/wiki/Letterboxing_(filming)) if its aspect ratio does not match the aspect ratio of the box.

- `cover`

  The replaced content is sized to maintain its aspect ratio while filling the element's entire content box. If the object's aspect ratio does not match the aspect ratio of its box, then the object will be clipped to fit.

- `fill`

  The replaced content is sized to fill the element's content box. The entire object will completely fill the box. If the object's aspect ratio does not match the aspect ratio of its box, then the object will be stretched to fit.

- `none`

  The replaced content is not resized.

- `scale-down`

  The content is sized as if `none` or `contain` were specified, whichever would result in a smaller concrete object size.



![image-20220801162031514](https://image-list-1258374833.cos.ap-chengdu.myqcloud.com/image-20220801162031514.png)

