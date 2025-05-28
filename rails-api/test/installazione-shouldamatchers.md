# Installazione ShouldaMatchers

Crea il file <mark style="color:red;">`spec/plugins/shoulda-matchers.rb`</mark> e aggiungilo al file <mark style="color:red;">`rails_helper.rb`</mark>

```ruby
require 'plugins/shoulda-matchers'
```

Nello stesso file aggiungere le config dentro al blocco <mark style="color:red;">`Shoulda::Matchers`</mark>

```ruby
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
```

A [questo link](https://matchers.shoulda.io/docs/v5.1.0/) puoi trovare la documentazione di tutti i matchers.
