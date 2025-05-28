# Installazione FactoryBot

Crea il file <mark style="color:red;">`spec/plugins/factory_bot.rb`</mark> e aggiungilo al file <mark style="color:red;">`rails_helper.rb`</mark>

```ruby
require 'plugins/factory_bot'
```

Nello stesso file aggiungere le config dentro al blocco <mark style="color:red;">`RSpec`</mark>

```ruby
RSpec.configure do |config|
  config.include FactoryBot::Syntax::Methods
end
```
