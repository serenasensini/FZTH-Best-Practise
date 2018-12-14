# FZTH-Best-Practise

## Introduzione
Un "buon codice" significa tempo e denaro risparmiato nell'apprendimento, nella manutenzione, nel test, nel fissaggio, nell'estensione e nella scalabilità del prodotto stesso. Ciò richiede più tempo e attenzione per lo sviluppo iniziale, ma restituisce rapidamente con gli interessi l'investimento con grandi interessi.

Le patch, le modifiche e le modifiche dell'ultimo minuto fanno parte del business IT, quindi le soluzioni 'rapide e sporche', quando necessario, dovrebbero sempre cercare di limitarsi in un luogo isolato dove sarà più facile refactoring in un secondo momento, e questo influenzerà il minimo possibile tutti gli altri componenti.

Le seguenti linee guida non sono esaustive e sono pensate per essere applicate in aggiunta ai principi SOLID e al corretto utilizzo dei modelli di progettazione OO.

1. Evita le classi onnipotenti, usa nomi significativi
Quando si progetta una nuova classe o si refactoring uno esistente, uno sviluppatore dovrebbe fare un elenco di tutte le attività eseguite da quella classe e trovare un nome che rappresenti facilmente e brevemente ciò che fa la classe.

Se la classe fa troppe cose, un nome rappresentativo è di solito qualcosa di mostruoso come: PageInfoBuilderAndConfigurationLoaderAndLinkAnalizerAndCacheManager. Ciò significa che la classe ha troppe responsabilità e dovrebbe essere suddivisa in più componenti, idealmente, con una responsabilità ciascuno.

Quando si utilizza OO Design Pattern, è molto più semplice trovare nomi significativi, poiché il modello di progettazione identifica spesso il tipo di responsabilità.

Per esempio:

Se la classe crea oggetti che usano il suffisso FactoryoBuilder
Se la classe è responsabile per il coordinamento e la comunicazione tra altre classi di business, quindi utilizzare il suffisso MediatoroFaçade
Se la classe è usata per controllare l'uso di una classe di risorse, allora Proxysarebbe un buon suffisso
Se una classe sta eseguendo il wrapping di un'altra in un'altra per regolare il suo utilizzo per una classe consumer, quindi usa il Adaptersuffisso
2. Evitare troppe istruzioni Se o Switch
Un'eccessiva logica condizionale fa girare la testa dello sviluppatore come Regan in The Exorcist. Le cose diventano più brutte quando la stessa logica condizionale viene applicata in molti punti dell'applicazione. Significa che i diversi comportamenti aziendali sono gestiti con ifs e switches invece di essere gestiti correttamente utilizzando l'ereditarietà o la composizione, separando i diversi comportamenti in diverse implementazioni di una abstract classe o interfaccia comune . Ad esempio, esiste una variabile di configurazione che indica se i dati devono essere archiviati in un database o in un file. Quindi ovunque nel codice, dovunque dobbiamo salvare qualcosa, usiamo la logica condizionale:

Nascondi   codice copia
If (ConfigParameters.SaveTo == StorageType.DataBase) 
           //Save something in the db
Else 
           //Save something to file
Immagina di dover aggiungere un comportamento diverso (ad esempio, salvare su una cache distribuita): ora dobbiamo estendere la logica condizionale ovunque nel codice. Immaginate quindi, più variabili di configurazione che determinano comportamenti multipli e che portano a una giungla di affermazioni condizionali non gestibili e intricate.

La strada da percorrere, nell'esempio sopra, dovrebbe essere:

Creare una abstract classe o un'interfaccia che rappresenta un Storage oggetto (ad esempio, IStorage), con metodi di salvataggio e caricamento.
Crea diverse implementazioni di astrazione, come DBStorage, FileStorage, CacheStorage, etc.
Creare una fabbrica che istanzia l'implementazione di storage corretta in base alla configurazione e restituirla come astrazione ( IStorage)
Quando hai bisogno di salvare dati, scrivi semplicemente: myStorage.SaveSomething(…)dove myStorageè una IStoragevariabile creata attraverso la fabbrica
Certo, è un sacco di lavoro ma ancora meno lavoro che mantenere le giungle condizionali. Ha anche un grande vantaggio che è spiegato nella seguente linea guida.

Gli sviluppatori che stanno abusando della logica condizionale stanno scavando le proprie tombe e di solito finiscono per essere sovraccarichi.

3. Usa giunzioni: l'estensione è meglio che cambiare
Una cucitura è un'area del codice sorgente in cui è possibile modificare il comportamento senza modificare il codice. Le cuciture sfruttano la flessibilità e sono perfette per una semplice ragione: incoraggiano ad estendere l'architettura invece di cambiarla.

Perché si sta estendendo meglio del cambiamento? Fai finta di essere un nuovo sviluppatore e ti viene chiesto di aggiungere nuove opzioni di archiviazione a un'applicazione.

