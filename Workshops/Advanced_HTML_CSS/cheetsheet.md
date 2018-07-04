We will make following website:

```
https://albertmontolio.github.io/digital_twins_html
```

# Introduction to HTML & CSS

Create two files in a folder:

- index.html
- style.css

In `index.html` we will create the structure of the web page, the skeleton.

In `style.css` we will put the style.

In your `index.html`, if you write the word "doc" and press tab, you will get the common structure of an html file (otherwise, copy this):

```html
<html>
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  
</body>
</html>
```

In the head we put the metadata, information that is not shown, but is needed from our webpage. For example, the information that google displays from our website, links from other libraries that we want to use in our website etc.

In the body we put what we want to show in our actual website, what the viewers see.

```html
<html>
<head>
  <!-- intelligence (metada-data) -->
</head>
<body>
  <!-- Stuff to display -->
</body>
</html>
```

In HTML you create elements of your page (titles, subtitles, paragrafs, etc) with tags.

There are a lot of tags, you will discover them along the course.

- h1, h2, h3, h4, h5, h6: for headers
- p: for paragraphs
- img: for images
- a: for links

There are more complex ones, like `<ul>` which stands for unordered list:

```html
<ul>
  <li> item 1 </li>
  <li> item 2 </li>
</ul>
```

## Live Code

Create h1 and h3 tags in your HTML file.

To open your first webpage in the browser, right click in your sublime file, select open in browser. In MAC you can drag and drop the google chrome icon that you have on top in your Sublime text editor.

```html
<body>
  <h1>Web Developers & Tech- Teachers</h1>
  <h3>We create Web Applications.</h3>
  <h3>We bring technology to passionate people.</h3>
  <h1>Projects</h1>
  <h1>Gallery</h1>
</body>
```

We have various h1, we have an h3. If we look at the browser, we see that we have spacing around the elements, and we didn't define it. This is because some tags have their own default spacing.

# Applying CSS

A page without style is pretty boring. Now we want to custom the style of these headers.

Let's take the following example to learn how to write CSS.

```
/* style.css */
h1 {
  color: red;
}
```

To write comments we use `/* some comment */`

To style an HTML element, we use its name tag to select it. In our case, we are selecting all the h1 of the website.

Inside the curly braces, we explain which properties we want to define, and we give a value.

In our example, we want that the color of the numerous h1 is red.

If we go to our web page, we don't see that our h1 are red. Why?

We are defining the style in the `style.css` file. The structure of the page is in the HTML file though. There we have the head tag. The brain of our web. We need to tell the head, where is this css file, so that our webpage can get this styling that we define.

```html
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <link rel="stylesheet" href="style.css">
</head>
```

Now we should how our webpage have some colors

We can play with more CSS

```css
h1 {
  color: red;
  font-size: 60px;
  font-family: arial;
  background-color: pink;
}
```

You can find in google hundres of properties that you can change in an HTML tag. It's up to you to discover more.

>Remember, you select the HTML tag, you define the CSS-property and you assign a value.

# Classes and Ids

# Select one HTML element with an ID.

We can now put some colors in the elements of the HTML file, but we are styling all the h1. 

What if I just want to style the first one?

I need to select the first h1 somehow. That's why we use Ids: to select an HTML element. Once selected, we can give the style in the CSS file, just to this specific element.

To label an HTML element with an ID, you should use this syntax:

```html
<h1 id="title-banner">Web Developers & Tech- Teachers</h1>
```

That's the syntax, the keyword id and the name that you want to give, or the label.

Now in CSS we can point to this element. We can apply style just to this element.

```css
#title-banner {
  color: orange;
}
```

Now we know how to select one element. But what happens, if we have two titles that have the same style? Like Projects and Gallery

# Select group of elements with same style: classes.

To apply one style to multiple HTML elements we use classes.

First we select the elements that we want to style.

```html
<h1 class="secondary-headers">Projects</h1>
<h1 class="secondary-headers">Gallery</h1>
```

Now we want to select two elements, so the two elements will have the same class, "secondary-headers"

Let's apply some styling to this class.

```css
.secondary-headers {
  color: blue;
}
```

>You can select other colors in the browser.
Right click, inspect, and in the small color square, click and play with the colors

Awesome, now we can apply styles to multiple elements and to unique elements.

# The Box Model: Everything inside boxes!

