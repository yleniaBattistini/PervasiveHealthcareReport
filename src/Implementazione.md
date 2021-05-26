# Implementazione
L'implementazione ha rispettato in maniera molto accurata le decisioni prese nella fase di design. Quest'ultima è stata curata notevolmente con lo scopo di avere una visione completa su ogni singolo aspetto che il sistema deve coprire.
L'unica questione che ci preme mettere in risalto, riguarda l'implementazione del pattern CQRS all'interno del sistema. Non essendo disponibile molta documentazione sull'implementazione di questo modello, abbiamo creato una nostra versione che rispetta tutti gli standard teorici definiti dal pattern.


### Comandi e Queries
Ogni comando e ogni query definita nel sistema è stata implementata come una class. Questo vengono passate al comand handler




### Command handler
Per facilitare l'esecuzione dei comandi viene definito un command handler, che, a seguito della loro esecuzione, restituisce il successo o il fallimento del comando.



### Eventi 
Ogni evento, indipendentemente dall'azione che rappresenta, è composto da:
- id univoco
- context, il contenuto dell'evento che varia in base all'evento.
- eventID, rappresentante la tipologia di evento
- timestamp della creazione dell'evento

Ogni evento è strutturato ed è formato da un ID, un timestamp, un codice dell'evento, per permettere il marshalling/unmarshalling corretto, ed i dati inerenti all'evento stesso.


### RM utility