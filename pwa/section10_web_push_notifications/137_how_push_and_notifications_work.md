137_how_push_and_notifications_work.md

web app, service worker running in the background

push: some server pushes some information to our webapp
notifications, what the user sees

first step: user enables notifications. one time step

we can display notification, independent of push from server

push notification

if we push messages from server

inform all users from new post

in web app, we can check for existing push subscriptions

create new subscription, created in js, with the sw

subscrption for push notification needs external service

if our webapp is closed, where do we show the notifications?

a broswer vendor push server, get push API endpoint

when we create a subscription, js reaches this browser vendor service

so, we have an endpoint where we can deliver pushes

we have our own server also

service worker stores subscripton on own server

in the db, later on, our backend server will push

send push notification to the web app

(a post was created)

it is sent to the browsesr vendor push server

this triggers the push event

service worker display the notification.








































