# Section 3

its best to start with a blank project!


library page

quotes page

favorites page

modal page

settings page

ionic generate page library
quotes, quote, favorites, settings

delete home folder, break app, we will fix it

in app.component.ts

delete the import for home

```js
import { HomePage } from '../pages/home/home';
```


```js
rootPage:any = FavoritesPage;
```

```js
import { FavoritesPage } from '../pages/favorites/favorites';
```

we need to add FavoritesPage in app.module.ts

# 42. Multiple stack vs one single stack

one Navigation Stack vs Multiple stacks

inside the stack you can split it up

tabs, two elements. 

when you click favorites, you load a sub stack



let's add tabs

are added as a separate pages

new directory in the pages folder

tabs.ts

```js
import { Component } from "@angular/core";
import { FavoritesPage } from "../favorites/favorites";
import { LibraryPage } from "../library/library";

@Component({
    selector: 'page-tabs',
    template: `
        <ion-tabs>
            <ion-tab [root]="favoritesPage" tabTitle="Favorites" tabIcon="star"></ion-tab>
            <ion-tab [root]="libraryPage" tabTitle="Library" tabIcon="book"></ion-tab>
        </ion-tabs>
    `
})

export class TabsPage {
    favoritesPage = FavoritesPage;
    libraryPage = LibraryPage;
}
```

in app.component.ts

```js
  rootPage:any = TabsPage;
```

app.module.ts!
add it there


the tabs, they occupy just a smallportion of the page, in the free page, you load the page that you want

it always loads the first tab

you could change this

```html
selectedIndex="1" in <ion-tabs>
```

# 44. Adding quotes DataCue

we have a file with quotes

create new folder in src, data

quotes.ts

we need to insert data in our favorites component

library component actually

we want to load all quotes, display them, and favorite the quotes

importing the quotes

we can define interfaces, define the types explicityl, because we know how the data is

in data, create file

quote.interface.ts

```js
export interface Quote {
    id: string;
    person: string;
    text: string
}
```

```js
import { Component } from '@angular/core';
import { Quote } from '../../data/quote.interface';

@Component({
  selector: 'page-library',
  templateUrl: 'library.html',
})
export class LibraryPage {
  quoteCollection: {category: string, quotes: Quote[], icon: string}
}
```

library.html

```html

<ion-content padding>
  <h3 text-center>Select your favorite Quote</h3>
  <ion-list>
    <button ion-item *ngFor="let quoteGroup of quoteCollection">
      Quote
    </button>
  </ion-list>
</ion-content>
```


item-left: positio yourself to the left, if you are inside an ion-item

click the icons! go to the quotes page, foor the cosen category

# 48. passing the quotes data between pages

quotes page!

quotes.ts

```html
<ion-content padding>
  <h3 text-center>Select your favorite Quote</h3>
  <ion-list>
    <button 
      ion-item 
      *ngFor="let quoteGroup of quoteCollection"
      [navPush]="quotesPage"
    >
      <ion-icon [name]="quoteGroup.icon" item-left></ion-icon>
      <h2>{{ quoteGroup.category | uppercase }}</h2>
      <p>{{ quoteGroup.quotes.length }} Quotes</p>
    </button>
  </ion-list>
</ion-content>
```

library.ts

```js
import { Component, OnInit } from '@angular/core';
import { Quote } from '../../data/quote.interface';
import quotes from '../../data/quotes';
import { QuotesPage } from '../quotes/quotes';

@Component({
  selector: 'page-library',
  templateUrl: 'library.html',
})
export class LibraryPage implements OnInit {
  quoteCollection: {category: string, quotes: Quote[], icon: string}[] // an array with this type of object
  quotesPage = QuotesPage;

  ngOnInit() {
    this.quoteCollection = quotes; // in the data / quotes file
  }
}
```

```js
    <button 
      ion-item 
      *ngFor="let quoteGroup of quoteCollection"
      [navPush]="quotesPage" [navParams]="quoteGroup"
    >
      <ion-icon [name]="quoteGroup.icon" item-left></ion-icon>
      <h2>{{ quoteGroup.category | uppercase }}</h2>
      <p>{{ quoteGroup.quotes.length }} Quotes</p>
    </button>
```


