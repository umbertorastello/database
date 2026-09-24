# Cibora – Progettazione di una Base di Dati per Food Delivery

Progetto del corso di **Laboratorio di Basi di Dati** (2022/2023) — Università di Torino, Dipartimento di Informatica, gruppo:

- Simone Marengo
- Umberto Rastello

## Descrizione

Il progetto consiste nella progettazione completa (concettuale, logica e implementazione) della base di dati per **Cibora**, un servizio di food delivery che gestisce:

- utenti registrati, il loro borsellino elettronico e i mezzi di pagamento
- ristoranti aderenti, i piatti offerti e le relative categorie/liste
- ordini effettuati dagli utenti e la loro evasione
- rider che effettuano le consegne (in bicicletta, bici elettrica o monopattino)
- chat tra utenti, ristoranti e rider
- recensioni, reclami, codici sconto e programma "Top Partner" per i ristoranti

## Struttura del progetto

Il documento (`Marengo_Rastello_DB.pdf`) è organizzato in tre fasi principali:

### 1. Progettazione concettuale
- Analisi dei requisiti a partire dalla descrizione testuale del servizio
- Glossario dei termini
- Requisiti riorganizzati in frasi omogenee
- Schema Entità-Relazione principale, con business rules di integrità e derivazione

### 2. Progettazione logica
- Tavola dei volumi (stima delle dimensioni delle entità/associazioni, es. 34M utenti, 500M ordini)
- Tavola delle operazioni (frequenza di inserimento/interrogazione sui dati)
- Ristrutturazione dello schema E-R:
  - analisi delle ridondanze (es. `Rider.nConsegne`, valutata e infine eliminata a favore del calcolo derivato)
  - eliminazione delle generalizzazioni
  - eliminazione di attributi composti e multivalore (es. `Indirizzo`, `Ingredienti`, `Allergeni`)
  - scelta degli identificatori principali
- Schema E-R ristrutturato finale + business rules aggiornate
- Schema relazionale con vincoli di integrità referenziale

### 3. Implementazione
- **DDL**: script SQL di creazione di tutte le tabelle del database (utenti, ristoranti, piatti, ordini, rider, chat, ecc.) con chiavi primarie, chiavi esterne e vincoli `ON UPDATE/DELETE CASCADE`
- **DML**: popolamento di ogni tabella con dati di esempio
- Query ed operazioni di verifica dei vincoli (cancellazioni, modifiche su chiavi esterne, alterazioni di schema, query di join)

## Concetti chiave del modello

- **Doppio meccanismo di sconto**: sconto impostato dal ristorante sul piatto vs. codici sconto accumulati dall'utente in base allo storico ordini
- **Assegnazione rider**: al momento dell'ordine viene selezionato il rider libero più vicino, con vincolo speciale sui rider con bici elettrica per tragitti superiori a 10 km
- **Top Partner**: categoria assegnata dinamicamente ai ristoranti che superano soglie di qualità/servizio (≥20 ordini consegnati, valutazione ≥4.5, cancellazioni ≤1.5%, reclami ≤2.5%)
- **Chat**: sempre tra esattamente due entità (utente-ristorante, utente-rider o ristorante-rider)

## Analisi costi/benefici

Il documento include un confronto quantitativo (accessi in lettura/scrittura al giorno) tra due scenari di progettazione per l'attributo derivato `n consegne` del rider, con la scelta finale motivata dai costi di storage e I/O stimati.

## Come consultare il progetto

Il file principale è la relazione (`Marengo_Rastello_DB.pdf`), che contiene sia la parte di analisi/design che gli script SQL completi (DDL + DML) riportati in fondo al documento.

## Note

Progetto realizzato a scopo didattico nell'ambito del corso di Basi di Dati. I volumi e le frequenze operative sono stime a fini di dimensionamento, non dati reali.
