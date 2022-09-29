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

CREATE DOMAIN dmz_ID_GLOB bigint;               -- id globale (long)
CREATE DOMAIN dmz_ID_LONG bigint;               -- id long
CREATE DOMAIN dmz_ID_INT  integer;              -- id integer

CREATE DOMAIN dmz_ID_STR varchar(32);           -- id string(32)
CREATE DOMAIN dmz_ID_STR05 varchar(5);          -- id string(5)
CREATE DOMAIN dmz_ID_STR10 varchar(10);         -- id string(10)

CREATE DOMAIN dmz_DESCR_05 varchar(05);
CREATE DOMAIN dmz_DESCR_10 varchar(10);
CREATE DOMAIN dmz_DESCRIZIONE varchar(75);
CREATE DOMAIN dmz_FLAG_S varchar(1);
CREATE DOMAIN dmz_FLAG_I integer;
CREATE DOMAIN dmz_NUMERO_COEFFICIENTE decimal(10,6);

CREATE DOMAIN dmz_TEXT AS BLOB SUB_TYPE TEXT;

