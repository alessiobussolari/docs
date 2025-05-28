# Collezioni

* Utilizzare la notazione letterale per la creazione di array e hash, a meno che non sia necessario passare parametri ai loro costruttori.

```ruby
# bad
arr = Array.new
hash = Hash.new

# good
arr = []
hash = {}
```

* Preferire la sintassi dell'array estesa rispetto a <mark style="color:red;">`%w`</mark> o <mark style="color:red;">`%i`</mark>.

```ruby
# bad
STATES = %w(draft open closed)

# good
STATES = ["draft", "open", "closed"]
```

* Aggiunge una virgola finale nei letterali di raccolta multilinea.

```ruby
# bad
{
  foo: :bar,
  baz: :toto
}

# good
{
  foo: :bar,
  baz: :toto,
}
```

* Quando si accede al primo o all'ultimo elemento di un array, preferire <mark style="color:red;">`first`</mark> o <mark style="color:red;">`last`</mark> a <mark style="color:red;">`[0]`</mark> o <mark style="color:red;">`[-1]`</mark>.&#x20;
* Evitare oggetti mutabili come chiavi di hash.&#x20;
* Usare una sintassi hash abbreviata quando tutte le chiavi sono simboli.

```ruby
# bad
{ :a => 1, :b => 2 }

# good
{ a: 1, b: 2 }
```

* Preferire la arrow syntax per gli hash a quella abbreviata quando non tutte le chiavi sono simboli.

```ruby
# bad
{ a: 1, "b" => 2 }

# good
{ :a => 1, "b" => 2 }
```

* Preferire <mark style="color:red;">`Hash#key?`</mark> a <mark style="color:red;">`Hash#has_key?`</mark>.&#x20;
* Preferire <mark style="color:red;">`Hash#value?`</mark> a <mark style="color:red;">`Hash#has_value?`</mark>.&#x20;
* Usare <mark style="color:red;">`Hash#fetch`</mark> quando si tratta di chiavi hash che dovrebbero essere presenti.

```ruby
heroes = { batman: "Bruce Wayne", superman: "Clark Kent" }
# bad - if we make a mistake we might not spot it right away
heroes[:batman] # => "Bruce Wayne"
heroes[:supermann] # => nil

# good - fetch raises a KeyError making the problem obvious
heroes.fetch(:supermann)
```

* Introdurre valori predefiniti per le chiavi hash tramite <mark style="color:red;">`Hash#fetch`</mark>, invece di usare una logica personalizzata.

```ruby
batman = { name: "Bruce Wayne", is_evil: false }

# bad - if we just use || operator with falsy value we won't get the expected result
batman[:is_evil] || true # => true

# good - fetch work correctly with falsy values
batman.fetch(:is_evil, true) # => false
```

* Collocare <mark style="color:red;">`]`</mark> e <mark style="color:red;">`}`</mark> sulla riga dopo l'ultimo elemento quando la parentesi graffa di apertura si trova su una riga separata dal primo elemento.

```ruby
# bad
[
  1,
  2]

{
  a: 1,
  b: 2}

# good
[
  1,
  2,
]

{
  a: 1,
  b: 2,
}
```
