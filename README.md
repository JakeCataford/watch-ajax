watch-ajax
==========

`watch-ajax` lets you live update your rails apps via polling. It was built with the idea of being very simple to implement.

This is probably not the best solution for large web applications, as it continuously `GET`s an update url for updates via ajax.

#### Installation

In your gemfile:
```Ruby
gem 'watch-ajax'
```

Then terminal:
```
$ bundle install
```

Include the javascript in application.js:
```Javascript
//= require watch-ajax
```
or coffescript:
```Coffeescript
#= require watch-ajax
```

Then, in your view:
```Ruby
<%= watch @model, :property %>
```
instead of:
```Ruby
<%= @model.property %>
```

And you're done.

#### Implementation

`watch-ajax` is a rails engine that gets mounted inside your application when you bundle install. It sets up a /watch controller and route (that extends your application controller). The `watch` view helper renders a `<span>` around the property you are rendering and supplies `data-attributes` to the span to identify it as something that watch cares about, and provides the model and property name to the dom for the javascript to handle.

The javascript componenet of watch uses jquery to loop through the afformentioned `<span>`s and generate a payload that lets watch know what to update. This payload is sent via a get request every 3 seconds to the `/watch` endpoint, who responds with a JSON payload that contains the updated attributes, which watch then bubbles to the DOM.

#### Things that suck

- `watch-ajax` continuously polls the server, which continuously loads from the database. So it is far from an efficient implementation. Future wants are to do a server-sent events implementation, and somhow integrate with ActiveModel::Dirty to only send the payload when watched properties are updated. (and only send the updated property)
- Doesn't handle helpers well atm

 
#### Contributing

Have something to add? Fork and submit a pull request. Topic branches get bonus points. Keep commits small and focused.