quotes.ts

```js
import { Component } from '@angular/core';
import { NavParams } from 'ionic-angular';
import { Quote } from '../../data/quote.interface';

@Component({
  selector: 'page-quotes',
  templateUrl: 'quotes.html',
})
export class QuotesPage {
  quoteGroup: {category: string, quotes: Quote[], icon: string}

  constructor (private navParams: NavParams) {}

  ionViewDidLoad() {
    this.quoteGroup = this.navParams.data;
  }
}
```


```html
<ion-header>
  <ion-navbar>
    <ion-title>{{ quoteGroup.category | uppercase }}</ion-title>
  </ion-navbar>
</ion-header>


<ion-content padding>

</ion-content>
```


error.
we can do 
the template gets created before we reach ionViewDidLoad()

```html
<ion-title>{{ quoteGroup?.category | uppercase }}</ion-title>
```

# 49. Cards

have a look at cards

```html
<ion-content padding>
  <ion-card *ngFor="let quote of quoteGroup.quotes; let i = index">
    <ion-card-header>
      #{{ i + 1 }}
    </ion-card-header>
    <ion-card-content>
        <p>{{ quote.text }}</p>
        <p>{{ quote.person }}</p>
    </ion-card-content>    
  </ion-card>
</ion-content>
```

# The grid!

```html
<ion-content padding>
  <ion-card *ngFor="let quote of quoteGroup.quotes; let i = index">
    <ion-card-header>
      #{{ i + 1 }}
    </ion-card-header>
    <ion-card-content>
        <p>{{ quote.text }}</p>
        <p>{{ quote.person }}</p>
    </ion-card-content>
    
    <ion-row>
      <ion-col text-right>
        <button ion-button clear small
          (click)="onAddToFavorite(quote)"
        >
        Favorite</button>
      </ion-col>
    </ion-row> 

  </ion-card>
</ion-content>
```


quotes.scss


```html
 <p class="author">{{ quote.person }}</p>
```


```css
 page-quotes {
    .author {
        color: #ccc;
        text-align: right;
        text-transform: uppercase;
        margin: 0px;
        padding: 0px;
        &:before {
            content: '- ';
        }
    }
}
```

# 52. adding alerts to the application

we want to display an alert

to implement an alert, we need to inject

```js
  constructor (
    private navParams: NavParams,
    private alertCtrl: AlertController
  ) {}
```

```js
onAddToFavorite(selectedQuote: Quote) {
    // show an alert
    const alert = this.alertCtrl.create({
      title: 'Add Quote',
      subTitle: 'Are you sure?',
      message: 'Are you sure you want to add the quote?',
      buttons: [
        {
          text: 'Yes, go ahead',
          handler: () => {
            console.log('Ok');
          }
        },
        {
          text: 'No, I changed my mind!',
          role: 'cancel',
          handler: () => {
            console.log('Cancelled!');
          }
        }
      ]
    });

    alert.present();
  }
```

uoooo

manage state of our quotes, to determine if our quote is a fav or not
we need to store the quote as af fave!


# 53. Working with Angular 2 Services in the Ionic 2 App

a place to state the state of our app, of our quote, to see which ones are faves

we use a service!

in the src, folder services

quotes.ts

```js
import { Quote } from "../data/quote.interface";

export class QuotesService {
    private favoriteQuotes: Quote[] = [];

    addQuoteToFavorites(quote: Quote) {
        this.favoriteQuotes.push(quote);
    }

    removeQuoteFromFavorites(quote: Quote) {
        const position = this.favoriteQuotes.findIndex((quoteEl: Quote) => {
            return quoteEl.id == quote.id;
        });
        this.favoriteQuotes.splice(position, 1);
    }

    getFavoriteQuotes() {
        return this.favoriteQuotes.slice();
        // create a copy of the array
        // to avoid favorieQuotes to be edited fromt he outside
    }
}
```

# 54.Marking Quotes as Favorites by using a Service

we want to use this service, to add quotes to our list of favorite quotes

we need to provide a service we want to use

in order to provide, app.module.ts

