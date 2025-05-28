---
description: >-
  Paragrafo sul come gestire i nomi delle variabili, costanti, metodi e altre
  descrizioni all'interno del codice
---

# Denominazione

* Usare <mark style="color:red;">`snake_case`</mark> per simboli, metodi e variabili.&#x20;
* Usare <mark style="color:red;">`CamelCase`</mark> per classi e moduli, ma mantenere acronimi come HTTP, RFC, XML in maiuscolo.&#x20;
* Usare <mark style="color:red;">`snake_case`</mark> per nominare i file e le directory, per esempio <mark style="color:red;">`hello_world.rb`</mark>.&#x20;
* Definire una sola classe o modulo per ogni file sorgente.&#x20;
* Nominare il nome del file come la classe o il modulo, ma sostituendo <mark style="color:red;">`CamelCase`</mark> con <mark style="color:red;">`snake_case`</mark>.&#x20;
* Usare <mark style="color:red;">`SCREAMING_SNAKE_CASE`</mark> per le altre costanti.&#x20;
* Quando si usa inject con blocchi brevi, nominare gli argomenti in base a ciò che viene iniettato, ad esempio <mark style="color:red;">`|hash, e|`</mark> (mnemonico: hash, element).&#x20;
* Quando si definiscono operatori binari, nominare il parametro <mark style="color:red;">`other`</mark> (<mark style="color:red;">`<<`</mark> e <mark style="color:red;">`[]`</mark> sono eccezioni alla regola, poiché la loro semantica è diversa).&#x20;
* Nominare i metodi di predicato con un <mark style="color:red;">`?`</mark> I metodi di predicato sono metodi che restituiscono un valore booleano.&#x20;
* Evitare di terminare i nomi dei metodi con un <mark style="color:red;">`?`</mark> se non restituiscono un valore booleano.&#x20;
* Evitare di anteporre ai nomi dei metodi il prefisso <mark style="color:red;">`is_`</mark>.

```ruby
# bad
def is_empty?
end

# good
def empty?
end
```

* Evitare di iniziare i nomi dei metodi con <mark style="color:red;">`get_`</mark>.&#x20;
* Evitare di terminare i nomi dei metodi con <mark style="color:red;">`!`</mark> quando non esiste un metodo equivalente senza il bang. I bang servono a contrassegnare una versione più pericolosa di un metodo, ad esempio <mark style="color:red;">`save`</mark> restituisce un booleano in ActiveRecord, mentre <mark style="color:red;">`save!`</mark> lancia un'eccezione in caso di fallimento.&#x20;
* Evitare i numeri magici.&#x20;
* Usare una costante e darle un nome significativo. Evitare una nomenclatura che abbia (o possa essere interpretata come) un'origine discriminatoria.