Preferiresti passare attraverso il codice e cambiarlo con le nuove opzioni ovunque qualcosa venga salvato? Scoprire il codice che qualcun altro ha scritto, che non comprendi ancora completamente, rischiando di rompere qualcosa o di perdere uno dei cento posti che devono essere cambiati?

O preferiresti piuttosto creare una nuova classe che tu scriveresti da zero che implementerebbe l' IStorage interfaccia e si farebbe senza nemmeno guardare il codice di qualcun altro, senza rischi?

Non è una sorpresa se il grado di sicurezza di uno sviluppatore è più alto in quest'ultimo caso.

4. Evitare comportamenti globali e non deterministici
Gli sviluppatori di COBOL negli anni '80 sapevano già che le globalvariabili di abuso sono una cattiva e cattiva pratica. Globalle variabili sono raramente giustificate e i loro dannosi effetti collaterali possono portare alla follia mentale. Questa vecchia e saggia regola è ancora estremamente valida anche nei moderni linguaggi orientati agli oggetti, come C #.

Uno dei tanti problemi è che creano uno stato globale dell'applicazione che compromette il comportamento deterministico di funzioni / metodi; in altre parole, chiamare la stessa funzione due volte con gli stessi parametri può dare risultati completamente diversi. Quindi, il codice è fragile, difficile da eseguire il debug ed estremamente difficile da eseguire su più thread.

L'uso di qualsiasi tipo di globalvariabili / oggetti è quindi altamente scoraggiato.

Idealmente una buona architettura è senza stato. In pratica, però, quasi tutte le architetture necessitano di una sorta di stato (ad es. Un database, file, ecc.).

La soluzione è l' isolamento dello stato e consiste in due semplici regole:

Mantieni la vita e l'ambito di uno stato il più breve possibile (il che significa, ad esempio, che i membri della classe dovrebbero essere incapsulati).
Isolare lo stato avvolgendolo in un livello separato che si presenta attraverso le astrazioni (classi di base o interfacce).
Ad esempio, un database può presentare dati sotto forma di entità aziendali attraverso un'interfaccia ben definita di un livello di accesso ai dati. L'isolamento renderà la vita molto più semplice quando si tratta di testare o risolvere un problema.

5. Evita classi statiche per aiutanti, programmi di utilità e C.
Gli sviluppatori sono spesso tentati di creare static classi, soprattutto quando si tratta di aiutanti, utilità e così via. Il motivo per cui uno sviluppatore ricorre a una static classe non è molto diverso dal motivo per cui uno sviluppatore COBOL utilizza una variabile globale nel suo codice: dà accesso immediato ai dati da qualsiasi parte del codice, senza sforzo.

Sfortunatamente, le static classi agiscono frequentemente come uno stato globale e creano il non-determinismo che dovrebbe essere evitato. Ma peggiora. Poiché le static classi possono essere utilizzate ovunque nel codice senza essere passate esplicitamente come parametri, creano dipendenze segrete che non vengono rivelate dalla documentazione dell'API. Il comportamento del codice diventa quindi sempre meno dichiarativo e la chiarezza e la manutenibilità del codice diminuiscono drasticamente. Static le classi arrivano con variabili globali e dipendenze creando un accoppiamento stretto in tutti i consumatori (l'accoppiamento è transitivo).

Ultimo, ma non meno importante, il codice che usa le static classi non è testabile in isolamento, rendendo il test delle unità un incubo.

A meno che non strettamente necessario per motivi di prestazioni, l'uso di static classi dovrebbe essere evitato. Static le variabili sono ancora OK per oggetti costanti (anche se una static proprietà senza setter sarebbe meglio) o per mantenere private riferimenti a oggetti all'interno di classi di fabbrica.