providers

```js
  providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler},
    QuotesService
  ]
```

```js
import { QuotesService } from '../services/quotes';
```

in the quotes.ts, in the handler, when we click i want to add this quote

i want to access my service

quotes/quotes.ts

inject the service

```js
  constructor (
    private navParams: NavParams,
    private alertCtrl: AlertController,
    private quotesService: QuotesService
  ) {}
```


```js
import { QuotesService } from '../../services/quotes';
```

```js
      buttons: [
        {
          text: 'Yes, go ahead',
          handler: () => {
            this.quotesService.addQuoteToFavorites(selectedQuote);
            console.log('Ok');
          }
        },
```

services/quotes.ts

just to see, console.log

```js
    addQuoteToFavorites(quote: Quote) {
        this.favoriteQuotes.push(quote);
        console.log(this.favoriteQuotes);
    }
```



# 55. Peparaing the Favorite Quotes Page

output quotes on the favorites page

pages/favorites.ts

```js
import { Component } from '@angular/core';

@Component({
  selector: 'page-favorites',
  templateUrl: 'favorites.html',
})
export class FavoritesPage {

}
```

goal is to get the quotes

```js
import { Component } from '@angular/core';
import { Quote } from '../../data/quote.interface';
import { QuotesService } from '../../services/quotes';

@Component({
  selector: 'page-favorites',
  templateUrl: 'favorites.html',
})
export class FavoritesPage {
  quotes: Quote[]; // we indicate type, array of quotes
  // get them, we need the service
  constructor (private quotesService: QuotesService) {}
  // we need a place to load our favoriteQuotes. in the constructor, in ngOnInit

  // always will be executed, even if the page was cached,
  // executed right before it's dispalyed
  ionViewWillEnter() {
    // 
    this.quotes = this.quotesService.getFavoriteQuotes();
  }
  
}
```

now we want to output them in the template

# 56. Diving Deeper into List Items

favorites.html


```html
<ion-header>

  <ion-navbar>
    <ion-title>Favorites Quotes</ion-title>
  </ion-navbar>

</ion-header>


<ion-content padding>
  <ion-list>
    <ion-item *ngFor="let quote of quotes">
      <h2>{{ quote.person }}</h2>
      <p>{{ quote.text }}</p>
    </ion-item>
  </ion-list>
</ion-content>
```


we want a model when we tip a favQuote

#57. Preparing a Modal Page

a modal is an overlay over the current page, it uses a normal ionic2 page for this, but its not placed in the stack of pages
we are not pushing it in the stack of pages, its just an overlay

quote.html

```html
<ion-content padding text-center>
  <ion-card>
    <ion-card-content>
      TEXT
    </ion-card-content>
    <ion-row>
      <ion-col>
        <button ion-button small outline color="danger">Unfavorite</button>
      </ion-col>
    </ion-row>
  </ion-card>

  <button ion-button color="danger">Close</button>

</ion-content>
```

#58. Presenting a Modal

we go to favorites component, there i can click on a favQuote

first add a clicklistener to this item

favorites.html

```html
<ion-content padding>
  <ion-list>
    <ion-item *ngFor="let quote of quotes"
      (click)="onViewQuote(quote)">
      <h2>{{ quote.person }}</h2>
      <p>{{ quote.text }}</p>
    </ion-item>
  </ion-list>
</ion-content>
```


favorites.ts

we need to inject a class that gives us the modalcontroller

```js
import { Component } from '@angular/core';
import { Quote } from '../../data/quote.interface';
import { QuotesService } from '../../services/quotes';
import { ModalController } from 'ionic-angular';
import { QuotePage } from '../quote/quote';

@Component({
  selector: 'page-favorites',
  templateUrl: 'favorites.html',
})
export class FavoritesPage {
  quotes: Quote[];

  constructor (private quotesService: QuotesService, private modalCtrl: ModalController) {}
  
  ionViewWillEnter() {
    this.quotes = this.quotesService.getFavoriteQuotes();
  }

  onViewQuote(quote: Quote) {
    // here i want to open the modal
    // quotepage, the page we want to load. import it on the top
    const modal = this.modalCtrl.create(QuotePage);
    // this is the content of our modal
    modal.present();
  }
  
}
```

