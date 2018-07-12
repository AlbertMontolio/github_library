# Section 8 

# 172 Module Content

- usage of native device features (e.g. Camera)

- usage of device storage (for files and data)

# 173 What we'll build

# 174. Generating the required Pages

ionic generate page place
ionic generate page add-place
ionic generate page set-location

add to ngmodule


```js
@NgModule({
  declarations: [
    MyApp,
    HomePage,
    AddPlacePage,
    SetLocationPage
  ],
  ```

```js

entryComponents: [
  MyApp,
  HomePage,
  AddPlacePage,
  SetLocationPage
],
```



two models, one model for our location, one for an individual place

# 174. Creating Models for Location and Places

folder src/models

models/location.ts

```js
export class Location {
    constructor(public lat: number, public lng: number) {}
}
```

models/place.ts

```js
import { Location } from "./location";

export class Place {
    constructor(
        public title: string, 
        public description: string,
        public location: Location,
        public imagePath: string
    ) {}
}
```


add places now

# 176. Implementing navigation

i want to add a plus button thatnavigates to the add-place

home.html

```html
<ion-navbar>
  <ion-buttons end>
    <button ion-button icon-only [navPush]="addPlacePage">
      <ion-icon name="add">
        
      </ion-icon>
    </button>
  </ion-buttons>
  <ion-title>Awesome Places</ion-title>
</ion-navbar>
```

home.ts

```js
export class HomePage {
  addPlacePage = AddPlacePage;
```

add a form there

# 177. Filling the new place page with life (incl. template-driven form)

add-place.html

```html
<ion-content padding>
  <form #f="ngForm" (ngSubmit)="onSubmit(f)">
    <ion-list>
      <ion-item>
        <ion-label fixed>Title</ion-label>
        <ion-input
          type="text"
          placeholder="Beautiful church..."
          name="title"
          ngModel
          required
        >
        
        </ion-input>
      </ion-item>
      <ion-item>
        <ion-label floating>Short description</ion-label>
        <ion-textarea
          name="description"
          ngModel
          required
        >
        </ion-textarea>
      </ion-item>
      
    </ion-list>

    <ion-grid>
      <ion-row>
        <ion-col>
          <button 
            ion-button
            block
            outline
            type="button"
            icon-left
            (click)="onLocate()"
          >
            <ion-icon name="locate"></ion-icon>
            Locate me
          </button>
        </ion-col>
        <ion-col>
          <button
            ion-button
            block
            outline
            type="button"
            icon-left
            (click)="onOpenMap()"
          >
            <ion-icon name="map">Select on Map</ion-icon>
          </button>
        </ion-col>
      </ion-row>

      <ion-row>
        <ion-col>
          <p>Selected Place...</p>
        </ion-col>
      </ion-row>

      <ion-row>
        <ion-col text-center>
          <h5>Take a photo!</h5>
        </ion-col>
      </ion-row>

      <ion-row>
        <ion-col>
          <button
            ion-button
            icon-left
            block
            outline
            type="button"
            (click)="onTakePhoto()"
          >
            <ion-icon name="camera"></ion-icon>
          </button>
        </ion-col>
      </ion-row>

      <ion-row>
        <ion-col>
          <img [src]="">
        </ion-col>
      </ion-row>

      <ion-row>
        <ion-col>
          <button
            ion-button
            color="secondary"
            block
            type="submit"
            [disabled]="!f.valid"
          >
            Add this place
          </button>
        </ion-col>
      </ion-row>

    </ion-grid>

  </form>
</ion-content>
```

add-place.ts

```js
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  selector: 'page-add-place',
  templateUrl: 'add-place.html',
})
export class AddPlacePage {
  onSubmit(form: NgForm) {
    console.log(form.value);
  }
}
```

picking a location!

Using Angular Google Maps
Section 8, Lecture 178
In the following lecture, we'll add Angular Google Maps to the project. There is a newer version available which uses different package + component names. 

Here are the required adjustments:

1) Install a different package: npm install --save @agm/core 

2) Import from '@agm/core'  instead of 'angular2-google-maps/core' 

