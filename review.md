# Meta
Observations about making this useful to the world, not just us.

I get why, but mixing the 'container' and the 'service' makes it a lot
harder to 'get into'. More folks will be able to help if you make this
container agnostic, meaning just an npm.

Postgres (have you heard Jim call it 'post-grey'?) and SQLite support
would be good 'driver' tests and make it locally hackable for non
windowsers out in the wild.

# Filewise

## Meta
I didn't see a way to just render the query but not run it. That's what
I'd expect `HEAD` to do.

I'd have providers for both templates and databases that are just
modules that are
[EventEmitters](http://nodejs.org/api/events.html#events_class_events_eventemitter).

## bin/epic-server:3-16
Oh man, I hope you have a macro for that. Can you share that macro?


## bin/epic-server:19-48
Smells like mapping, how about defining a name protocol for env-vars and
auto making this object by looping `_.keys(process.env)`.

## bin/epic-server:48
Herokuism, which assumes containers, but the prevailing convention is
merely `PORT`.

## bin/epic-server:77
Is this just becuase we have non unicode columns, becuase none of those
characters are what I'd call *special*?

Seems like we should be able to bypass this if you set
`EPIQUERY_UNICODE_SAFE`.

## bin/epic-server:77
Having a separate *ro* connection confuses me. Actually no, it doesn't
confuse me, but it is redundant. I'd set connections to be *ro*, and use
that pool for `GET`, separate pool for `POST`.

## bin/epic-server:81-89
[Factor 11](http://12factor.net/logs)

## bin/epic-server:81-89
Look at [mincer](https://github.com/nodeca/mincer), this is the best of
this genre I've found yet.

## bin/epic-server:154-180
Now I see a theme, if there is one module for each engine that `exports`
a set of well defined functions, that'll make you a simple provider.

## bin/epic-server:160
Doesn't set ro, i.e. doesn't match your comment.

## bin/epic-server:208
Pool.
I'd use [async](https://github.com/caolan/async) rather than promises,
as this feels a lot like a staged pipeline.
[Example](https://github.com/wballard/starphleet/blob/master/scripts/starphleet.coffee)
268

## bin/epic-server:254
This batch/buffer should switch to a *common* event strategy to get you
the ability to SSE. If you are getting a bulk JSON, then the events can
just be accumulated to a buffer. So, a 'template' stage, a 'query'
stage' and a 'results' sink, which is pluggable to SSE, WS, or REST.

## bin/epic-server:332
I know this area isn't you. But my initial reaction is *really*, hard
code in a GLG catalog? I'm not sure this is headed in the 'generally
t
usable open source' direction.

*But*, it points out you are missing *schema/catalog* from your set of
env vars.

## bin/epic-server:338-346
This code appears to be unaware of bin/epic-server:68, and has invented
an alternate header scheme.

## bin/epic-server:401
This feels like something to be added by the container.

## bin/epic-server:430
The `if/else` chain referring to flagging set by bin/epic-server:411
feels too hard coded to me. I get each 'database' has a directory of
files, but why not have a `.env`, which overrides for that folder on
down. Cat down the path, shell it with a `set` command at the end, read back the env vars. Later on these can be cached with a file watcher.

## bin/setup-environment
Even if you don't containerize, check out
[buildpack](https://github.com/wballard/heroku-buildpack-nodejs)
