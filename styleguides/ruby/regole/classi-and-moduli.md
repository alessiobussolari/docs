# Classi & Moduli

* Preferire i moduli alle classi con i soli metodi della classe.
* Le classi dovrebbero essere usate solo quando ha senso creare istanze da esse. Preferire <mark style="color:red;">`extend self`</mark> a <mark style="color:red;">`module_function`</mark>.

```ruby
# bad
module SomeModule
  module_function

  def some_method
  end

  def some_other_method
  end
end

# good
module SomeModule
  extend self

  def some_method
  end

  def some_other_method
  end
end
```

* Quando si definiscono i metodi di una classe, utilizzare il blocco <mark style="color:red;">`class << self`</mark> piuttosto che <mark style="color:red;">`def self`</mark>. e raggrupparli in un unico blocco.

```ruby
# bad
class SomeClass
  def self.method1
  end

  def method2
  end

  private

  def method3
  end

  def self.method4 # this is actually not private
  end
end

# good
class SomeClass
  class << self
    def method1
    end

    private

    def method4
    end
  end

  def method2
  end

  private

  def method3
  end
enby
```

* Rispettare il [principio di sostituzione di Liskov](../approfondimenti/principio-di-sostituzione-di-liskov.md) quando si progettano le gerarchie di classi.&#x20;
* Usare <mark style="color:red;">`attr_accessor`</mark>, <mark style="color:red;">`attr_reader`</mark> e <mark style="color:red;">`attr_writer`</mark> per definire accessori e mutatori banali.

```ruby
# bad
class Person
  def initialize(first_name, last_name)
    @first_name = first_name
    @last_name = last_name
  end

  def first_name
    @first_name
  end

  def last_name
    @last_name
  end
end

# good
class Person
  attr_reader :first_name, :last_name

  def initialize(first_name, last_name)
    @first_name = first_name
    @last_name = last_name
  end
end
```

* Preferire <mark style="color:red;">`attr_reader`</mark> e <mark style="color:red;">`attr_accessor`</mark> ad <mark style="color:red;">`attr`</mark>.&#x20;
* Evitare le variabili di classe (<mark style="color:red;">`@@`</mark>).&#x20;
* Preferire <mark style="color:red;">`alias_method`</mark> a <mark style="color:red;">`alias`</mark>.
* Indentare i metodi <mark style="color:red;">`public`</mark>, <mark style="color:red;">`protected`</mark> e <mark style="color:red;">`private`</mark> tanto quanto le definizioni dei metodi a cui si applicano. Lasciare una riga vuota sopra il modificatore di visibilità e una riga vuota sotto di esso.

```ruby
class SomeClass
  def public_method
    # ...
  end

  private

  def private_method
    # ...
  end

  def another_private_method
    # ...
  end
end
```