how to get rid of the modal?

#59. Dismissing Modals

the quote page is the one that we use as a modal

quote.ts

before,

quote.html

```html
  <button ion-button color="danger" (click)="onClose()">Close</button>
```


quote.ts

```js
import { Component } from '@angular/core';

@Component({
  selector: 'page-quote',
  templateUrl: 'quote.html',
})
export class QuotePage {

}
```


get rid of our selves

i need to disappear.we used before the navCtrl. but now not, its not a navigation stack

```js
constructor (private viewCtrl: ViewController) {}
```

controls the currently active view. the visible one! in our case, the QuotePage!

the navCtrl controls our stack of pages

```js
import { Component } from '@angular/core';
import { ViewController } from 'ionic-angular';

@Component({
  selector: 'page-quote',
  templateUrl: 'quote.html',
})
export class QuotePage {

  constructor (private viewCtrl: ViewController) {}

  onClose() {
    this.viewCtrl.dismiss();
    // deletes the page, gets rid of it
    // we have to have something instead
  }
}
```

next step, pass some data to the modal!!

# 60. Passing Data to a Modal

in the favorites page, where we view the modal, onViewQuote

favorites.ts

```js
  onViewQuote(quote: Quote) {
    const modal = this.modalCtrl.create(QuotePage, quote);
    modal.present();
  }
```

we pass it to the modal, as a second argument

in the modal, we want to access this data

we get the data through NavParams. we are not using a nav, but we use the navparams

quote.ts

```js
import { Component } from '@angular/core';
import { ViewController, NavParams } from 'ionic-angular';

@Component({
  selector: 'page-quote',
  templateUrl: 'quote.html',
})
export class QuotePage {

  constructor ( private viewCtrl: ViewController,
                private navParams: NavParams) {}

  onClose() {
    this.viewCtrl.dismiss();
  }
}
```

when should we retrieve the data?

```js
  ionViewDidLoad() {
    this.person = this.navParams.get('person');
    this.text = this.navParams.get('text');
  }
```




in quote.html, let's display the data, with interpolation



```html

<ion-navbar>
  <ion-title>{{ person }}</ion-title>
</ion-navbar>

[...]

<ion-card-content>
  {{ text }}
</ion-card-content>
```

in modals, in quote.ts, we use ionViewDidLoad

next thing to hook up, this unfavorite button

close the modal, remove the quote from our favquotes

# 61. Passing Data from a Modal back to the Page

click unfavourite

the page that we view when we close the modal, is in favorite.ts, the onViewQuote

kind of handle any data that its given us back

in the quote.ts

on the dismiss method we can pass the data


first, in the template, quote.html

let's add an evenet listener

```html
<button ion-button small outline color="danger"
        (click)="onClose(true)"
        >Unfavorite</button>
```


quote.ts

```js
  onClose(remove = false) {
    this.viewCtrl.dismiss(remove);
  }
```


in the favorites.ts

i can handle that data

how do i get this data

modal is just a view. i can execute all the methods

for example
modal.dismiss();

modal.onDidDismiss();

takes a function as an argument, which is executed whenever this view is indeed dismissed

remember, in the quote.ts, the modal, we had

```js
  onClose(remove = false) {
    this.viewCtrl.dismiss(remove);
  }
  ```

so, when the modal is dismissesd, we get the data

on favorites.ts

```js
  onViewQuote(quote: Quote) {
    const modal = this.modalCtrl.create(QuotePage, quote);
    modal.present();
    modal.onDidDismiss((remove: boolean) => {
      // to be done
    });
  }
```

hooks we can listen for on views


# 62. Understanding ViewController Hooks

we learnt the lifecicle hooks for pages,
methods we can implement on the page itselfl, ionic2 execute them whenever the page is presented, or loaded

view hooks are different

not on a page, instead, executed on a viewcontroller, ie, on a modal which is loaded, executed on certain points of time

willEnter observable, fired when component is about to become active

didEnter  observable, fired when Component has become active

willLeave  observable, fired when component is about to become inactive

