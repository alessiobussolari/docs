# Stringhe

* Preferire l'interpolazione e la formattazione delle stringhe invece della concatenazione delle stringhe

```ruby
# bad
email_with_name = user.name + " <" + user.email + ">"

# good
email_with_name = "#{user.name} <#{user.email}>"

# good
email_with_name = format("%s <%s>", user.name, user.email)
```

* Evitare la spaziatura tra parentesi graffe nelle espressioni interpolate.

```ruby
# bad
"From: #{ user.first_name }, #{ user.last_name }"

# good
"From: #{user.first_name}, #{user.last_name}"
```

* Utilizzare stringhe con doppi apici.

```ruby
# bad
'Just some text'
'No special chars or interpolation'

# good
"Just some text"
"No special chars or interpolation"
"Every string in #{project} uses double_quotes"
```

*   Evitare la sintassi letterale dei caratteri <mark style="color:red;">`?x`</mark>.


* Usare <mark style="color:red;">`{}`</mark> intorno alle variabili di istanza e globali che vengono interpolate in una stringa.

```ruby
class Person
  attr_reader :first_name, :last_name

  def initialize(first_name, last_name)
    @first_name = first_name
    @last_name = last_name
  end

  # bad - valid, but awkward
  def to_s
    "#@first_name #@last_name"
  end

  # good
  def to_s
    "#{@first_name} #{@last_name}"
  end
end

$global = 0
# bad
puts "$global = #$global"

# fine, but don't use globals
puts "$global = #{$global}"y
```

* Evitare <mark style="color:red;">`Object#to_s`</mark> sugli oggetti interpolati.

```ruby
# bad
message = "This is the #{result.to_s}."

# good - `result.to_s` is called implicitly.
message = "This is the #{result}."
```

* Evitare <mark style="color:red;">`String#gsub`</mark> negli scenari in cui è possibile utilizzare un'alternativa più veloce e specializzata.

```ruby
url = "http://example.com"
str = "lisp-case-rules"

# bad
url.gsub("http://", "https://")
str.gsub("-", "_")
str.gsub(/[aeiou]/, "")

# good
url.sub("http://", "https://")
str.tr("-", "_")
str.delete("aeiou")
```

* Quando si usano gli heredoc per le stringhe multilinea, bisogna tenere presente che essi conservano gli spazi bianchi iniziali. È buona norma utilizzare un margine in base al quale tagliare gli spazi bianchi in eccesso.

```ruby
code = <<-END.gsub(/^\s+\|/, "")
  |def test
  |  some_method
  |  other_method
  |end
END
# => "def test\n  some_method\n  other_method\nend\n"

# In Rails you can use `#strip_heredoc` to achieve the same result
code = <<-END.strip_heredoc
  def test
    some_method
    other_method
  end
END
# => "def test\n  some_method\n  other_method\nend\n"
```

* In Ruby 2.3, si preferisce la sintassi <mark style="color:red;">"squiggly heredoc"</mark>, che ha la stessa semantica di <mark style="color:red;">`strip_heredoc`</mark> di Rails:

```ruby
code = <<~END
  def test
    some_method
    other_method
  end
END
# => "def test\n  some_method\n  other_method\nend\n"
```

* Indentare il contenuto di heredoc e la chiusura in base all'apertura.

```ruby
# bad
class Foo
  def bar
    <<~SQL
      'Hi'
  SQL
  end
end

# good
class Foo
  def bar
    <<~SQL
      'Hi'
    SQL
  end
end
```
