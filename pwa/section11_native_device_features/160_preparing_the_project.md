160_preparing_the_project.md



we add a video and a canvas to take a picture

a image uploader in case we dont have camera device

index.html

```html
<div id="create-post">

  <video id="player" autoplay></video>
  <canvas id="canvas" width="320px" height="240px"></canvas>
  <button class="mdl-button mdl-js-button mdl-button--raised mdl-button--colored" id="capture-btn">Capture</button>
  <div id="pick-image">
    <h6>Pick an Image instead</h6>
    <input type="file" accept="image/*" id="image-picker">
  </div>
```


```html
<div class="input-section mdl-textfield mdl-js-textfield mdl-textfield--floating-label" id="manual-location">
  <input class="mdl-textfield__input" type="text" id="location">
  <label class="mdl-textfield__label" for="location" name="location">Location</label>
</div>

<div class="input-section">
  <button class="mdl-button mdl-js-button mdl-button mdl-button--colored" type="button" id="location-btn">
    Get location
  </button>
  <div class="mdl-spinner mdl-js-spinner is-active" id="location-loader"></div>
</div>
```


sw, change our CACHE_STATIC_NAME

then we can reload the page, install the sw
close the page, open it again, we activate the new sw

in feed.css

top: 56px --> 0px

```css
  min-height: calc(100vh - 56px);
```

and then

```css
  top: 56px;
```

we are showing all the elements in html, not good
lets add some classes in css



```css
#create-post video, #create-post canvas {
  width: 512px;
  max-width: 100%;
  display: none;
  margin: auto;
}

#create-post #pick-image {
  display: none;
}

#create-post #capture-btn {
  margin: 10px auto;
}

.mdl-spinner {
  margin: auto;
}
```












































