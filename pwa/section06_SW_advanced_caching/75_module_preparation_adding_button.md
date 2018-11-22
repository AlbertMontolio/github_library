75_module_preparation_adding_button.md


cache triggered by a user event

we will access the cache not from sw, but from inside our normal js code

typical usecase, an article to read offline

we need to turn off the dynamic cache, otherwise we cache everything

we add a new card, the shared-moments div

```html
<div class="page-content">
  <h5 class="text-center mdl-color-text--primary">Share your Moments</h5>
  <div id="shared-moments"></div>
</div>
```

we populate in feed.js

my goal is to add a btn and save this card when the user wants to do so

lets add a button



```js
cardSupportingText.textContent = 'In San Francisco';
cardSupportingText.style.textAlign = 'center';
var cardSaveButton = document.createElement('button');
cardSaveButton.textContent = 'Save';
cardSaveButton.addEventListener('click', onSaveButtonClicked);
cardSupportingText.appendChild(cardSaveButton);
```

just a reference

```js
function onSaveButtonClicked(event) {
  console.log('clicked');
}
```














