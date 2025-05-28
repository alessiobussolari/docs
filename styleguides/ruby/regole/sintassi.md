# Sintassi

* Usare <mark style="color:red;">`::`</mark> solo per fare riferimento a costanti (incluse classi e moduli) e costruttori (come <mark style="color:red;">`Array()`</mark> o <mark style="color:red;">`Nokogiri::HTML()`</mark>).&#x20;
* Evitare <mark style="color:red;">`::`</mark> per le normali invocazioni di metodi.&#x20;
* Evitare l'uso di <mark style="color:red;">`::`</mark> per definire classi e moduli o per l'ereditarietà, poiché la ricerca delle costanti non viene effettuata nelle classi/moduli padre.

```ruby
# bad
module A
  FOO = "test"
end

class A::B
  puts FOO  # this will raise a NameError exception
end

# good
module A
  FOO = "test"

  class B
    puts FOO
  end
end
```

* Usare def con le parentesi quando ci sono dei parametri.&#x20;
* Omettere le parentesi quando il metodo non accetta alcun parametro.&#x20;
* Evitare il metodo <mark style="color:red;">`for`</mark>.&#x20;
* Evitare <mark style="color:red;">`then`</mark>.&#x20;
* Preferite l'operatore ternario(<mark style="color:red;">`?:`</mark>) ai costrutti <mark style="color:red;">`if`</mark>/<mark style="color:red;">`then`</mark>/<mark style="color:red;">`else`</mark>/<mark style="color:red;">`end`</mark>.

```ruby
# bad
result = if some_condition then something else something_else end

# good
result = some_condition ? something : something_else
```

* In un operatore ternario si usa un'espressione per ramo. Questo significa anche che gli operatori ternari non devono essere annidati. In questi casi, preferire i costrutti <mark style="color:red;">`if`</mark>/<mark style="color:red;">`else`</mark>.&#x20;
* Evitate gli operatori ternari multilinea <mark style="color:red;">`?:`</mark>; usate invece <mark style="color:red;">`if`</mark>/<mark style="color:red;">`unless`</mark>.&#x20;
* Usare <mark style="color:red;">`when x then ...`</mark> per i casi a una riga.
* Usare <mark style="color:red;">`!`</mark> invece di <mark style="color:red;">`not`</mark>.&#x20;
* Preferire <mark style="color:red;">`&&`</mark>/<mark style="color:red;">`||`</mark> a <mark style="color:red;">`and`</mark>/<mark style="color:red;">`or`</mark>.&#x20;
* Preferire <mark style="color:red;">`unless`</mark> a <mark style="color:red;">`if`</mark> per le condizioni negative.&#x20;
* Evitare l'uso di <mark style="color:red;">`unless`</mark> con <mark style="color:red;">`else`</mark>. Riscrivere le condizioni con il caso positivo per primo.
* Usare le parentesi intorno agli argomenti delle invocazioni di metodi.&#x20;
* Omettere le parentesi quando non si forniscono argomenti. Omettere le parentesi anche quando l'invocazione è a riga singola e il metodo:
  * è una chiamata a un metodo di classe con ricevitore implicito.&#x20;
  * è uno dei seguenti metodi:
    * <mark style="color:red;">`require`</mark>
    * <mark style="color:red;">`require_relative`</mark>
    * <mark style="color:red;">`require_dependency`</mark>
    * <mark style="color:red;">`yield`</mark>
    * <mark style="color:red;">`raise`</mark>
    * <mark style="color:red;">`puts`</mark>
  * è chiamato da uno "zucchero sintattico" (ad esempio: <mark style="color:red;">`1 + 1`</mark> chiama il metodo <mark style="color:red;">`+`</mark>, <mark style="color:red;">`foo[bar]`</mark> chiama il metodo <mark style="color:red;">`[]`</mark>, ecc.)

```ruby
# bad
class User
  include(Bar)
  has_many(:posts)
end

# good
class User
  include Bar
  has_many :posts
  SomeClass.some_method(:foo)
end
```

