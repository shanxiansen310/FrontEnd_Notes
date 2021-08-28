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

