We know that HTML is the structure. To position elements in the file, to put the required spaces between elements, best way is to inlcude the elements inside boxe or containers, called `div`s. 

As we have learnt, we can select these containers, whether with an id-name or with a class-name.

Let's create the structure of this first part of the web page, the banner!

```html
<body>

  <div class="title-wrapper">
    <h1 id="title-banner">Web Developers & Tech- Teachers</h1>
    <h3>We create Web Applications</h3>
    <h3>We bring technology to passionate people.</h3>
  </div>

</body>
```

In the website nothing changed, but if we inspect our webpage in the browser, we see our new brand container.

Let's do the same for the top-navbar. Let's structure it with `div`s.

```html
  <div class="top-navbar">
    <div class="company-name">
      DIGITAL TWINS
    </div>
    <ul>
      <li>COURSES</li>
      <li>ABOUT US</li>
      <li>CONTACT</li>
    </ul>
  </div>
```

We are almost done with the banner. We are missing though the big `div`, which is including everything.

```html
  <div class="top-navbar">
    <!- [... insert top-navbar code ...] -->
  </div>

  <div class="banner">

    <div class="title-wrapper">
      <!- [... insert title-wrapper code ...] -->
    </div>

  </div>
```

Now we have a nice structure. But things are not in their place. We want to position the elements in our webpage.

To position the `div`s in our Web Page, we use two technologies:

- The Flexbox technology
- The position relative, absolute pattern

# The Flexbox technology

Flexbox is absolutely amazing. It helps us to position elements in our website. There is a lot of documentation on the internet. The best resource by fare is this one:

```html
https://css-tricks.com/snippets/css/a-guide-to-flexbox/
```

Let's speack about the basics of Flexbox. You always have a big container, which includes some elements inside, that we want to position.

In our banner, we want to position the title in the center of the screen. The banner would be the big container, and we will position the title, relative to the container.

When you want to position stuff in your webpage, you can use this trick:

>Put background colors in every element to see them clearly, otherwise they are all white and you don't see their positions.

Let's paint the background for the `banner`:

```css
.banner {
  background-color: pink;
}
```

and now let's paint our `title-wrapper`.

```css
.title-wrapper {
  background-color: green;
}
```

Thanks to this strategy, we see that the `title-wrapper` ocuppies all the `banner`, that means, that the `banner` is to small. We want the `banner` to occupy all the screen, right?

css code!

```css
.banner {
  background-color: pink;
  height: 100%;
  width: 100%;
}

Now our banner occupies all the screen.
Let's use the technology flex. If we use `display-flex` in the banner, that would act as the big container. All the elements inside will follow the flex box model.

```css
.banner {
  [...]
  display: flex;
}
```

Elements are aligned, that's the first property in the flex box model.
Now we can use the property `justify-content`, to align the items horizontally.

### here

```css
.banner {
  background-color: pink;
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: center;
}
```


we justify horizontally: we can use flex-start, flex-end

vertically?

```css
.banner {
  /* [...] */
  align-items: center;
}
```

# Space between elements, padding and margins

We can now positionaed what we want wherever we want. Some times we want more space between elements. 

This box model, you can put sapce inside the box, or you can move the boxes away from other boxes.

- padding: you put space inside the box

```css
.title-wrapper {
  padding: 20px;
  background-color: green;
}
```

or, we want to separate an element from the elements around him. let's separete the h1 from the other h3

first, we select the element, wheather with ids or classes. in this case, it has an id.

```html
<h1 id="title-banner">Web Developers & Tech- Teachers</h1>
```

```css
#title-banner {
  background-color: yellow;
  margin-bottom: 50px;
}
```

now, we are not putting space inside the element, but outside. there is no new space in the yellow box.

we are pushing elements aways.



now that you know about margins, what is going one with this white border? every html has this by default. look that the body engloves everything.

we need to kill it.

so, 

```css
body {
  margin: 0px;
}
```


# navbar, it's above. we want the navbar on top

we need to learn another way of positioning

# position: relative and position: absolute

we have the same situation, a big div, which contains elements inside that we want to positionated.

let's make a quick example

let's create the test element inside the banner

```html
  <div class="banner">

    <div id="test-positioning">
      albert
    </div>

    <!-- [...] -->
  </div>