3) Import + configure the SAME module (in AppModule  imports[] , add AgmCoreModule.forRoot({...})

4) Use different component names: <agm-map>  instead of <sebm-google-map> , <agm-marker>  instead of <sebm-google-map-marker> 

That's it! You can read more in the official docs: https://angular-maps.com/guides/getting-started/



# 179. Adding Google Maps to the App

```
npm install angular2-google-maps OBSOLETE
```

```js
npm install --save @agm/core 
```

include it in our app

set location page html

```html
<ion-content padding>
  <ion-grid>
    <ion-row>
      <ion-col>
          <agm-map>
            
          </agm-map>
      </ion-col> 
    </ion-row>
  </ion-grid>
</ion-content>
```

set-location.scss

```css
page-set-location {
    agm-map {
        height: 250px;
    }
}
```

add-place.ts

```js
export class AddPlacePage {
  constructor(private modalCtrl: ModalController) {
    
  }

  onSubmit(form: NgForm) {
    console.log(form.value);
  }

  onOpenMap() {
    const modal = this.modalCtrl.create(SetLocationPage);
    modal.present();
  }
}
```

we need to configure the agmmap tag

app.module.ts


```js
import { AgmCoreModule } from "@agm/core"
```

```js
imports: [
    BrowserModule,
    IonicModule.forRoot(MyApp),
    AgmCoreModule.forRoot({

    })
  ],
```

we need an api key

googe, angular 2 google maps api key

www.angular-maps.com

look for set api key here

https://developers.google.com/maps/documentation/javascript/get-api-key?hl=en#key

get started

create new project

```js
AgmCoreModule.forRoot({
  apiKey: ""
})
```

# 180. Configuring our maps

set-location.html

we started the map in the ocean

set-location.ts

new property location

```js
export class SetLocationPage {
  location: Location;
}
```

add-place.ts

```js
export class AddPlacePage {
  location: Location = {
    lat: 40.7624324,
    lng: -73.9759827
  };
```


i need to pass this location to my modal

```js
  onOpenMap() {
    const modal = this.modalCtrl.create(SetLocationPage, {location: this.location});
    modal.present();
  }
  ```

set-location.ts

```js
export class SetLocationPage {
  location: Location;

  constructor(private navParams: NavParams) {
    this.location = this.navParams.get('location');
  }
}
```


set-location.html

```html
<agm-map
  [latitude]="location.lat"
  [longitude]="location.lng"
  [zoom]="16"
>
</agm-map>
```



we have some properties that we can bind from outside


# 180. Allowing the User to Place a Marker on the Map

we need to tap the map

```html
<agm-map
  [latitude]="location.lat"
  [longitude]="location.lng"
  [zoom]="16"
  (mapClick)="onSetMarker($event)"
>
<agm-marker></agm-marker>
</agm-map>
```

set-location.ts

```js
export class SetLocationPage {
  location: Location;
  marker: Location;

  constructor(private navParams: NavParams) {
    this.location = this.navParams.get('location');
  }

  onSetMarker(event: any) {
    console.log(event);
    this.marker = new Location(event.coords.lat, event.coords.lng)
  }
}
```

https://angular-maps.com/guides/getting-started/

# 182. Passing the Chosen Location back to the Page

set-location.html

```html
<ion-row>
  <ion-col>
    <button
      ion-button
      block
      color="secondary"
      (click)="onConfirm()"
      [disabled]="!marker"
    >
    Confirm
    </button>
  </ion-col>
  <ion-col>
    <button
      ion-button
      block
      color="danger"
      (click)="onAbort()"
    >
    Abort
    </button>
  </ion-col>
</ion-row>
```

```js
constructor(
  private navParams: NavParams,
  private viewCtrl: ViewController
) {
  this.location = this.navParams.get('location');
}
```

```js
onConfirm() {
  // the location that the user choses
  this.viewCtrl.dismiss({location: this.marker});
}

onAbort() {
  this.viewCtrl.dismiss();
}
```

the location is the one that we use to center the map
now we want the location that the user pressed: this.marker

# 183. Displaying the chosen location





add-place.ts

```js
onOpenMap() {
  const modal = this.modalCtrl.create(SetLocationPage, {location: this.location});
  modal.present();
  modal.onDidDismiss(
    data => {
      if (data) {
        this.location = data.location;
      }
    }
  )
}
```

