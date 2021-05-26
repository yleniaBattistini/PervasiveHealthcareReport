# Processo di sviluppo adottato
Il processo di sviluppo adottato segue la metodologia DevOps.
Il nostro team è formato da 3 componenti:
- *Battistini*, addetto alle operazioni IT e sviluppatore.
- *Gnagnarella*, addetto alle operazioni IT e sviluppatore.
- *Scucchia*, sviluppatore.

Il processo di sviluppo prevede il supporto nell'intero ciclo di vita del progetto dei vari strumenti che la filosofia DevOps mette a disposizione, quali build automation e continuous integration.



# Tool di DevOps
In questa sezione sarà introdotto il processo di BA e CI, che ha accompagnato e automatizzato molteplici operazioni durante l'intero sviluppo del software.

## Build Automation
La build automation ha permesso di automatizzare un'ampia varietà di compiti nelle nostre attività quotidiane di sviluppo come:
- compilazione del codice sorgente in codice binario
- pacchettizzazione del codice binario
- esecuzione di test
- creazione di documentazione

All'interno del progetto abbiamo implementato le funzioni di build automation nel file *build.gradle.kts*.
Lo sviluppo del nostro elaborato è partito dal template  *buildautomationsetup* disponibile [qui](https://github.com/enrignagna/buildautomationsetup).

Questo repository è stato realizzato da noi precedentemente all'implementazione del sistema e contiene un template generico sfruttabile per lo sviluppo di nuovi sistemi. Al suo interno è definita la struttura corretta per progetti Gradle e sono già presenti alcuni strumenti di build automation, utili per lo sviluppo corretto di qualsiasi progetto.

### Plugin
Il nostro progetto è sviluppato principalmente in linguaggio Scala, quindi sono stati importati i plugin specifici per tale linguaggio. Abbiamo utilizzato, in aggiunta, il plugin \textit{Jacoco}, per il controllo della coverage all'interno del progetto ed il plugin di Semantic Versioning, realizzato dal professor Danilo Pianini, per riuscire a controllare che stessimo versionando correttamente.
Per garantire il corretto funzionamento di questi ultimi sono state definiti dei settings appositi sempre nello stesso file *build.gradle.kts*.

### Dipendenze
Il sistema Pervasive Healthcare essendo un ambiente di dimensioni moderate ha visto l'utilizzo di un gran numero di librerie esterne, le quali sono state importate in maniera opportuna, in base al loro utilizzo nell'implementazione od esclusivamente nei test. In aggiunta nei repository di riferimento sono state inserite le piattaforme in cui andare a reperirle per includerle nel progetto.

### Refresh automatico delle dipendenze
All'interno del file *settings.gradle.kts*, è stato abilitato il refresh automatico delle versioni delle dipendenze. Questo sistema permette appunto di andare ad aggiornare tutte le dipendenze e librerie presenti nel progetto, in modo da disporre sempre della versione aggiornata.
Questa funzionalità a fianco di un sistema di build e testing automatizzato permette di verificare costantemente se le nuove versioni delle dipendenze compromettono il nostro software.



## Continuous Integration
La CI, a seguito dell'implementazione della BA, è punto centrale nella filosofia DevOps. All'interno del progetto è stata sviluppata sfruttando la piattaforma Github Actions, che permette di definire delle pipeline per l'esecuzione di build, test e deployment in ambienti differenti.
Nel nostro sistema sono stati definiti 3 workflow, rispettivamente per la build del progetto, per la build delle pull request ed infine per la pubblicazione della documentazione sul dominio offerto da Github. Di seguito verranno spiegati più nel dettaglio i vari workflow.

### Build workflow
Questo workflow reagisce all'evento di push rispettivamente sui branch **main** e **develop**; in aggiunta a ciò, è stato impostata l'esecuzione periodica ogni lunedì alle ore 02:00.
L'esecuzione di questo workflow viene svolta sulla combinazione di sistemi:
- Sistema operativo:
  -  Windows
  - MacOs
  - Ubuntu
- Versione JVM:
    - 8
    - 11
    - 14
    - 15
    
Gli step eseguiti prevedono una serie di operazioni di preparazione dell'ambiente di esecuzione:
- il controllo del repository
- l'installazione del JDK

Di seguito vengono svolti i seguenti processi: build del progetto ed il check, ovvero la creazione del coverage report tramite Jacoco, il quale viene poi inviato alla piattaforma Codecov. Quest'ultima operazione viene fatta tramite l'apposita action *codecov-action@v1*.
Se al termine, tutte queste operazioni hanno avuto successo, viene creato un artefatto il quale viene a sua volta caricato su Github Actions.

### Build pull request workflow
Questo workflow reagisce all'evento di una pull request.
L'esecuzione di questo workflow viene svolta sulla stessa combinazione di sistemi previsti per il build workflow.
Per quanto riguarda gli step eseguiti sono previste una serie di operazioni iniziale per la preparazione dell'ambiente di esecuzione le quali rispecchiano quelle eseguite nel workflow precedente.
Lo step successivo prevede lo svolgimento della build del progetto ed il check, ovvero la creazione del coverage report tramite Jacoco, il quale viene poi inviato alla piattaforma Codecov tramite l'apposita action *codecov-action@v1*.


### Website workflow
Questo workflow reagisce all'evento di una release.
A seguito dell'avvio del workflow viene eseguito un controllo del repository, viene generata la documentazione del progetto la quale ,infine, viene caricata sul branch *gh-pages*, il quale è visibile tramite il dominio offerto da Github Pages.


## Problematiche riscontrate
Durante lo sviluppo delle pipeline di CI avevamo integrato nella pipeline di build e delle build request, la creazione di un container MongoDB, in modo da disporre anche del DB per testare le funzionalità del read e write model.
La creazione di un container risulta essere fattibile solo sulle macchine Ubuntu, ma limitare l'esecuzione della build dalla strategy matrix definita ora a un solo sistema, non ci aggradava come scelta, quindi abbiamo optato per non implementare la creazione di un container durante i suddetti workflow.

