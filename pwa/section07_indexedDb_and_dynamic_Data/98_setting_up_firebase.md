98_setting_up_firebase.md

set up a backend, file storage

firebase.console


create project

databse, storage, hosting, functions, to send endpoints

realtime database, rules

```js
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

we dont use authentication in this course

```js
{
  "rules": {
    ".read": "true",
    ".write": "auth != null"
  }
}
```

posts
  first-post
    image: value
    id: "first-post"
    location: "in san francisco"
    title: "awesome trip to san francisco"



storage

to upload an image


```js
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

under file location, we see download url

this is the url i want to store

https://firebasestorage.googleapis.com/v0/b/pwagram-50bea.appspot.com/o/Screen%20Shot%202018-03-12%20at%205.11.07%20PM.png?alt=media&token=54403787-68e6-4ddf-9012-e7a2c426fdd0

i put in my db

in image









