```js
export class AddPlacePage {
  location: Location = {
    lat: 40.7624324,
    lng: -73.9759827
  };

  locationIsSet = false;
  ```

```js
data => {
  if (data) {
    this.location = data.location;
    this.locationIsSet = true;
  }
}
```

add-place.html

output the location


instead of 

```html
<p>Selected Place...</p>
```

only show the row if we did select a location

```html
<ion-row *ngIf="locationIsSet">
  <ion-col>
      <agm-map
        [latitude]="location.lat"
        [longitude]="location.lng"
        [zoom]="16"
      >
      <agm-marker
        [latitude]="location.lat"
        [longitude]="location.lng"
      >
      </agm-marker>
      </agm-map>
  </ion-col>
</ion-row>
```

add-place.scss

```js
page-add-place {
    agm-map {
        height: 250px;
    }
}
```

add-place.html

```html
[disabled]="!f.valid || !locationIsSet"
```


```html
<agm-map
  [latitude]="location.lat"
  [longitude]="location.lng"
  [zoom]="16"
>
```

centers the map

if i go back to select location, i go back to the initial marker

set-location.js

```js
const modal = this.modalCtrl.create(SetLocationPage, {location: this.location, isSet: this.locationIsSet});
```

set-location.ts (the modal)

i wanna set the marker, if we already chose a location

```js
constructor(
  private navParams: NavParams,
  private viewCtrl: ViewController
) {
  this.location = this.navParams.get('location');
  if (this.navParams.get('isSet')) {
    this.marker = this.location;
  }
}
```

automatically pick our location based in our current location

# 184. Using ionic native 3 instead of 2

Using Ionic Native 3 instead of 2
Section 8, Lecture 184
This course covers Ionic 2 (but will also work with Ionic 3) and Ionic-native 2 (which actually is a separate package). The following lectures use Ionic-native 2.

