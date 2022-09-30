# MAIZ-ERP-DATABASE

Schema database per ERP


## Generalità

Questo è un progetto per la definizione di uno schema di database (da ora maiz-db) da usare per la realizzazione di un ERP.
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
Un CODICE (alfanumerico) è invece un identificatore univoco, che può essere mutabile, destinato all'uso degli utenti: sarà possibile cambiare ad esempio il codice di un cliente.
Una ipotetica tabella clienti potrebbe essere definita come segue:

```
CREATE TABLE MAIZ_CLIENTI (
   Id_Cliente   dmz_ID_GLOB NOT NULL,
   Cod_cliente  dmz_ID_STR not null,
   ...
);
}
```


ID Numerici

| DOMAIN      | DATA TYPE | NOTE                                                         |
| ----------- | --------- | ------------------------------------------------------------ |
| dmz_ID_GLOB | bigint    | ID bigint univoco globale: non è possibile trovare lo stesso ID in tabelle diverse |
| dmz_ID_LONG | bigint    | ID bigint univoco a livello di singola tabella               |
| dmz_ID_INT  | integer   | ID integer univoco a livello di singola tabella              |



CODICI Alfanumerici

| DOMAIN        | DATA TYPE   | NOTE                          |
| ------------- | ----------- | ----------------------------- |
| dmz_COD_STR   | varchar(32) | CODICE alfanumerico, len = 35 |
| dmz_COD_STR05 | varchar(5)  | CODICE alfanumerico, len =  5 |
| dmz_COD_STR10 | varchar(10) | CODICE alfanumerico, len = 10 |



DOMAINS generici per campi dati

| DOMAIN                  | DATA TYPE     | NOTE                                |
| ----------------------- | ------------- | ----------------------------------- |
| dmz_DESCR_05            | varchar(05)   | campo "descrizione mini", len =  5  |
| dmz_DESCR_10            | varchar(10)   | campo "descrizione breve", len = 10 |
| dmz_DESCRIZIONE         | varchar(75)   | campo "descrizione", len = 75       |
| dmz_FLAG_S              | varchar(1)    | campo "flag" alfanumerico, Len = 1  |
| dmz_FLAG_I              | integer       | campo "flag" intero                 |
| dmz_NUMERO_COEFFICIENTE | decimal(10,6) | campo "coefficiente", decimale      |
| dmz_TEXT                | Text          | Testo libero                        |



### Multiazienda e multisede

maiz-db è multiazienda e multisede.

Questo significa che le tabelle non hanno il prefisso dell'azienda ma la discriminante dell'azienda (in realtà della sede dell'azienda) è un campo all'interno delle tabelle. 

Per definizione, ogni azienda ha almeno una sede.



MAIZ_AZIENDE

| Campo       | Tipo            | Chiave | Note                        |
| ----------- | --------------- | ------ | --------------------------- |
| Id_Azienda  | dmz_ID_STR05    | PK     | Identificativo dell'azienda |
| Descrizione | dmz_DESCRIZIONE |        | Descrizione dell'azienda    |



MAIZ_AZIENDE_SEDI

| Campo       | Tipo            | Chiave | Note                      |
| ----------- | --------------- | ------ | ------------------------- |
| Id_Sede     | dmz_ID_GLOB     | PK     | Identificativo della sede |
| Id_Azienda  | dmz_ID_STR05    | FK     | Rifderimento dell'azienda |
| Descrizione | dmz_DESCRIZIONE |        | Descrizione della sede    |



### Contatti

La tabella dei contatti è generica, un contatto può essere:

- una persona fisica
- un'azienda
- una struttura dell'azienda stessa

In questa tabella ci sono i dati generici del contatto, se poi ad esempio un contatto è anche cliente, allora avremo un record anche nella tabella dei clienti con lo stesso Id del contatto.

MAIZ_CONTATTI

| Campo       | Tipo            | Chiave | Note                        |
| ----------- | --------------- | ------ | --------------------------- |
| Id_Contatto | dmz_ID_GLOB     | PK     | Identificativo del contatto |
| Id_Azienda  | dmz_ID_STR05    | FK     | Rifderimento dell'azienda   |
| Descrizione | dmz_DESCRIZIONE |        | Descrizione della sede      |
| Indirizzo   | dmz_DESCRIZIONE |        | Indirizzo                   |
| CAP         | dmz_DESCR_10    |        | C.A.P.                      |
| Localita    | dmz_DESCRIZIONE |        | Paese o Comune di residenza |
| Provincia   | dmz_DESCR_05    |        | Provincia                   |
| Nazione     | dmz_DESCR_05    |        | Nazione                     |
| Note        | dmz_TEXT        |        | Note sul contatto           |
| DataIni     | date            |        | Data inizio validità        |
| DataFin     | date            |        | Data fine validità          |




