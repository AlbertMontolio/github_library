116_css_media_queries.md

feed.css

```css
.shared-moment-card.mdl-card {
  margin: 10px auto;
  width: 400px; <!-----
}
```

always 400px in all devices

solution

we can use a % value

33%, but its ugly in small devices

width: 270px; this looks good in all small devices

```css
.shared-moment-card.mdl-card {
  margin: 10px auto;
  width: 270px;
}

@media (min-width: 600px) {
  .shared-moment-card.mdl-card {
    width: 400px;
  }
}
```



Do you want to see more of the images in your post? I deliberately chose a small height so that more posts fit onto the starting screen.

You can easily change the look of the posts with these two tweaks:

a) Change the position of the image

To see the bottom part of the image (opposed to the top part which is shown in the course), you can adjust the styling like this (in the feed.js  file => createCard()  method):

cardTitle.style.backgroundPosition = 'bottom'; // Or try 'center'
Alternatively, you can of course define a CSS class (e.g. in feed.css  file) and assign that class to a newly created card:

cardTitle.classList.add('my-created-css-class');

b) To increase the overall post height, you could alter the media queries like this:

.shared-moment-card .mdl-card__title {
  height: 250px;
}

@media (min-height: 700px) {
  .shared-moment-card .mdl-card__title {
    height: 300px;
  }
}

@media (min-height: 1000px) {
  .shared-moment-card .mdl-card__title {
    height: 380px;
  }
}
Of course, pick your favorite height .