* Omettere le parentesi graffe esterne intorno a un hash implicito di opzioni.&#x20;
* Utilizzare l'abbreviazione di invocazione proc quando il metodo invocato è l'unica operazione di un blocco.

```ruby
# bad
names.map { |name| name.upcase }

# good
names.map(&:upcase)
```

* Preferire <mark style="color:red;">`{...}`</mark> a <mark style="color:red;">`do...end`</mark> per i blocchi a riga singola.&#x20;
* Preferire <mark style="color:red;">`do...end`</mark> a <mark style="color:red;">`{...}`</mark> per i blocchi a più righe.&#x20;
* Omettere <mark style="color:red;">`return`</mark> quando possibile.&#x20;
* Omettere <mark style="color:red;">`self`</mark> quando possibile.

```ruby
# bad
self.my_method

# good
my_method

# also good
attr_writer :name

def my_method
  self.name = "Rafael" # `self` is needed to reference the attribute writer.
end
```

* Avvolgere l'assegnazione tra parentesi quando si utilizza il suo valore di ritorno in un'istruzione condizionale.

```ruby
if (value = /foo/.match(string))
```

* Usare <mark style="color:red;">`||=`</mark> per inizializzare le variabili solo se non sono già inizializzate.&#x20;
* Evitare di usare <mark style="color:red;">`||=`</mark> per inizializzare variabili booleane.

```ruby
# bad - would set enabled to true even if it was false
@enabled ||= true

# good
@enabled = true if @enabled.nil?

# also valid - defined? workaround
@enabled = true unless defined?(@enabled)
```

* Evitare gli spazi tra il nome di un metodo e la parentesi di apertura.&#x20;
* Preferire la sintassi lambda literal a lambda.

```ruby
# bad
l = lambda { |a, b| a + b }
l.call(1, 2)

l = lambda do |a, b|
  tmp = a * 7
  tmp * b / 50
end

# good
l = ->(a, b) { a + b }
l.call(1, 2)

l = ->(a, b) do
  tmp = a * 7
  tmp * b / 50
end
```

* Preferire <mark style="color:red;">`proc`</mark> a <mark style="color:red;">`Proc.new`</mark>.&#x20;
* Prefissare i parametri di blocco non utilizzati con <mark style="color:red;">`_`</mark>. È accettabile anche usare solo <mark style="color:red;">`_`</mark>.&#x20;
* Preferite una clausola di guardia quando potete affermare dati non validi. Una clausola di guardia è una dichiarazione condizionale all'inizio di una funzione che permette di uscire dal metodo il prima possibile.

```ruby
# bad
def compute_thing(thing)
  if thing[:foo]
    update_with_bar(thing)
    if thing[:foo][:bar]
      partial_compute(thing)
    else
      re_compute(thing)
    end
  end
end

# good
def compute_thing(thing)
  return unless thing[:foo]
  update_with_bar(thing[:foo])
  return re_compute(thing) unless thing[:foo][:bar]
  partial_compute(thing)
end
```

* Preferire gli argomenti tramite parole chiave a quelli delle opzioni hash.&#x20;
* Preferire <mark style="color:red;">`map`</mark> a <mark style="color:red;">`collect`</mark>, <mark style="color:red;">`find`</mark> a <mark style="color:red;">`detect`</mark>, <mark style="color:red;">`select`</mark> a <mark style="color:red;">`find_all`</mark>, <mark style="color:red;">`size`</mark> a <mark style="color:red;">`length`</mark>.&#x20;
* Preferire <mark style="color:red;">`Time`</mark> a <mark style="color:red;">`DateTime`</mark>.&#x20;
* Preferisce <mark style="color:red;">`Time.iso8601(foo)`</mark> invece di <mark style="color:red;">`Time.parse(foo)`</mark> quando si aspettano stringhe di tempo formattate ISO8601 come <mark style="color:red;">`"2018-03-20T11:16:39-04:00"`</mark>.