```

in css

```css
.banner {
  /* [...] */

  position: relative;
}
```


```css
#test-positioning {
  position: absolute;
  top: 100px;
  right: 100px;
}
```

we pin the main container, we say, hej, you are fixed, you are the reference. you stay where you are. and the element that has the position abasolute, we can move it inside.

IMPORTANT. the element that has the position absolute, occupies no space.

that's key

let's do the same with our navbar

```css
body {
  position: relative;
}

.top-navbar {
  position: absolute;
  top: 0px;
  left: 0px;
}
```

we inspect, and it's there! but behind.

we just need to tell, hej, we want it above.

in CSS there is the z-index.

you order stuff in this axis

```css
.top-navbar {
  position: absolute;
  top: 0px;
  left: 0px;
  z-index: 2;
}
```

we are almost done with the structure. 

we need to finish the navbar

we want to align again the items of the navbar. they are in a ul. that would be the big container. inside there are li, we could use flexbox...

but, we will use a new concept.

it's time to introduce bootstrap.

bootstrap is a CSS-library. that means, you use things that are already made for you.

bootstrap it's just a huge CSS file, with a lot of classes defined.
you don't define the classes, you just use them

first of all, we need to have this huge file. how?

we get it from the internet. we just need the link, and put it in our head of the html file. the brain of our web


https://getbootstrap.com/docs/4.0/getting-started/introduction/

```html
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
```

copy this in your head tag

the letters change a little bit. cool, signal that bootstrap is there.

now we can use stuff from them


list linline

```html
<ul class="list-inline">
  <li class="list-inline-item">Lorem ipsum</li>
  <li class="list-inline-item">Phasellus iaculis</li>
  <li class="list-inline-item">Nulla volutpat</li>
</ul>
```


now we want to positionate the elements. we know how to do that. we have the container, the topnavbar, and two elements inside, the company-name and the list

let's put first some colors, to see, and make the topnavbar with 100%.



```css
.top-navbar {
  /* [...] */
  width: 100%;
  background-color: yellow;
}

.company-name {
  background-color: blue;
}

#navbar-items {
  background-color: red
}
```

you see the structure

let's use a new property of flex, instead of justify-content: center

we have:

```css
.top-navbar {
  /* [...] */
  display: flex;
  justify-content: space-between;
}
```

it separates the elements

we have a small issue. bootstrap puts some margin-bottom in the ul, because he wants to do that. we don't, we kill it

```css
#navbar-items {
  background-color: red;
  margin-bottom: 0px;
}
```

let's put some spacing inside the topnavbar

```css
.top-navbar {
  /* [...] */
  padding: 40px 80px;
}
```

buala! the structure is beautiful

let's take out some ugly colors

# Video in the background. 

let's add the magic

now we are masters in positioning and styling, let's add the magic, a youtube video that will play automatically, loop automatically etc.

where do we put it? exactly. like the topnavbar, we want to put it in a different layer. we want to put it behinde the banner, behind the topnavbar


how?

let's put our first youtube video first in our page

you go to youtube, you search for a video

you go to share

you click embeded, it gives you the code

copy

we put it as the first element in our body

```html
<iframe width="560" height="315" src="https://www.youtube.com/embed/LAySCljzpAE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
```

that's it. we have a nice video. it could be from us. we have to play the video, it's boring. and the video does not loop when it's done. and we listen to the sound.

you can pass some settings to achieve that

```html

<iframe id="bannerVideo" width="560" height="315" src="https://www.youtube.com/embed/LAySCljzpAE?loop=1&autoplay=1&playlist=LAySCljzpAE&showinfo=0&controls=0&start=4&rel=0&mute=1" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen class="banner-video"></iframe>
```

?loop=1&
autoplay=1&
playlist=LAySCljzpAE&
showinfo=0&
controls=0&
start=4&
rel=0&
mute=1

i looked them up for you :)


we want the video to be behind and occupy all the screen

let's give him an id and put some style


```css
#bannerVideo {
  width: 100%;
  height: 100%;
}

```

nice! almost there! now, if we want to put stuff, that does not affect the space,

we use... position: absolute!

```css
#bannerVideo {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0px;
  left: 0px;
}
```

almost there!

let's put white letters

in the navbar and title with the color: white




```css
#title-banner {
  color: white;
}

