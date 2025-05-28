# Layout

* Utilizzare <mark style="color:red;">`UTF-8`</mark> come codifica del file sorgente.&#x20;
* Utilizzare un rientro di 2 spazi, senza tabulazioni.&#x20;
* Usare terminazioni di riga in stile Unix.&#x20;
* Evitare l'uso di <mark style="color:red;">`;`</mark> per separare dichiarazioni ed espressioni.&#x20;
* Usare un'espressione per riga.&#x20;
* Usare gli spazi intorno agli operatori, dopo le virgole, i punti e le virgole, intorno a <mark style="color:red;">`{`</mark> e prima di <mark style="color:red;">`}`</mark>.&#x20;
* Evitare gli spazi dopo <mark style="color:red;">`(`</mark>, <mark style="color:red;">`[`</mark> e prima di <mark style="color:red;">`]`</mark>, <mark style="color:red;">`)`</mark>.&#x20;
* Evitare gli spazi dopo l'operatore <mark style="color:red;">`!`</mark>&#x20;
* Evitare gli spazi all'interno dei letterali di intervallo.&#x20;
* Evitare gli spazi intorno agli operatori di chiamata di metodo.

```ruby
# bad
foo . bar

# good
foo.bar
```

* Evitare lo spazio nelle dichiarazioni lambda.

```ruby
# bad
a = -> (x, y) { x + y }

# good
a = ->(x, y) { x + y }
```

* Indentare <mark style="color:red;">`when`</mark> allo stesso livello del <mark style="color:red;">`case`</mark>.&#x20;
* Quando si assegna il risultato di un'espressione condizionale a una variabile, allineare i suoi rami con la variabile che riceve il valore di ritorno.

```ruby
# bad
result =
  if some_cond
    # ...
    # ...
    calc_something
  else
    calc_something_else
  end

# good
result = if some_cond
  # ...
  # ...
  calc_something
else
  calc_something_else
end
```

* Quando si assegna il risultato di un blocco <mark style="color:red;">`begin`</mark>, allineare <mark style="color:red;">`rescue/ensure/end`</mark> con l'inizio della riga.

```ruby
# bad
host = begin
         URI.parse(value).host
       rescue URI::Error
         nil
       end

# good
host = begin
  URI.parse(value).host
rescue URI::Error
  nil
end
```

* Usare linee vuote tra le definizioni dei metodi e anche per suddividere internamente i metodi in paragrafi logici.&#x20;
* Usare gli spazi intorno all'operatore <mark style="color:red;">`=`</mark> quando si assegnano valori predefiniti ai parametri dei metodi.&#x20;
* Evitare la continuazione di riga con <mark style="color:red;">`\`</mark> quando non è necessaria.&#x20;
* Allineare i parametri di una chiamata di metodo, se si estendono su più righe, con un livello di rientro rispetto all'inizio della riga con la chiamata di metodo.

```ruby
# starting point (line is too long)
def send_mail(source)
  Mailer.deliver(to: "io@pandev.it", from: "noi@pandev.it", subject: "Messaggio importante", body: source.text)
end

# bad (double indent)
def send_mail(source)
  Mailer.deliver(
      to: "io@pandev.it",
      from: "noi@pandev.it",
      subject: "Messaggio importante",
      body: source.text)
end

# good
def send_mail(source)
  Mailer.deliver(
    to: "io@pandev.it",
    from: "noi@pandev.it",
    subject: "Messaggio importante",
    body: source.text,
  )
```

* Quando si concatenano metodi su più righe, indentare le chiamate successive di un livello di indentazione.

```ruby
# bad (indented to the previous call)
User.pluck(:name)
    .sort(&:casecmp)
    .chunk { |n| n[0] }

# good
User
  .pluck(:name)
  .sort(&:casecmp)
  .chunk { |n| n[0] }
```

* Allinea gli elementi delle matrici letterali che si estendono su più righe.&#x20;
* Limitare le righe a 120 caratteri.&#x20;
* Evitare gli spazi bianchi di coda.&#x20;
* Evitare spazi bianchi aggiuntivi, tranne che per l'allineamento.&#x20;
* Terminare ogni file con una newline.&#x20;
* Evitare i commenti a blocchi.

```ruby
# bad
=begin
comment line
another comment line
=end

# good
# comment line
# another comment liney
```

* Posizionare la parentesi di chiusura del metodo sulla riga dopo l'ultimo argomento quando la parentesi  di apertura si trova su una riga separata dal primo argomento.

```ruby
# bad
method(
  arg_1,
  arg_2)

# good
method(
  arg_1,
  arg_2,
)
```

* Separare i commenti magici dal codice e dalla documentazione con una riga vuota.

```ruby
# bad
# frozen_string_literal: true
# Some documentation for Person
class Person
  # Some code
end

# good
# frozen_string_literal: true

# Some documentation for Person
class Person
  # Some code
end
```

* Utilizzare linee vuote intorno agli accessi agli attributi.

```ruby
# bad
class Foo
  attr_reader :foo
  def foo
    # do something...
  end
end

# good
class Foo
  attr_reader :foo

  def foo
    # do something...
  end
end
```

* Evitare le righe vuote intorno ai corpi dei metodi, delle classi, dei moduli e dei blocchi.

```ruby
# bad
class Foo

  def foo

    begin

      do_something do

        something

      end

    rescue

      something

    end

    true

  end

end

# good
class Foo
  def foo
    begin
      do_something do
        something
      end
    rescue
      something
    end
  end
end
```