If you want to stick to Ionic-native 2, you might need to install it via npm install --save ionic-native  (run this command if you can't find an entry in the dependency list in your package.json file).

Ionic-native 3 changes the way you provide and use the native plugins (such as the camera). Using it is optional, you should be able to still use the approach shown over the next lectures.

But if you want to use Ionic-native 3, you should follow the instructions provided in this official blog post and the official upgrade readme.

Official Blog Post: http://blog.ionic.io/ionic-native-3-x/

Official Upgrade Readme: https://github.com/driftyco/ionic-native/blob/master/README.md


# 185. Using a Native Device Feature: Geolocation to locate the user

inoicframework docu nativ

geolocation

```js
$ ionic cordova plugin add cordova-plugin-geolocation --variable GEOLOCATION_USAGE_DESCRIPTION="To locate you"

```

```js
ionic cordova plugin add cordova-plugin-geolocation
```

add-place.ts

```js
import { Geolocation } from '@ionic-native/geolocation';
```

```js
  constructor(
    private modalCtrl: ModalController,
    private geolocation: Geolocation
  ) {}
```

```js
onLocate() {
    // fetch my location
    this.geolocation.getCurrentPosition()
      .then(
        location => {
          this.location.lat = location.coords.latitude;
          this.location.lng = location.coords.longitude;
          this.locationIsSet = true;
        }
      )
      .catch(
        error => {
          console.log(error);
        }
      );
  }
```


app.module.ts

```js
import { Geolocation } from '@ionic-native/geolocation';
```


```js
providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler},
    Geolocation
  ]
```

it took some seconds!

but working :D

# 186. Polishing the Auto-Locate-Feature

add-place.ts

```js
constructor(
    private modalCtrl: ModalController,
    private geolocation: Geolocation,
    private loadingCtrl: LoadingController,
    private toastCtrl: ToastController
  ) {}
```

```js
onLocate() {
  const loader = this.loadingCtrl.create({
    content: 'Getting your location...'
  });
  loader.present();
  // fetch my location
  this.geolocation.getCurrentPosition()
    .then(
      location => {
        loader.dismiss();
        this.location.lat = location.coords.latitude;
        this.location.lng = location.coords.longitude;
        this.locationIsSet = true;
      }
    )
    .catch(
      error => {
        loader.dismiss();
        const toast = this.toastCtrl.create({
          message: 'Could not get location, please pick it manually!',
          duration: 2500
        })
        console.log(error);
        toast.present();
      }
    );
```

# 186. Using a native device feature: the camera

```js
ionic cordova plugin add cordova-plugin-camera
npm install --save @ionic-native/camera
```

app.module.ts

```js
import { Camera } from '@ionic-native/camera';
```

```js
@NgModule({
  ...

  providers: [
    ...
    Camera
    ...
  ]
  ...
})
```

add-place.ts

```js
import { Camera, CameraOptions } from '@ionic-native/camera';

```

```js
constructor(private camera: Camera) { }
```

```js
onTakePhoto() {
  this.camera.getPicture({
    encodingType: this.camera.EncodingType.JPEG,
    correctOrientation: true
  })
   .then(
     imageData => {
       console.log(imageData);
     }
   )
   .catch(
     err => {
       console.log(err);
     }
   );
}
```

error: cordova_not_available

we did to run on a native device

```js
ionic build ios
```

# 188. Displaying the picture

add-place.html

```html
<ion-row>
  <ion-col>
    <img [src]="imageUrl">
  </ion-col>
</ion-row>
```

add-place.js

```js
  locationIsSet = false;
  imageUrl = '';
```


```js
.then(
 imageData => {
   this.imageUrl = imageData;
 }
)
```


```js
ionic run ios --device
The run command has been renamed. To find out more, run:

ionic cordova run --help
```

# 189. Managing data 

add-place.html

```html
[disabled]="!f.valid || !locationIsSet || imageUrl == ''"
```

where we can store it?

we need a service

folder services
add places.ts

```js
import { Place } from "../models/place";
import { Location } from "../models/location";

export class PlacesService {
    private places: Place[] = [];

    addPlace(title: string, description: string, location: Location, imageUrl: string) {
        const place = new Place(title, description, location, imageUrl);
        this.places.push(place);
    }

    loadPlaces() {
        return this.places.slice();
    }
}
```

home.html

```html
<ion-content padding>
  <ion-card *ngFor="let place of places" (click)="onOpenPlace(place)">
    <img [src]="place.imageUrl">
    <ion-card-content>
      <ion-card-title>
        {{ place.title }}
      </ion-card-title>
      <p>{{ place.description }}</p>
    </ion-card-content>
  </ion-card>
</ion-content>
```

home.ts

```js
export class HomePage {
  addPlacePage = AddPlacePage;
  places: Place[] = [];

  constructor(public navCtrl: NavController) {}

  ionViewWillEnter() {

  }

}
```

app.module.ts

```js
providers: [
  StatusBar,
  SplashScreen,
  {provide: ErrorHandler, useClass: IonicErrorHandler},
  Geolocation,
  Camera,
  PlacesService
]
```

home.ts

```js
constructor(public navCtrl: NavController, private placesService: PlacesService) {}

ionViewWillEnter() {
  this.places = this.placesService.loadPlaces();
}
```

add-place.ts

```js
  constructor(
    private modalCtrl: ModalController,
    private geolocation: Geolocation,
    private loadingCtrl: LoadingController,
    private toastCtrl: ToastController,
    private camera: Camera,
    private placesService: PlacesService
  ) {}

  onSubmit(form: NgForm) {
    console.log(form.value);
  }
```

```js
  onSubmit(form: NgForm) {
    this.placesService.addPlace(
      form.value.title, 
      form.value.description,
      this.location,
      this.imageUrl
    );
    form.reset();
    this.location = {
      lat: 40.7624324,
      lng: -73.9759827
    };
    this.imageUrl = '';
    this.locationIsSet = false;
  }
```


run in my ios device????????ßß

ionic run ios --device

we don't see the image

add-place.html

```html
<ion-row *ngIf="imageUrl != ''">
  <ion-col>
    <img [src]="imageUrl">
  </ion-col>
</ion-row>
```

place.ts

```js
export class Place {
    constructor(
        public title: string, 
        public description: string,
        public location: Location,
        public imageUrl: string
    ) {}
}
```




































































