didLeave observable, fired when component has become inactive

willUnload  observable, fired when component has been destroyed

onWillDismiss called when current viewcontroller will be dismissed

onDidDismiss called when current viewcontroller was dismissesd


# 63. Must-Read: Modal & willLeave

!!! In the following lecture ("Receiving Modal Data..."), we'll use willLeave . Please be aware that since the Modal no longer extends the ViewController, this method is NOT AVAILABLE anymore!

# 64. Receiving Modal Data by Using the ViewController


modal.willLeave, simple returns an observable, we can subsribe to it.

```js
  onViewQuote(quote: Quote) {
    const modal = this.modalCtrl.create(QuotePage, quote);
    modal.present();
    modal.onDidDismiss((remove: boolean) => {
      // we use this method, when we dismissing a viewcontroller like the modal
      console.log("onDidDismiss", remove);
    });
  }
```

```js
  onViewQuote(quote: Quote) {
    const modal = this.modalCtrl.create(QuotePage, quote);
    modal.present();
    modal.onDidDismiss((remove: boolean) => {
      if (remove) {
        this.quotesService.removeQuoteFromFavorites(quote);
      }
    });
  }
```

it does not dissapear, just if we switch the page

# 64. Updating the "Favorite Quotes" Page

this page is not rerender when we dismiss the modal

the modal is just an overlay, the page below it, just was hidden

the page is not rerender.

favorites.ts

```js

onViewQuote(quote: Quote) {
  const modal = this.modalCtrl.create(QuotePage, quote);
  modal.present();
  modal.onDidDismiss((remove: boolean) => {
    if (remove) {
      this.quotesService.removeQuoteFromFavorites(quote);
      // alternative 1
      this.quotes = this.quotesService.getFavoriteQuotes();
      // alternative 2
      // const position = this.quotes.findIndex((quoteEl: Quote) => {
      //   return quoteEl.id == quote.id;
      // })
      // this.quotes.splice(position, 1);
    }
  });
}
```

# 66. Adding an "Unfavorite" functionality to the App

prevent user to favorite a quote several time
switch favorite button to unfavourite

quotes.html

button to unfavorite a quote

```html
<ion-row>
  <ion-col text-right>
    <button ion-button clear small
      (click)="onAddToFavorites(quote)"
    >
    Favorite</button>
    <button ion-button clear small color="danger"
      (click)="onRemoveFromFavorites(quote)"
    >
    Unfavorite</button>
  </ion-col>
</ion-row> 
```

i dont want to display the btns all the time

impement a helper method, in the quotes.ts

```js
  onRemoveFromFavorites(quote: Quote) {

  }

  isFavorite(quote: Quote) {
    
  }
```

going to our services/quotes.ts

another helper method

```js
    isQuoteFavorite(quote: Quote) {
        return this.favoriteQuotes.find((quoteEl: Quote) => {
            return quoteEl.id == quote.id;
        })
    }
```

exectuing the find method in our favQuotes array, it takes an argument, a function, that is executed for each element in this array. this function needs to return true or false, indicating if its the element that i was looking for or not

in quotes.ts


```js
  isFavorite(quote: Quote) {
    return this.quotesService.isQuoteFavorite(quote);
  }
```


now i can use this favorite method in my template

quotes.html

```html
<ion-col text-right>
  <button ion-button clear small
    (click)="onAddToFavorites(quote)"
    *ngIf="!isFavorite(quote)"
  >
  Favorite</button>
  <button ion-button clear small color="danger"
    (click)="onRemoveFromFavorites(quote)"
    *ngIf="isFavorite(quote)"
  >
  Unfavorite</button>
</ion-col>
```

logic for unfavorite

quotes.ts

```js
onRemoveFromFavorites(quote: Quote) {
  this.quotesService.removeQuoteFromFavorites(quote);
}
```

# 67. Revealing extra List Item Options upon Sliding

work on favorites page

we want to slide the item, to reveal a delete button
to slide it to the left!!

favorites.html

```html
<ion-item-sliding>
      
</ion-item-sliding>
```



```html
<ion-item-options>
        
</ion-item-options>
```

they work together

