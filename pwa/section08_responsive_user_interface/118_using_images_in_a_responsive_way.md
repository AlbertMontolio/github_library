118_using_images_in_a_responsive_way.md

picking the right image

images folder, we have more than one image

2 sol
- picture element is for chosing images and focus in a different part of the image

- srcset=""

we specify the path, space, and when to use it, pixesw

```html
<img src="/src/images/main-image.jpg"
 srcset="/src/images/main-image-lg.jpg 1200w,
         /src/images/main-image.jpg 900w,
         /src/images/main-image-sm.jpg 480w"
 alt="Explore the City"
 class="main-image">
 ```

 if you resize the screen, images are download

with caching?

if you didnt cache the image first (user visits). if you are offline, you dont have it

you could precache it statically

you dont want to cache all our images









