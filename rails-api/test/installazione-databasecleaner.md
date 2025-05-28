# Installazione DatabaseCleaner

Crea il file <mark style="color:red;">`spec/plugins/database-cleaner.rb`</mark> e aggiungilo al file <mark style="color:red;">`rails_helper.rb`</mark>

```ruby
require 'plugins/database-cleaner'
```

Nello stesso file aggiungere il codice sottostante

```ruby
# frozen_string_literal: true

require 'database_cleaner/active_record'

RSpec.configure do |config|
  config.before(:suite) do
    DatabaseCleaner.clean_with(:truncation)
    DatabaseCleaner.strategy = :transaction

    DatabaseCleaner.start
  end

  config.around do |example|
    DatabaseCleaner.cleaning do
      example.run
    end
  end
end
```
