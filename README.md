earls is a hack to display web resources that are being tweeted with a given hashtag.
It uses node and redis to watch Twitter for new tweets with urls, then fetches the web resource, attempts to pull the title out of the HTML, and then stashes away some results into a Redis database The idea
is that it can help share information about what is being tweeted about at conferences, and other real or virtual events that people are tweeting at.

When your event is over it is easy to create a static site of your earls 
instance and turn off the node server and redis backend, and just let it be 
served up as HTML, CSS and JavaScript.

## Install

1. install [redis](http://redis.io) and [node](http://nodejs.org)
1. git clone https://github.com/edsu/earls
1. cd earls
1. npm install
1. set Twitter credentials in environment: TWITTER\_CONSUMER\_KEY, TWITTER\_CONSUMER\_SECRET, TWITTER\_ACCESS\_TOKEN, TWITTER\_ACCESS\_TOKEN\_SECRET
1. set ENV about TCP/IP port and IP address using EARLS_PORT=Your-Port amd EARLS_HOSTNAME="Your-IP"
1. ./earls.js '#c4l15'

## Heroku

To get things to run on Heroku you'll need to set some environment variables
before you can push there:

```
heroku config:set EARLS_TRACK=#c4l15
heroku config:set EARLS_PORT=Your-Port
heroku config:set EARLS_HOSTNAME="Your-IP"
heroku config:set TWITTER_CONSUMER_KEY=XXX
heroku config:set TWITTER_CONSUMER_SECRET=XXX
heroku config:set TWITTER_ACCESS_TOKEN=XXX
heroku config:set TWITTER_ACCESS_TOKEN_SECRET=XXX
```
EARLS_PORT, *earls* default port is 3000.
EARLS_HOSTNAME, *earls* default hostname is "" (listen and bind on all local IP interfaces like "*").

## Loading

earls listens for new tweets. If you'd like to add some existing tweets that
you've collected with [twarc](http://github.com/edsu/twarc) you can load them
into redis like so:

    ./load.js tweets.json 

This will connect to a local redis instance by default. If you would like to 
load into a remote herkou redis db get the redis URI from the resources tab in 
the Heroku admin, and then pass it in as a second parameter:

    ./load.js tweets.json redis://redistogo:YOURPASSWORDHERE@mummichog.redistogo.com:10771

Loading existing tweets is useful when you haven't been running earls since the
beginning of an event, and you would like to load some of this historical data.
