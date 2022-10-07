# SELECT_TEORIA

```sql
# seleziona colonne dalla tabella

SELECT colonna1, colonna2, ...
FROM nome_tabella;
```

e' la sintassi del `SELECT` in sql.
Serve a estrapolare dati dal db che rispettano una condizione precisata.
Ma dove si trovano i criteri? Infatti nell'esempio sopra non ci sono, eccoli qui in basso:

```sql
# seleziona colonne dalla tabella rispettanti condizione

SELECT colonna1, colonna2, ...
FROM nome_tabella
WHERE condizioni;
```

le `condizioni` sono specificate dalla *clausola* `WHERE`.
Li dentro mettiamo tutto quello che ci interessa per estrarre i dati che ci interessano.

- Esiste un modo per il `SELECT` d'indicare <u>tutte le colonne</u> della tabella usando `*`
> tutte le colonne della tabella riferita
	SELECT * FROM tabella;

- Esiste un modo per il `SELECT` d'indicare solo <u>distinti</u> elementi della tabella usando `DISTINCT`
> per precisare solo elementi distinti della colonna riferita
	SELECT DISTINCT colonna FROM tabella;

- Esistono *funzioni* per il `SELECT`:
	  - `MIN()` ritorna il <u>valore piu' piccolo</u> delle colonne selezionate;
	  - `MAX()` ritorna il <u>valore piu' grande</u> delle colonne selezionate;
	  - `AVG()` ritorna il <u>valore medio</u> di una colonna numerica;
	  - `COUNT()` ritorna il <u>numero di righe</u> che rispettano un criterio;
	  - `SUM()` ritorna la <u>somma dei valori</u> in una colonna numerica.
> funzioni argomento del SELECT

	   SELECT MIN / MAX / AVG / COUNT / SUM (nome_colonna)
	   FROM nome_tabella
	   WHERE condizione;

- Una colonna o tabella, puo' essere rinominata con la *parola chiave* `AS`.
> per rinominare colonna o tabella o risultato funzione
	SELECT nome_colonna AS variabile
	FROM tabella;

*Operatori* della clausola `WHERE` sono:

| operatore | descrizione                         |
| --------- | ----------------------------------- |
| =         | uguale                              |
| >         | maggiore di                         |
| <         | minore di                           |
| >=        | maggiore di o uguale                |
| <=        | minore di o uguale                  |
| !=        | non uguale                          |
| BETWEEN   | tra un certo range                  |
| LIKE      | cerca per un pattern                |
| IN        | specifica multipli valori possibili | 

I record possono essere filtrati specificando piu' condizioni:
- `AND` mostra un record *se tutte le condizioni* separate con esso vengono soddisfatte;
- `OR` mostra record *se una qualsiasi delle condizioni* separate dallo stesso viene soddisfatta;
- `NOT` mostra il record se il risultato della condizione e' "non vero".

		SELECT colonna1, colonna2, ...
		FROM tabella
		WHERE condizione1 AND / OR / NOT condizione2 
			AND / OR / NOT condizione3, ... ;

# SELECT_ESEMPI
## example1
Immaginiamo di avere un db con al suo interno una tabella chiamata `Clienti`. Al suo interno ci sono 7 colonne e una di queste si chiama `paese`. Ora, immaginiamo di:

`estrarre tutte le info dei clienti italiani`
```sql
SELECT * FROM Clienti
WHERE paese='Italia';
```

| ID  | nome                         | contatto         | indirizzo               | citta        | cap   | paese  |
| --- | ---------------------------- | ---------------- | ----------------------- | ------------- | ----- | ------ |
| 27  | franchi s.p.a.               | Paolo Accorti    | Via Monte 34            | Torino        | 10100 | Italia |
| 49  | magazzini alimentari riuniti | Giovanni Rovelli | Via Ludovico il Moro 22 | Bergamo       | 24100 | Italia |
| 66  | Reggiani Caseifici           | Maurizio Moroni  | Strada Provinciale 124  | Reggio Emilia | 42100 | Italia |

## example2

Immaginiamo ora di voler estrarre, sempre all'interno della stessa tabella `Clienti`, tutti i paesi che ne fanno parte.
Per farlo ci serve un modo per fare distinzione tra duplicati (non vogliamo imbrogliare contando 2/3/4 volte il paese `Italia`).

`estraiamo i paesi nella tabella`
```sql
SELECT DISTINCT paese
FROM Clienti;
```

| paese     | /   |
| --------- | --- |
| argentina | /   |
| belgio    | /   |
| brazile   | /   |
| canada    | /   |
| danimarca | /   |
| finlandia | /   |
| italia    | /   |
| ...       | /   |

## example3

Abbiamo una lista di `paesi`, vorremmo ora contarla.
Gia' che ci siamo diamo un nome a quello che otteniamo.
Facciamo uso della funzione `COUNT()` e di `AS`.

`contiamo i paesi nella tabella e assegnamo variabile`
```sql
SELECT COUNT(DISTINCT paese) AS numeroPaesi
FROM Clienti;
```

| numeroPaesi | /   |
| ----------- | --- |
| 21          | /   | 

## example4
`estraiamo le info dei clienti in Germania ma solo dei paesi Munchen e Berlin`
```sql
SELECT * FROM Clienti
WHERE paese = 'Germania' AND 
	(citta = 'Berlin' OR citta = 'Munchen');
```

| ID  | nome                | contatto      | indirizzo         | citta   | cap   | paese    |
| --- | ------------------- | ------------- | ----------------- | ------- | ----- | -------- |
| 1   | alfreds futterkiste | Maria Anders  | Obere Str. 57     | Berlin  | 12209 | Germania |
| 25  | Frankenversand      | Peter Franken | Berliner Platz 43 | Munchen | 80805 | Germania | 

