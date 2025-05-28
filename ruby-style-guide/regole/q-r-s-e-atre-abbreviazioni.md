# %q %r %s e atre abbreviazioni

*   Usare <mark style="color:red;">`%()`</mark> per le stringhe a una riga che richiedono sia l'interpolazione che i doppi apici incorporati. Per le stringhe a più righe, preferire gli heredoc.


*   Evitate <mark style="color:red;">`%q`</mark> a meno che non abbiate una stringa che contiene sia <mark style="color:red;">`'`</mark> che <mark style="color:red;">`"`</mark>. I letterali delle stringhe regolari sono più leggibili e dovrebbero essere preferiti, a meno che non debbano essere evasi molti caratteri.


* Usare <mark style="color:red;">`%r`</mark> solo per le espressioni regolari che corrispondono ad almeno un carattere <mark style="color:red;">`/`</mark>.

```ruby
# bad
%r{\s+}

# good
%r{^/(.*)$}
%r{^/blog/2011/(.*)$}
```

*   Evitare l'uso di <mark style="color:red;">`%s`</mark>. Usare <mark style="color:red;">`:"qualche stringa"`</mark> per creare un simbolo con spazi al suo interno.


* Preferite <mark style="color:red;">`()`</mark> come delimitatore per tutti i letterali <mark style="color:red;">`%`</mark>, tranne, come spesso accade nelle espressioni regolari, quando le parentesi appaiono all'interno del letterale. Utilizzare il primo di <mark style="color:red;">`()`</mark>, <mark style="color:red;">`{}`</mark>, <mark style="color:red;">`[]`</mark>, <mark style="color:red;">`<>`</mark> che non compare all'interno del letterale.
