# Espressioni regolari

* Preferire la ricerca in testo normale alle espressioni regolari nelle stringhe.

```ruby
string["text"]
```

* Utilizzare non-capturing groups quando non si utilizza il risultato trovato.

{% hint style="info" %}
I capturing groups sono un modo per trattare più caratteri come un'unica unità. Vengono creati inserendo i caratteri da raggruppare all'interno di una serie di parentesi. Ad esempio, l'espressione regolare (cane) crea un singolo gruppo contenente le lettere "c" "a", "n" e "e". [Per approfondimenti clicca qui.](../approfondimenti/regexp-non-capturing-groups.md)
{% endhint %}

```ruby
# bad
/(first|second)/

# good
/(?:first|second)/
```

* Preferire <mark style="color:red;">`Regexp#match`</mark> alle variabili Perl-legacy per catturare le corrispondenze di gruppo.

```ruby
# bad
/(regexp)/ =~ string
process $1

# good
/(regexp)/.match(string)[1]
```

* Preferire i gruppi denominati a quelli numerati.

```ruby
# bad
/(regexp)/ =~ string
...
process Regexp.last_match(1)

# good
/(?<meaningful_var>regexp)/ =~ string
...
process meaningful_var
```

* Preferire <mark style="color:red;">`\A`</mark> e <mark style="color:red;">`\z`</mark> rispetto a <mark style="color:red;">`^`</mark> e <mark style="color:red;">`$`</mark> quando si confrontano le stringhe dall'inizio alla fine.

```ruby
string = "some injection\nusername"
string[/^username$/] # `^` and `$` matches start and end of lines.
string[/\Ausername\z/] # `\A` and `\z` matches start and end of strings.
```
