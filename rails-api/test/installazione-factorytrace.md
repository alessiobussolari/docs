# Installazione FactoryTrace

Crea il file <mark style="color:red;">`spec/plugins/factory_trace.rb`</mark> e aggiungilo al file <mark style="color:red;">`rails_helper.rb`</mark>

```ruby
require 'plugins/factory_trace'
```

Nello stesso file aggiungere le config dentro al blocco <mark style="color:red;">`FactoryTrace`</mark>

```ruby
FactoryTrace.configure do |config|
  # default ENV.key?('FB_TRACE') || ENV.key?('FB_TRACE_FILE')
  config.enabled = true

  # default is ENV['FB_TRACE_FILE']
  # when nil outputs to STDOUT
  config.path = 'log/factory_trace.txt'

  # default is true when +path+ is nil
  config.color = true

  # default is ENV['FB_TRACE'] || :full
  # can be :full or :trace_only
  config.mode = :full

  # used to trace definitions places
  # default is true
  # can be true or false  
  config.trace_definition = true
end
```
