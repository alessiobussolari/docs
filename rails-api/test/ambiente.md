# Ambiente

Lista delle gemme per l'ambiente di test

```ruby
group :test do
    # ...
    gem 'rspec', '~> 3'
    gem 'simplecov', '~> 0'
    gem 'database_cleaner', '~> 2'
    gem 'timecop', '~> 0'
    gem 'factory_bot', '~> 6'
    gem 'factory_trace', '~> 1'
    gem 'shoulda-matchers', '~> 5'
    gem 'webmock', '~> 3'
    gem 'vcr', '~> 6.0'
end
```

Lista delle gemme per l'ambiente di development

```ruby
group :development do
    # ...
    gem 'gem_updater', '~> 5'
end
```

Per entrambi gli ambienti

```ruby
group :development, :test do
  # ...
  gem 'annotate', '~> 3'
  gem 'bullet', '~> 7'
  gem 'ffaker', '~> 2'
  gem 'letter_opener_web', '~> 2'
  
  gem 'rubocop', '~> 1', require: false
  gem 'rubocop-performance', '~> 1', require: false
  gem 'rubocop-rails', '~> 2', require: false
  gem 'rubocop-rspec', '~> 2', require: false
end
```

