28_understanding_service_worker_events.md

listenable events in service worker

event / source
- fetch: brower or page-related js initiates a fetch (http request)

ajax does not trigger this event


we can cache, stop, this requests

- push notifications: service worker receives web push noticication (from server)

- notification interaction: user interacts with displayed notification

- background sync: service worker receives background sync event (e.g internet connection was restored)

- service worker lifecycle: service worker phase changes

test

















