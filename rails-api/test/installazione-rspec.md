# Installazione Rspec

Dopo aver eseguito il passaggio precedente ed aver installato le gemme per la suite di test inizializzare rspec con il comando sottostante

```
rspec --init
```

Apri il file <mark style="color:red;">`.rspec`</mark> e sovrascrivi il contenuto con

{% code title=".rspec" %}
```
--require rails_helper
--format documentation
--order random
--color

```
{% endcode %}

Apri il file <mark style="color:red;">`spec/spec_helper.rb`</mark> e sovrascrivi il contenuto con

{% code title="spec/spec_helper.rb" %}
```ruby
require 'webmock/rspec'

RSpec.configure do |config|
  config.expect_with :rspec do |expectations|
    expectations.syntax                                               = :expect
    expectations.include_chain_clauses_in_custom_matcher_descriptions = true
    expectations.max_formatted_output_length                          = 1_000_000
  end

  config.mock_with :rspec do |mocks|
    mocks.syntax                 = :expect
    mocks.verify_partial_doubles = true
  end

  config.raise_errors_for_deprecations!
  config.disable_monkey_patching!

  config.default_formatter = 'doc' if config.files_to_run.one?
  config.filter_run_when_matching(:focus)

  config.silence_filter_announcements = true
  config.fail_if_no_examples          = true
  config.warnings                     = false
  config.raise_on_warning             = true
  config.threadsafe                   = true

  config.shared_context_metadata_behavior = :apply_to_host_groups
  config.order                            = :random
  Kernel.srand(config.seed)
end

```
{% endcode %}

Apri il file <mark style="color:red;">`spec/rails_helper.rb`</mark> e sovrascrivi il contenuto con

```ruby
# This file is copied to spec/ when you run 'rails generate rspec:install'
require 'spec_helper'
ENV['RAILS_ENV'] ||= 'test'

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

require File.expand_path('../config/environment', __dir__)
# Prevent database truncation if the environment is production
abort('The Rails environment is running in production mode!') unless Rails.env.test?

require 'rspec/rails'

# PLUGIN import
require 'plugins/database_cleaner'
# require 'plugins/devise'
require 'plugins/shoulda_matchers'
require 'plugins/factory_bot'
require 'plugins/rspec_benchmark'
require 'plugins/vcr'

DatabaseCleaner.url_allowlist = %w[postgres://postgres:postgres@postgres postgres://postgres:postgres@localhost]

begin
  ActiveRecord::Migration.maintain_test_schema!
rescue ActiveRecord::PendingMigrationError => e
  puts e.to_s.strip
  exit 1
end

RSpec.configure do |config|
  config.fixture_path               = "#{::Rails.root}/spec/factories"
  config.use_transactional_fixtures = true
  config.infer_spec_type_from_file_location!
  config.filter_rails_from_backtrace!
  # devise spec helpers like sign_in
  config.include(Devise::Test::IntegrationHelpers, type: :request)
end

```

Aggiungere, in <mark style="color:red;">`config/initializers/generators.rb`</mark> queste configurazioni per bloccare la generazione automatica dei test da parte di Rails.

```ruby
config.generators do |g|
  g.test_framework :rspec,
                   fixtures:         true,
                   view_specs:       false,
                   helper_specs:     false,
                   routing_specs:    false,
                   controller_specs: false,
                   request_specs:    false
end
```