we slide the element next to the ion-item-options

```html
<ion-content padding>
  <ion-list>
    <ion-item-sliding *ngFor="let quote of quotes">
      <ion-item 
        (click)="onViewQuote(quote)">
        <h2>{{ quote.person }}</h2>
        <p>{{ quote.text }}</p>
      </ion-item>
      <ion-item-options>
        <button ion-button color="danger">
          <ion-icon name="trash"></ion-icon>
          Delete
        </button>
      </ion-item-options>
    </ion-item-sliding>
  </ion-list>
</ion-content>
```

working!
the button is not doing anything

```html
<button ion-button color="danger"
        (click)="onRemoveFromFavorites(quote)"
>
```

favorites.ts

we copy paste the code from onViewQuote

```js
  onRemoveFromFavorites(quote: Quote) {
    this.quotesService.removeQuoteFromFavorites(quote);
    // alternative 1
    this.quotes = this.quotesService.getFavoriteQuotes();
    // alternative 2
    // const position = this.quotes.findIndex((quoteEl: Quote) => {
    //   return quoteEl.id == quote.id;
    // })
    // this.quotes.splice(position, 1);
  }
```


let's refactor

```js
  onViewQuote(quote: Quote) {
    const modal = this.modalCtrl.create(QuotePage, quote);
    modal.present();
    modal.onDidDismiss((remove: boolean) => {
      if (remove) {
        this.onRemoveFromFavorites(quote);
      }
    });
  }
```

working!

# 68. Changing the overall App Theme

the theming

settings page, we are not using it at all

theme folder,

variable.scss

overriding ionic variables (docu)

we want to change the primary color

we have too much padding

it already exist, if we check the documentation. it has 16px

```css
// Shared Variables
// --------------------------------------------------
$content-padding: 8px
```


```css
$colors: (
  primary:    #488aff,
  secondary:  #32db64,
  danger:     #f53d3d,
  light:      #f4f4f4,
  dark:       #222,
  // add your own colors
  quoteBackground: #f2f7c0
);
```


```html
<ion-item color="quoteBackground"
  (click)="onViewQuote(quote)">
  <h2>{{ quote.person }}</h2>
  <p>{{ quote.text }}</p>
</ion-item>
```

we can use this color created, just in Ionic2 components

you can overwrite defaults

```css
primary:    #ffbb00,
```

not blue anymore, gold


# 69. Adding a side-menu

side menu, go to settings

on the roots page, above the current root page, would be the bottom navb page

if we open the menu, and choose settings, i want to go to a totally different page

i dont want to see the favorites library icons

the menu, needs to be able to edit the rootpage

lets go to the rootcomponent

app.html

```html
<ion-nav [root]="rootPage"></ion-nav>
```

```html
<ion-menu>
    
</ion-menu>

<ion-nav [root]="rootPage"></ion-nav>
```

ion menu needs to know where it should render its content

i want to render my content here, on the navigation stack

```html
<ion-menu [content]="nav">

</ion-menu>

<ion-nav [root]="rootPage" #nav></ion-nav>
```

i will create a local reference, #nav, and i can pass it

i have to bind to the content property and i will bind this navreference

the content, of the menu item i select, should get render in this navigation stack

now i can style the menu

app.html

```html
<ion-menu [content]="nav">
    <ion-header>
        <ion-toolbar>
            <ion-title>Menu</ion-title>
        </ion-toolbar>
    </ion-header>
    <ion-content>
        <ion-list>
            <button ion-item (click)="onLoad(tabsPage)">
                <ion-icon name="quote" item-left></ion-icon>
                Quotes
            </button>
            <button ion-item (click)="onLoad(settingsPage">
                <ion-icon name="settings" item-left></ion-icon>
                Settings
            </button>
        </ion-list>
    </ion-content>
</ion-menu>

<ion-nav [root]="rootPage" #nav></ion-nav>

```

app.component.ts

```js
export class MyApp {
  rootPage:any = TabsPage;
  // rootPage:any = LibraryPage;
  tabsPage = TabsPage;
  settingsPage = SettingsPage;
  [...]
}
```

i can delete rootPage

app.html

