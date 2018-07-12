# Section: 22 Deploying an Angular App

# 291. Module Introduction

deploy

on a real server

deploying an angular app is easy

we will deploy to aws s3, this is just an example

in the end, the deployment process always follows the same steps

# 292. Deployment Preparatons and Important Steps

Deployment Steps & Things to keep in mind


first of all, we need ot build our app for production

minified and consider aot compilation

set the correct <base> element

in the index.html

for example, 

```js
example.com/my-app you should have <base href="/my-app/">
```

by default, if you are serving your app form your domain.com, / nothing

then it will be set correctly

but if your angular app runs not in your root domain, but in some nested path, like /my app

then you should adjust the base element to /myapp

make sure, that your server always reutrns index.html

routes re registered in angular app, so the server wont know your routes. return index.html in case of 404 errors




it only applies if u are using the path location strategy for routing, which is the default, 

normal, urls, of your app.com/recipes

when using this, the route is parsed by the server parsed, this is how the we works

we send the request to the server

the server recieves the erquest and tries to find the route

chances are, he will not find the route, because the route is registered in angular, not in the server

therefore, your server, will come to the ocnclusion, that this route is not found, and will throw a 404 error

this is the default behavior, its bad here, we dont want to get an error, we want our app

we know that the route is in our angular app

the solution is, config your server which is hosting your angular app to always return the angular app in case of a unfound route

then angular has a chance of taking over

it depends which server you are using

its important that you configure it that way.

heroku angular 





with ng serve we didnt have this issue. we have this behaviour automatically

we can deploy the generated stuff in your dist folder

# 293. Example: deploying to AWS S3

we need to be logged in in aws in the s3 page

they have a free tier

we can use another server, same steps, but how you return the index.html in case of 404 may be different

amazon s3 its a typical chosie, for single web app



we need to create a bucket, 

s3 console, create a bucket, chose a bucket name

it should be unique

bucket names are shared in the whole amazone ecosytem

next next

now, to properties

static website hosting

host a static website, which does not require server-side technologies

use this website to host a website

index document
we want to serve the index.html

the error document, in 404 cases, this should be the index.html too

save

bucket hosting!

give the right permissions

we can add a bucket policy

we are in the bucket policy editor

go to documentation

bucket policy examples

click, granting read-only permission to an anonymous user

```js
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"AddPerm",
      "Effect":"Allow",
      "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::examplebucket/*"]
    }
  ]
}
```

thats what we need, every one should be able to read

replace exmaple bucket with our bucket name

save it

with that, the bucket is configured

now we can upload our content

in order to upload our content, we need to make sure that we built it first

quit ng serve

```js
ng build --prod --aot
```

```js
Date: 2018-07-10T08:45:37.546Z
Hash: 1b055188cdacde029f3c
Time: 38975ms
chunk {0} 0.17c22ad224eac1bedb43.js () 23.1 kB  [rendered]
chunk {1} runtime.e69b9c4643fe1a8a5986.js (runtime) 1.84 kB [entry] [rendered]
chunk {2} styles.1817f60d06d502d4a471.css (styles) 114 kB [initial] [rendered]
chunk {3} polyfills.92108b287fe28032870b.js (polyfills) 59.6 kB [initial] [rendered]
chunk {4} main.9e5e560c2a55c3afbf73.js (main) 1.11 MB [initial] [rendered]
```

build our project with aot

the build process finished, now its about the content of the dist

everything inside dist we need, outside, not

note about base href
by default is "/"

if you plan to deploy your app to / my app, you need to change it there

we re going to serve our app from the root domain

so, back to s3 console

uplod all the files

inside dist

so, all the files were uploaded to the s3, to the bucket we jsut created

if we go to the properties, on static ewb hosting, we find the url


































