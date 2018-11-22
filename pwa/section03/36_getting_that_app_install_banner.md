now we meet the criteria

What is the criteria?
In order for a user to be able to install your Progressive Web App, it needs to meet the following criteria:

The web app is not already installed
Meets a user engagement heuristic (currently, the user has interacted with the domain for at least 30 seconds)
Includes a web app manifest that includes:
short_name or name
icons must include a 192px and a 512px sized icons
start_url
display must be one of: fullscreen, standalone, or minimal-ui
Served over HTTPS (required for service workers)
Has registered a service worker with a fetch event handler


cuz we have a service worker registered

visited at least twice, with at least five minutes between visits.