```html
<ion-nav [root]="tabsPage" #nav></ion-nav>
```

by default i want to load the tabsPage

i need to implement onLoad

app.components.ts

```js
  // i expect a page
  onLoad(page: any) {
    // i want to replace my currently active rootpage
    // i want to replace the root attribute of my navigation stack, in app.html
  }
```

# 70 How to change the root page


inject navcontroller

i am in the app component, where the application is started

app.componenet.ts

the stack was not created, so we can not inject navController


let's have access to our navigation stack

we can use, 

```js
@ViewChild
```

import it

```js
import { Component, ViewChild } from '@angular/core';
```


i want to have acess to my ion-nav element



is kind of my navctrl represented in a template, this is what instantiates my navcontroller, initates my navigation stack

we want to select by the reference nav

local references in the template, #nav

store it in a property named nav: of type NavController

```js
import { Platform, NavController } from 'ionic-angular';
```

```js
export class MyApp {
  // rootPage:any = TabsPage;
  // rootPage:any = LibraryPage;
  tabsPage = TabsPage;
  settingsPage = SettingsPage;
  @ViewChild('nav') nav: NavController;
```

!!! Difficult!

```js
  onLoad(page: any) {
    this.nav.setRoot(page);
  }
```

Import menucontroller

```js
constructor(platform: Platform, private menuCtrl: MenuController,
    statusBar: StatusBar, splashScreen: SplashScreen) {
    platform.ready().then(() => {
      statusBar.styleDefault();
      splashScreen.hide();
    });
```

```js
  onLoad(page: any) {
    this.nav.setRoot(page);
    this.menuCtrl.close();
  }
```


# 71. Adding a Menu Toggle Button to the Navigation Bar

the menu item, should be display on the fav page, library page, setting page

lets start with the fav page

favorites.html

```html
  <ion-navbar>
    <ion-buttons start>
      <button ion-button>
        <ion-icon name="menu"></ion-icon>
      </button>
    </ion-buttons>
    <ion-title>Favorites Quotes</ion-title>
  </ion-navbar>
```

when working with buttons, you should wrap it into ion-buttons, its easier to position them

```html
<button ion-button (click)="onOpenMenu()">
        <ion-icon name="menu"></ion-icon>
      </button>
```

we controll the menu through the MenuController

```js
  constructor ( private quotesService: QuotesService, 
                private modalCtrl: ModalController,
                private menuCtrl: MenuController
              ) {}
```

import

```js
import { ModalController, MenuController } from 'ionic-angular';
```

```js
  onOpenMenu() {
    this.menuCtrl.open();
  }
```


now we should to add the method, the import, the constructor, in every page

there is a shortcut, remove everything


in the template, favorites.html

remove click, event listener

helpful directive

```html
<button ion-button (click)="onOpenMenu()">
  <ion-icon name="menu"></ion-icon>
</button>
```

now let's implement this in the other pages

library.html


```html
<ion-header>
  <ion-navbar>
    <ion-buttons start>
      <button ion-button menuToggle>
        <ion-icon name="menu"></ion-icon>
      </button>
    </ion-buttons>
    <ion-title>Quotes Library</ion-title>
  </ion-navbar>
</ion-header>
```


settings.html

```html
<ion-header>
  <ion-navbar>
    <ion-header>
      <ion-navbar>
        <ion-buttons start>
          <button ion-button menuToggle>
            <ion-icon name="menu"></ion-icon>
          </button>
        </ion-buttons>
        <ion-title>Quotes Library</ion-title>
      </ion-navbar>
    </ion-header>
    <ion-title>settings</ion-title>
  </ion-navbar>
</ion-header>
```


in library inspiration, in a quote, we dont have the menu, because we didn't put it

the missing piece, implement the settings page, display another background



# 72. Preparing the settings page

```html
<ion-content padding>
  <ion-grid>
    <ion-row>
      <ion-col>
        <ion-label>
          Alternative Background
        </ion-label>
      </ion-col>
      <ion-col>
        <ion-toggle></ion-toggle>
      </ion-col>
    </ion-row>
  </ion-grid>
</ion-content>
```

in the toogle, i want to lissten to an event