6. Separare la logica creazionale dalla business logic
Uno dei principi fondamentali di una buona progettazione architettonica è che la logica della creazione di un oggetto e della logica aziendale (ciò che l'oggetto fa realmente) sono due preoccupazioni diverse che dovrebbero essere tenute il più lontano possibile.

La creazione di oggetti è una preoccupazione che appartiene a classi specializzate, come fabbriche o costruttori. Dovrebbero avere esclusivamente il monopolio della creazione di oggetti.

Gli oggetti, dall'altra parte, dovrebbero preoccuparsi solo di eseguire la logica di business (idealmente, una sola preoccupazione aziendale) e non preoccuparsi di creare altri oggetti (dipendenze).

Ad esempio, un oggetto chiamato HTMLAnalyzer (progettato per analizzare i collegamenti HTML) ha bisogno di un LinkAnalyzer per funzionare. Ciò significa che dipende dalla LinkAnalyzer classe e, pertanto, un'astrazione di LinkAnalyzer deve essere passata esplicitamente come parametro nel costruttore HTMLAnalyzer o come parametro dei metodi che lo utilizzano. Uno sviluppatore può invece pensare di creare l' LinkAnalyzer interno HTMLAnalyzer usando la new dichiarazione.

Doppio errore:

Prima di tutto, l' new istruzione crea una dipendenza per un tipo specifico (nessuna astrazione); pertanto, il comportamento è scolpito nella pietra e impossibile da modificare senza importanti refactoring.
Testare HTMLAnalyzer in isolamento ora è impossibile perché dovremo testare LinkAnalyzer allo stesso tempo.
Il modo corretto è utilizzare l'iniezione di dipendenza per iniettare esplicitamente e dichiarativamente tutte le dipendenze necessarie quando e dove sono necessarie, ad esempio, nel costruttore.

In questo caso, l'oggetto che utilizza un HTMLAnalyzer richiama una fabbrica per ottenere un'astrazione (interfaccia o classe base) di un LinkAnalyzer oggetto e iniettarlo nel HTMLAnalyzer costruttore.

In qualsiasi momento, il comportamento di LinkAnalyzer può essere modificato creando implementazioni alternative e l'unica modifica richiesta sarà in fabbrica, non nel codice effettivo della business logic in cui i cambiamenti sono costosi e pericolosi.

Una semplice implicazione di ciò è che il modello di progettazione di Singleton è intrinsecamente sbagliato e non dovrebbe mai essere usato. Mescola in sé la logica creazionale (crea se stessa) e la logica del business, per non dire che si tiene come static global oggetto che non arriverà mai al garbage collector (un singleton, come l'amore, è per sempre) e costituisce un cattivo stato globale di cui abbiamo già parlato.

Le fabbriche sono i pezzi di codice lunghi, semplici, facili da controllare e noiosi in cui tutte le modifiche sono semplici.

Gli oggetti business contengono tutti i trucchi magici e la complessità, quindi meno cambieremo qui, meno si romperà.

Mantenerli separati porterà ad una minore modifica della logica di business e ad una maggiore estensione della logica di business.

7. Mantieni fuori casa i costruttori
Come corollario del punto precedente, la logica aziendale non dovrebbe mai essere codificata in costruttori di un oggetto . Lo scopo dei costruttori è solo di assegnare alcune proprietà, inizializzare le variabili, eseguire semplici convalide dei parametri e infine collegare eventi. Se i costruttori stanno attivamente facendo qualcosa di business, allora, non saremo mai in grado di separare la logica creazionale dal business e l'architettura sarà sempre caotica.

8. Legge di Demeter
Questo principio è per lo più sconosciuto tra gli sviluppatori e, ovviamente, uno dei più utili. Senza entrare nelle definizioni formali, il principio afferma che una classe dovrebbe dipendere solo e strettamente da ciò che sta usando; quindi, solo ciò che è realmente usato dovrebbe essere iniettato in una classe.

Una grande spiegazione che ho trovato in un Google TechTalk ( http://www.youtube.com/watch?v=RlfLCWKxHJ0 ) è la seguente:

In un sistema eCommerce, ogni utente ha un Wallet oggetto contenente una raccolta di CreditCardoggetti e altre informazioni finanziarie. La classe responsabile del pagamento online ha un metodo come questo:

Nascondi   codice copia
bool Pay(Wallet wallet, string ccNumber, double amount) {
                CreditCard cCard = wallet.GetCreditCards().GetCard(ccNumber);
                return ProcessPayment(cCard, amount);
}
Come consideri un metodo di pagamento come questo? Quando paghi per la tua nuova maglietta da Macy's, daresti tutto il tuo portafoglio pieno di carte di credito e contanti alla cassiera e gli permetteresti di scegliere la carta di credito o le fatture giuste? Ovviamente no. Quindi, se una classe di pagamento ha bisogno solo di un CreditCardoggetto, CreditCarddeve essere data - niente di più, niente di meno. Non c'è bisogno di creare una dipendenza passando un oggetto contenitore enorme (il Wallet) pieno di oggetti inutili che creano rischi di accoppiamento e sicurezza.

Oggetti contenitori (solitamente denominati con i suffissi vaghi come Container, Context, Serviceo ServiceLocator, Portal, Environment, etc.) non devono essere passati come parametri di costruttori o metodi (a meno che non è necessaria l'intero contenuto).

9. Mantenere basso il livello di complessità
Il livello più interno di cicli annidati, switches e ifs è la misura di quanto sia complesso un metodo. Ad esempio, if all'interno di un foreach loop all'interno di un altro foreach ciclo viene valutata come una complessità di 3. La complessità di un metodo è la massima complessità del suo codice e non dovrebbe mai essere superiore a 4. Se è più di 4, allora è tempo di refactoring!

La complessità ciclomatica è facilmente misurabile, tuttavia non è l'unico tipo di complessità da tenere d'occhio. 
Se vuoi saperne di più sulla complessità del codice, discuto l'argomento più in dettaglio nell'articolo The Rule of Transparency .

10. Nessun metodo lungo
Come regola generale, ogni metodo dovrebbe adattarsi a uno schermo senza scorrimento verticale. Se non lo fa, è tempo di refactoring. I metodi lunghi sono spesso un segno di troppe responsabilità e programmazione procedurale. 
Il refactoring di metodi lunghi renderà lo sviluppatore molto riconoscente riguardo all'assenza di globalvariabili.
