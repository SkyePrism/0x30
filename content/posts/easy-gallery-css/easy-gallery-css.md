---
title: "How To Make a Cool Gallery in CSS easily!"
date: 2025-05-07T01:00:00
draft: false
toc: true
images:
tags:
    - tutorial
---

If you want a very easy very quick copy paste gallery that will display all your images using only css, 
this is the guide for you! It couldn't be more simple than this.

```css
.gallery {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
  padding: 10px;
}

.gallery > img {
  height: 200px;
  width: auto;
  object-fit: cover; 
  border: thick solid black;
}

@media (max-width: 800px) {
  .gallery {
    flex-direction: column;
    align-items: center;
  }

  .gallery > img {
    width: 100%;
    max-width: 90vw;
    height: auto;
    object-fit: contain;
  }
}
```

Simply plop this css snipped into your css file, and define your gallery like so:

```html
<div class="gallery">
  <img src="/image1.png">
  <img src="/image2.png">
</div>
```

In HTML!

## End result:

<div class="gallery">
  <img src="/images/image1.png">
  <img src="/images/image2.png">
  <img src="/images/image3.png">
  <img src="/images/image4.png">
  <img src="/images/image5.png">
  <img src="/images/image6.png">
  <img src="/images/image7.png">
  <img src="/images/image8.jpg">
  <img src="/images/image9.jpg">
  <img src="/images/image10.png">
  <img src="/images/image11.jpg">
  <img src="/images/image12.webp">
</div>

