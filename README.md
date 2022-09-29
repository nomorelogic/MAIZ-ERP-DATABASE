# MAIZ-ERP-DATABASE
Schema database per ERP


## Generalità
Questo è un progetto per la definizione di uno schema di database da usare per la realizzazione di un ERP.
L'intenzione è di fare in modo che lo stesso schema possa essere utilizzato su diversi RDBMS; per questo motivo si eviterà, nei limiti del possibile, di sfruttare le particolarità di un determinato DBMS ma si cercherà di adottare soluzioni tecniche utilizzabili sui diversi database server.
I DBMS a cui attualmente di pensa di adattare lo schema sono: Postgresql, Firebird, MsSql e MySql.
In fase di valutazione sono Oracle e SqLite.

E' importante comprendere che in questo progetto troveremo solamente lo schema del database.


## Analisi dello schema

### Tipologie di campi e domini
Molti DBMS hanno la possibilità di definire i domini (domains) per poi utilizzare questi nella definizione delle tabelle.
quì spieghiamo come verranno utilizzati i domini.
Nella tabella seguente, il prefisso "dmz" sta ad indicare: DoMain maiZ.

In questo database sono previsti sia identificatori numerici (ID) che alfanumerici (CODICI).
Un ID (numerico) è un identificatore univoco immutabile all'interno del database ed è destinato ad essere usato nelle PK e nelle FK.
Un CODICE (alfanumerico) è invece un identificatore univoco mutabile destinato all'uso degli utenti: sarà possibile cambiare ad esempio il codice di un articolo.
Una ipotetica tabella articoli può essere definita come segue:
```
CREATE TABLE MAIZ_ARTICOLI (
   Id_Articolo          dmz_ID_GLOB NOT NULL,
   Codice_Articolo      dmz_COD_STR not null,
   ...
);
}
```

ID Numerici
| DOMAIN                  | DATA TYPE     | NOTE                |
|-------------------------|---------------|---------------------|
| dmz_ID_GLOB             | bigint        | ID bigint univoco globale: non è possibile trovare lo stesso ID in tabelle diverse |
| dmz_ID_LONG             | bigint        | ID bigint univoco a livello di singola tabella                                     |
| dmz_ID_INT              | integer       | ID integer univoco a livello di singola tabella                                    |


CODICI Alfanumerici
| DOMAIN                  | DATA TYPE     | NOTE                          |
|-------------------------|---------------|-------------------------------|
| dmz_COD_STR             | varchar(32)   | CODICE alfanumerico, len = 35 |
| dmz_COD_STR05           | varchar(5)    | CODICE alfanumerico, len =  5 |
| dmz_COD_STR10           | varchar(10)   | CODICE alfanumerico, len = 10 |


DOMAINS generici per campi dati
| DOMAIN                  | DATA TYPE     | NOTE                                |
|-------------------------|---------------|-------------------------------------|
| dmz_DESCR_05            | varchar(05)   | campo "descrizione mini", len =  5  |
| dmz_DESCR_10            | varchar(10)   | campo "descrizione breve", len = 10 |
| dmz_DESCRIZIONE         | varchar(75)   | campo "descrizione", len = 75       |
| dmz_FLAG_S              | varchar(1)    | campo "flag" alfanumerico, Len = 1  |
| dmz_FLAG_I              | integer       | campo "flag" intero                 |
| dmz_NUMERO_COEFFICIENTE | decimal(10,6) | campo "coefficiente", decimale      |


