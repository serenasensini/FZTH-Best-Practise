# Cosa sono
Una **espressione regolare** (in forma ridotta, _regex_) è una sequenza di simboli (quindi una stringa) che identifica un insieme di stringhe. Un esempio è quello di rappresentare un insieme di stringhe che iniziano tutte per 'a': si verifica che se il primo carattere di ogni stringa da controllare sia pari ad 'a' o meno, e in tal caso si accetta o si rifiuta. 
Si definisce quindi quello che è un "pattern", ovvero un modello da rispettare, e lo si applica come _controllo_ a tutte le stringhe che devono essere validate.

# Quando usarle
Quando, per esempio, si vuole verificare se una stringa sia conforme rispetto ad uno standard definito: per parlare di un caso concreto, si può usare una serie di esempi: avendo un insieme di stringhe come queste:
- mail.mail.it
- esempiociaociao.it@dominio
- mail@dominio
- esempio@mail.com
è facile affermare che solo l'ultima sia una mail valida; questo perché le mail hanno una serie di caratteristiche che vanno rispettate. Come? Prosegui con la lettura. 

# Come usarle
In primis, vediamo i simboli che fanno parte della sintassi:
- il simbolo **.** rappresenta un singolo carattere, che può essere di tipo alfanumerico;
- i simboli **[]** rappresentano l'insieme entro cui deve appartenere la stringa; all'interno delle parentesi possono essere usati insiemi già definiti di caratteri, come l'insieme **a-z** per i caratteri alfabetici minuscoli, oppure **a-zA-Z** per includere anche quelli maiuscoli; così vale anche per i numeri, che possono essere scritti come **0-9**, o per la punteggiatura, con **:punct:** ;
- il simbolo **^** corrisponde all'inizio della stringa;
- il simbolo **$** rappresenta la fine della stringa;
- il simbolo __*__ rappresenta una ripetizione, di 0 o più volte, dell'espressione cercata;
- il simbolo __?__ rappresenta una ripetizione, di 0 o una volta, dell'espressione cercata;
- il simbolo **+** rappresenta una ripetizione, di 1 o più volte, dell'espressione cercata;

## Esempi:
- Cercare tutte le stringhe che finiscono in "atto" e che hanno un carattere alfabetico qualsiasi all'inizio, prendendo come input l'insieme [gatto, metto, fatto, matto, adatto, atto, supergatto]
`.atto` funziona per gatto, fatto, matto, atto, ma non per supergatto;
- Cercare tutte le stringhe che iniziano per "c", prendendo come input l'insieme [cavolo, cavolfiore, carciofo, fragola, mela]
`[^c]` restituisce, per esempio, cavolo, cavolfiore, carciofo, ma non fragola, mela, ecc...

## Uso: JavaScript
Vedi [link](https://www.w3schools.com/jsref/jsref_obj_regexp.asp)

## Uso: Python
Vedi [link](https://www.w3schools.com/python/python_regex.asp)

## Uso: Java
Vedi [link](https://www.tutorialspoint.com/java/java_regular_expressions.htm)

## Uso: R
Vedi [link](https://www.regular-expressions.info/rlanguage.html)

## Uso: PHP
Vedi [link](https://www.regular-expressions.info/php.html)

## Uso: sistemi Unix
Vedi [link](https://www.softwaretestinghelp.com/unix-regular-expressions/)