.secondary-headers {
  color: white;
}
```

```css
#navbar-items li {
  color: white;
}
```

TRICK, you don't need to assign classes for everything that you want to select.
css has this syntax

almost done!! we can not see the white letters clearly

let's add a layer, that brings opacity

# LAYER

you know how, we create a big div that occupies all the screen, and we put there the opacity effect in the css style.



```html
  <div class="video-layer">
    
  </div>
```

```css
.video-layer {
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
  height: 100%;
  background-color: orange;
}
```

we don't see the background

remember, we have to tell in which position in the z-index

it must be above the video, but below the letters

```css
.banner {
  /* [...] */
  z-index: 2;
}
```

```css
.top-navbar {
  /* [...] */
  z-index: 2;
}
```

and the video is in 0, so, the layer should b in the middle, in position 1!

awesome! problem is, now we don't see the video, we don't want a background-color orange

we want it transparent, but with a filter. filters there are a lot, take this one


```css
.video-layer {
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
  height: 100%;
  background-image: linear-gradient( -225deg, rgba(16, 33, 43, 0.8) 0%, rgba(24, 43, 56, 0.6) 50% );
  z-index: 1;
}
```


we are done!!!!!


# Bootstrap

Change the path to your images

```html
<div class="container my-container">
    <div class="row my-row">
      <div class="col-xs-12 col-sm-6 col-md-4 item1 item">

        <div class="card">
          <img class="card-img-top" src="images/bosque.jpeg" alt="Card image cap">
          <div class="card-body">
            <h5 class="card-title">HTML & CSS Workshop</h5>
            <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
            <a href="#" class="btn btn-primary">Go somewhere</a>
          </div>
        </div>

      </div>
      <div class="col-xs-12 col-sm-6 col-md-4 item2 item">

        <div class="card">
          <img class="card-img-top" src="images/bosque.jpeg" alt="Card image cap">
          <div class="card-body">
            <h5 class="card-title">HTML & CSS Workshop</h5>
            <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
            <a href="#" class="btn btn-primary">Go somewhere</a>
          </div>
        </div>

      </div>
      <div class="col-xs-12 col-sm-6 col-md-4 item3 item">

        <div class="card">
          <img class="card-img-top" src="images/costa.jpeg" alt="Card image cap">
          <div class="card-body">
            <h5 class="card-title">HTML & CSS Workshop</h5>
            <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
            <a href="#" class="btn btn-primary">Go somewhere</a>
          </div>
        </div>

      </div>
    </div>
  </div>

```

In bootstrap, look for a modal and copy paste it in your code.

don't forget the javascript files.

```html
<div class="subscribe-wrapper">
  <button id="subscribe-btn" type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal" data-whatever="@mdo">Open modal for @mdo</button>
</div>


<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">New message</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <form>
          <div class="form-group">
            <label for="recipient-name" class="col-form-label">Recipient:</label>
            <input type="text" class="form-control" id="recipient-name">
          </div>
          <div class="form-group">
            <label for="message-text" class="col-form-label">Message:</label>
            <textarea class="form-control" id="message-text"></textarea>
          </div>
        </form>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Send message</button>
      </div>
    </div>
  </div>
</div>
```

# Fontawesome

find the best icons

https://fontawesome.com/icons?from=io

Get started / copy the CDN-url

use icons!


# Media queries

Select a break point in your size, and change the classes!

```css
@media only screen and (max-width: 450px) {
  .top-navbar {
    padding: 40px 15px;
  }
  .top-navbar .name { font-size: 14px; }
  .top-navbar .item { font-size: 14px; }

  .title-wrapper {
    padding: 10px;
  }

  .title-wrapper h1 {
    font-size: 40px;
    color: white;
  }
  .title-wrapper h3 {
    font-size: 20px;
    color: white;
  }
  .gallery {
    width: 100%;
  }

  .subscribe-wrapper {
    padding: 200px 20px;
  }

}
```





# Github pages

a free service to host your webpage.

First you need to create a webpage.

If you have a Github account,
type in your folder:

```
git init
ga .
gc -m "init"
hub create
git checkout -b gh-pages
git push origin gh-pages
```


```
git init
```

Initiate your git project

```
ga .
```

Track all the files in your project

```
gc- m "init"
```

Make a snapshot of the status of your file

```
hub create
```

Connect your local repository to your github account

```
git checkout -b gh-pages
```

Create a gh-pages branch. It has to be this name. It's the branch, that github uses to host your webpage

```
git push origin gh-pages
```

Push your code into this branch.

































































































































