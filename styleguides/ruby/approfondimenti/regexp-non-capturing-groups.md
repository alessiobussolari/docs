# Regexp non-capturing groups

Consideriamo questa stringa.

```
https://stackoverflow.com/questions/tagged/regex
```

Se applichiamo questa regexp.

```
(https?|ftp)://([^/\r\n]+)(/[^\r\n]*)?
```

I risultati saranno.

```
Match "http://stackoverflow.com/"
     Group 1: "http"
     Group 2: "stackoverflow.com"
     Group 3: "/"

Match "https://stackoverflow.com/questions/tagged/regex"
     Group 1: "https"
     Group 2: "stackoverflow.com"
     Group 3: "/questions/tagged/regex"
```

A questo punto se non ci interessa il protocollo è meglio utilizzare il non-capturing groups <mark style="color:red;">`?:`</mark>

```
(?:https?|ftp)://([^/\r\n]+)(/[^\r\n]*)?
```

Cosi il risultato finale sarà:

```
Match "http://stackoverflow.com/"
     Group 1: "stackoverflow.com"
     Group 2: "/"

Match "https://stackoverflow.com/questions/tagged/regex"
     Group 1: "stackoverflow.com"
     Group 2: "/questions/tagged/regex"
```
