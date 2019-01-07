# FZTH-Best-Practise

## Introduzione
Un "buon codice" significa tempo e denaro risparmiato nell'apprendimento, nella manutenzione, nel test, nel fissaggio, nell'estensione e nella scalabilità del prodotto stesso. Ciò richiede più tempo e attenzione per lo sviluppo iniziale, ma restituisce rapidamente con gli interessi l'investimento con grandi interessi.

Le seguenti linee guida non sono esaustive e sono pensate per essere applicate in aggiunta ai principi SOLID e al corretto utilizzo dei modelli di progettazione OO.

__1. Evita le classi onnipotenti, usa nomi significativi__

Quando si progetta una nuova classe o si effettua il refactoring di una esistente, uno sviluppatore dovrebbe fare un elenco di tutte le attività eseguite da quella classe e trovare un nome che rappresenti facilmente e brevemente ciò che fa la classe. Se la classe fa troppe cose, un nome rappresentativo è di solito qualcosa di mostruoso come: InfoAndConfigurationOfSomeProperties. Ciò significa che la classe ha troppe responsabilità e dovrebbe essere suddivisa in più componenti, idealmente, con una responsabilità ciascuno. Quando si utilizza un Design Pattern, è molto più semplice trovare nomi significativi, poiché il modello di progettazione identifica spesso il tipo di responsabilità.

Per esempio:

- Se la classe crea oggetti che usano il suffisso Factory o Builder;
- Se la classe è responsabile per il coordinamento e la comunicazione tra altre classi di business, quindi utilizzare il suffisso Façade;
- Se la classe è usata per controllare l'uso di una classe di risorse, allora Proxy sarebbe un buon suffisso;
- Se una classe sta eseguendo il wrapping di un'altra, si usa il Adapter suffisso. 

__2. Evitare troppe istruzioni Se__

"Un'eccessiva logica condizionale fa girare la testa dello sviluppatore come Regan in The Exorcist". Gli sviluppatori che stanno abusando della logica condizionale stanno scavando le proprie tombe e di solito finiscono per essere sovraccarichi.

__3. Usare le interfacce: l'estensione è meglio che il sovraccarico di una classe__

Un'interfaccia è un'area del codice sorgente in cui è possibile modificare il comportamento senza modificare il codice. Le interfacce sfruttano la flessibilità e sono perfette per una semplice ragione: incoraggiano ad estendere l'architettura invece di cambiarla. 

__4. Evitare comportamenti globali e non deterministici__

L'uso di qualsiasi tipo di globale è altamente scoraggiato, a meno che esse non siano costanti e/o proprietà ben definite dell'oggetto.

__5. Evitare l'uso di oggetti statici__

__6. Separare la logica creazionale dalla business logic__

Uno dei principi fondamentali di una buona progettazione è che la logica della creazione di un oggetto e della logica di business (ciò che l'oggetto fa realmente) sono due preoccupazioni diverse che dovrebbero essere tenute il più lontano possibile. La creazione di oggetti è una preoccupazione che appartiene a classi specializzate, come Factory o costruttori. Dovrebbero avere esclusivamente il monopolio della creazione di oggetti. Gli oggetti, dall'altra parte, dovrebbero preoccuparsi solo di eseguire la logica di business e non preoccuparsi di creare altri oggetti (dipendenze).

Le Factory sono i pezzi di codice lunghi, semplici, facili da controllare e noiosi in cui tutte le modifiche sono semplici.
Gli oggetti business contengono tutti i trucchi magici e la complessità, quindi meno cambieremo qui, meno si romperà.
Mantenerli separati porterà ad una minore modifica della logica di business e ad una maggiore estensione della logica di business.

__7. Mantieni fuori casa i costruttori__

Come corollario del punto precedente, la logica aziendale non dovrebbe mai essere codificata in costruttori di un oggetto . Lo scopo dei costruttori è solo di assegnare alcune proprietà, inizializzare le variabili, eseguire semplici convalide dei parametri e infine collegare eventi. Se i costruttori stanno attivamente facendo qualcosa di business, allora, non saremo mai in grado di separare la logica creazionale dal business e l'architettura sarà sempre caotica.

__8. Legge di Demeter__
Questo principio è per lo più sconosciuto tra gli sviluppatori e, ovviamente, uno dei più utili. Senza entrare nelle definizioni formali, il principio afferma che una classe dovrebbe dipendere solo e strettamente da ciò che sta usando; quindi, solo ciò che è realmente usato dovrebbe essere iniettato in una classe.

Oggetti contenitori (solitamente denominati con i suffissi vaghi come Container, Context, Serviceo ServiceLocator, Portal, Environment, etc.) non devono essere passati come parametri di costruttori o metodi (a meno che non è necessaria l'intero contenuto).

__9. Mantenere basso il livello di complessità__
Il livello più interno di cicli annidati, switches e ifs è la misura di quanto sia complesso un metodo. Ad esempio, if all'interno di un foreach loop all'interno di un altro foreach ciclo viene valutata come una complessità di 3. La complessità di un metodo è la massima complessità del suo codice e non dovrebbe mai essere superiore a 4. Se è più di 4, allora è tempo di refactoring!

__10. Un metodo per un compito specifico__
Un metodo dovrebbe avere un unico scopo, che svolge in poche righe. Questo non significa definire un metodo per ogni operazione di calcolo, come ad esempio un metodo per il calcolo di un semplice valore, soprattutto se questo valore viene utilizzato staticamente in un altro metodo; piuttosto, si include il calcolo nel metodo chiamante. Come regola generale, ogni metodo dovrebbe adattarsi a uno schermo senza scorrimento verticale. Se non lo fa, è tempo di refactoring. I metodi lunghi sono spesso un segno di troppe responsabilità e programmazione procedurale. 
Il refactoring di metodi lunghi renderà lo sviluppatore molto riconoscente riguardo all'assenza di globalvariabili.
