# Eccezioni

* Segnalare le eccezioni con il metodo <mark style="color:red;">`raise`</mark>.&#x20;
* Omettere <mark style="color:red;">`RuntimeError`</mark> nella versione a due parametri di <mark style="color:red;">`raise`</mark>.

```ruby
# bad
raise RuntimeError, "message"

# good - signals a RuntimeError by default
raise "message"
```

* Preferisce fornire una classe di eccezione e un messaggio come due argomenti separati da <mark style="color:red;">`raise`</mark>, invece di un'istanza di eccezione.

```ruby
# bad
raise SomeException.new("message")
# Note that there is no way to do `raise SomeException.new("message"), backtrace`.

# good
raise SomeException, "message"
# Consistent with `raise SomeException, "message", backtrace`.
```

* Evitare il ritorno da un blocco <mark style="color:red;">`ensure`</mark>. Se si ritorna esplicitamente da un metodo all'interno di un blocco <mark style="color:red;">`ensure`</mark>, il ritorno avrà la precedenza su qualsiasi eccezione sollevata e il metodo ritornerà come se non fosse stata sollevata alcuna eccezione. In effetti, l'eccezione sarà gettata via silenziosamente.

```ruby
# bad
def foo
  raise
ensure
  return "very bad idea"
end
```

* Utilizzare blocchi di <mark style="color:red;">`begin`</mark> impliciti, ove possibile.

```ruby
# bad
def foo
  begin
    # main logic goes here
  rescue
    # failure handling goes here
  end
end

# good
def foo
  # main logic goes here
rescue
  # failure handling goes here
end
```

* Evitare dichiarazioni <mark style="color:red;">`rescue`</mark> vuote.

```ruby
# bad
begin
  # an exception occurs here
rescue SomeError
  # the rescue clause does absolutely nothing
end

# bad - `rescue nil` swallows all errors, including syntax errors, and
# makes them hard to track down.
do_something rescue nil
```

* Evitare il <mark style="color:red;">`rescue`</mark> nella sua forma di modificatore.

```ruby
# bad - this catches exceptions of StandardError class and its descendant
# classes.
read_file rescue handle_error($!)

# good - this catches only the exceptions of Errno::ENOENT class and its
# descendant classes.
def foo
  read_file
rescue Errno::ENOENT => error
  handle_error(error)
end
```

* Evitare di effettuare il catch della classe <mark style="color:red;">`Exception`</mark>.

```ruby
# bad
begin
  # calls to exit and kill signals will be caught (except kill -9)
  exit
rescue Exception
  puts "you didn't really want to exit, right?"
  # exception handling
end

# good
begin
  # a blind rescue rescues from StandardError, not Exception.
rescue => error
  # exception handling
end
```

* Preferire le eccezioni della libreria standard all'introduzione di nuove classi di eccezioni.&#x20;
* Utilizzare nomi significativi per le variabili di eccezione.

```ruby
# bad
begin
  # an exception occurs here
rescue => e
  # exception handling
end

# good
begin
  # an exception occurs here
rescue => error
  # exception handling
end
```
