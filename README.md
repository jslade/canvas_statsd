# CanvasStatsd

configurable statsd client proxy

## Configuration

Set a few enviroment variables:


```bash

export CANVAS_STATSD_HOST=statsd.example.org
export CANVAS_STATSD_PORT=1234
export CANVAS_STATSD_NAMESPACE=my_app.prod
export CANVAS_STATSD_APPEND_HOSTNAME=false

```


Or pass a hash to `CanvasStatsd.settings`

```ruby

settings = {
  host: 'statsd.example.org'
  port: 1234
  namespace: 'my_app.prod'
  append_hostname: false
}

CanvasStatsd.settings = settings

```

Values passed to `CanvasStatsd.settings` will take precedence over ENV vars


## Configuration Options

Only the `host` (or `CANVAS_STATSD_HOST` ENV var) is required, all other config
is optional

##### `host`

Location of the statsd box you want to send stats to.

##### `port`

port of the statsd box you want to send stats to.


##### `namespace`

If a namespace is defined, it'll be prepended to the stat name. So the following:

```ruby

settings = {
  host: 'statsd.example.org'
  namespace: 'my_app.prod'
}

CanvasStatsd.settings = settings

CanvasStatsd::Statsd.timing('some.stat', 300)

```

would use `my_app.prod.some.stat` as it's stat name.


##### `append_hostname`

The hostname of the server will be appended to the stat name, unless
`append_hostname: false` is specified. So if the namespace is `canvas` and the
hostname is `app01`, the final stat name of `my_stat` would be
`canvas.my_stat.app01` (assuming the default statsd/graphite configuration)


## Usage

Outside of configuration, app code generally interacts with the
`CanvasStatsd::Statsd` object, which is a proxy class to communicate messages
to statsd.

Available statsd messages are described in:

* [etsty/statsd README](https://github.com/etsy/statsd/blob/master/README.md)
* [reinh/statsd source](https://github.com/reinh/statsd/blob/master/lib/statsd.rb)

So for instance:

```ruby
ms = Benchmark.ms { ..code.. }
CanvasStatsd::Statsd.timing("my_stat", ms)
```

If statsd isn't configured and enabled, then calls to `CanvasStatsd::Statsd.*`
will do nothing and return nil.


## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request