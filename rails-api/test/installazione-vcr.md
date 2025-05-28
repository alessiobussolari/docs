# Installazione VCR

Crea il file <mark style="color:red;">`spec/plugins/vcr_config.rb`</mark> e aggiungilo al file <mark style="color:red;">`rails_helper.rb`</mark>

{% code title="rails_helper.rb" %}
```ruby
require 'plugins/vcr-config'
```
{% endcode %}

Aggiungere al file <mark style="color:red;">`spec_helper.rb`</mark>

{% code title="spec_helper.rb" %}
```ruby
require 'webmock/rspec'
```
{% endcode %}

Nello stesso file aggiungere il codice sottostante

{% code title="vcr_config.rb" %}
```ruby
# frozen_string_literal: true

require 'vcr'

# rubocop:disable Layout/SpaceAroundOperators
VCR.configure do |config|
  config.cassette_library_dir                    = 'spec/vcr/cassettes'
  config.hook_into :webmock
  config.configure_rspec_metadata!
  config.ignore_localhost                        = true
  config.allow_http_connections_when_no_cassette = true
end
# rubocop:enable Layout/SpaceAroundOperators
```
{% endcode %}
