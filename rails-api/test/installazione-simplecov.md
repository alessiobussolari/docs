# Installazione Simplecov

Aggiungere al file <mark style="color:red;">`rails_helper.rb`</mark>

```ruby
require 'simplecov'
require 'simplecov_json_formatter'

FORMATTERS = [
               SimpleCov::Formatter::HTMLFormatter,
               SimpleCov::Formatter::JSONFormatter
             ].freeze

SimpleCov.formatters = SimpleCov::Formatter::MultiFormatter.new(FORMATTERS)
SimpleCov.start 'rails' do
  add_group 'Serializers', 'app/serializers'
  add_group 'Services', 'app/services'
  add_group 'Models', 'app/models'
  minimum_coverage 100
  add_filter '/spec/'
  add_filter '/lib'
  add_filter '/app/controllers/api/shared'
end
```