its fired whenever we use the toggle

```html
<ion-toggle (ionChange)="onToggle($event)"></ion-toggle>
```

settings.ts


```js
import { Component } from '@angular/core';

@Component({
  selector: 'page-settings',
  templateUrl: 'settings.html',
})
export class SettingsPage {

}
```

i know that i get an event type toggle

```js
import { Component } from '@angular/core';
import { Toggle } from 'ionic-angular';

@Component({
  selector: 'page-settings',
  templateUrl: 'settings.html',
})
export class SettingsPage {
  onToogle(toggle: Toggle) {
    // toogle has some info about the state of this input
    console.log(toggle);
  }
}
```

checked: true

the most important information

we need to store this information in a central page, so that we can use this information in the favorites page

a serviceeee

# 73. Creating and Using the Settings Service to Store Settings

new service called settings in the services folder

services/settings.js

```js
export class SettingsService {
    private altBackground = false;

    setBackground(isAlt: boolean) {
        this.altBackground = isAlt;
    }

    isAltBackground() {
        return this.altBackground;
    }
}
```




i want to access this service in settings/settings.ts

i need to add it in the providers

app.module.ts

```js
  providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler},
    QuotesService,
    SettingsService
  ]
```

```js
import { SettingsService } from '../services/settings';
```

now in settings/settings.ts

i can inject it, in the constructor

```js
constructor(private settingsService: SettingsService) {}
```

```js
import { SettingsService } from '../../services/settings';
```

```js
  onToggle(toggle: Toggle) {
    this.settingsService.setBackground(toggle.checked);
  }

  checkAltBackground() {
    return this.settingsService.isAltBackground();
  }
```

we need this method because

in the settings.html. 

we want to display the state it is

if we destroy the page, we would start at the unchecked again

ion-toggle also takes an input

```html
  <ion-toggle (ionChange)="onToggle($event)" [checked]="checkAltBackground()"></ion-toggle>
```

store the state stored in our service



the toggle remains toggle, if we change the page

because its correctly stored in the settings, and then we pass it to the html

# 74. Adding an alternative Item Background

time to implemenet our alternative background

variables.css

```css
$colors: (
  primary:    #ffbb00,
  secondary:  #32db64,
  danger:     #f53d3d,
  light:      #f4f4f4,
  dark:       #222,
  // add your own colors
  quoteBackground: #f2f7c0,
  altQuoteBackground: #f9ebc2
);
```

in favorites.html

```html
<ion-item color="quoteBackground"
        (click)="onViewQuote(quote)">
        <h2>{{ quote.person }}</h2>
        <p>{{ quote.text }}</p>
      </ion-item>
```

right now we are assigning quoteBackground as the colour

i want to change this

i need to pass a different name, depending if its toggled or not

implement a helper method

favorites.ts

```js
  getBackground() {
    
}
```

i want to use a the settingsservice, 

```js
  constructor ( private quotesService: QuotesService, 
                private modalCtrl: ModalController,
                private settingsService: SettingsService
              ) {}
```

```js
import { SettingsService } from '../../services/settings';
```

```js
  getBackground() {
    return this.settingsService.isAltBackground() ? "altQuoteBackground" : "quoteBackground";
  }
```


HTML, set up a color property binding

favorites.html

```html
<ion-item [color]="getBackground()"
        (click)="onViewQuote(quote)">
        <h2>{{ quote.person }}</h2>
        <p>{{ quote.text }}</p>
      </ion-item>
```

alternative to use the colours

create a special class, in scss file

in folder favorites

favorites.scss

```css
page-favorites {
    .alt {
        background-color: color($colors, altQuoteBackground)
    }
}
```

now i am fetching this color from the variables.scss dynamicall, because we have same name

```html
<ion-item color="quoteBackground" [ngClass]="{alt: isAltBackground()}"
  (click)="onViewQuote(quote)">
  <h2>{{ quote.person }}</h2>
  <p>{{ quote.text }}</p>
</ion-item>
```

favorites.ts

```js
  isAltBackground() {
    return this.settingsService.isAltBackground();
  }
```




















































































































































































































































































































































































































