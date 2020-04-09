---
layout: post
title: "[CSS] 기본 틀(Media query, 보일러 플레이트)"
categories: CSS
---

main.css
```css
/* 보일러 플레이트 */
/** * Eric Meyer's Reset CSS v2.0 (http://meyerweb.com/eric/tools/css/reset/) * http://cssreset.com */
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure,
 footer, header, hgroup, menu, nav, section {
	display: block;
}
body {
  line-height: 1;
  background-color: #F6F6F6;
}
ol, ul {
	list-style: none;
}
blockquote, q {
	quotes: none;
}
blockquote:before, blockquote:after,q:before, q:after {
	content: '';
	content: none;
}
table {
	border-collapse: collapse;
	border-spacing: 0;
}
/* 보일러 플레이트 끝 */

/* 
  ##Device = Desktops
  ##Screen = 1281px to higher resolution desktops
*/

@media (min-width: 1281px) {
  
    .test {
        height: 100%;
        width: 100%;
        background-color: red;
    }

  }
  
  /* 
    ##Device = Laptops, Desktops
    ##Screen = B/w 1025px to 1280px
  */
  @media (min-width: 1025px) and (max-width: 1280px) {
    
    .test {
        height: 100%;
        width: 100%;
        background-color: orange;
    }
    
  }
  
  /* 
    ##Device = Tablets, Ipads (portrait)
    ##Screen = B/w 768px to 1024px
  */
  @media (min-width: 768px) and (max-width: 1024px) {
    
    .test {
        height: 100%;
        width: 100%;
        background-color: yellow;
    }
    
  }
  
  /* 
    ##Device = Tablets, Ipads (landscape)
    ##Screen = B/w 768px to 1024px
  */
  @media (min-width: 768px) and (max-width: 1024px) and (orientation: landscape) {
    
    .test {
        height: 100%;
        width: 100%;
        background-color: greenyellow;
    }
    
  }
  
  /* 
    ##Device = Low Resolution Tablets, Mobiles (Landscape)
    ##Screen = B/w 481px to 767px
  */
  @media (min-width: 481px) and (max-width: 767px) {
    
    .test {
        height: 100%;
        width: 100%;
        background-color: blue;
    }
    
  }
  
  /* 
    ##Device = Most of the Smartphones Mobiles (Portrait)
    ##Screen = B/w 320px to 479px
  */
  @media (min-width: 320px) and (max-width: 480px) {
    
    .test {
        height: 100%;
        width: 100%;
        background-color: purple;
    }
    
  }
```
   
index.html
```html
<!DOCTYPE html>
<html>

<head>
    <link rel="stylesheet" href="./main.css">
</head>

<body>
    <div class="test">adad</div>
</body>

</html>
```