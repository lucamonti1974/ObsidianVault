# LGV system
|Code|Description|Type|Rows|Positions|
|---|---|---|---|---|
|CB10S|CB10 simplex|Forks|1|1|
|CB10T|CB10 Triplex|Forks|1|1|
|REACH|Reach doppia profondità|Forks|1|2|
|CONV|Conveyors|Conveyor|2|1|

# Material Type
- **PG** Prodotto grezzo
- **PS** Prodotto sabbiato
- **PS** Prodotto sabbiato termoregolato
- **PL** Prodotto lavorato
- **PV** Prodotto vendibile

# Layout

![[Pasted image 20240403022914.png]]


# Item Types
||Pallet ID|Descrizione|CB10S|CB10T|Reach|Conv|
|-|-|-|-|-|-|-|
|Semilavorato|1|FOMA|Lato corto||Lato lungo|Lungo e Corto|
|Prodotto finito AUDI|2|AUDI|Lungo e Corto|Lungo e Corto|Lungo e Corto||
|Prodotto Finito PUNCH|3|PUNCH|Lungo e Corto|Lungo e Corto|Lungo e Corto|Lungo|
|EPS|1,2,3||Lato corto|Lato corto|Lato corto|Lungo e Corto|

# Item - SKU
Corrispondenza 1-1 un item è pallettizzato in un unico modo
Informazioni non standard dell' item master:
- Ore stagionatura
La stagionatura (di una unità di carico che NON è cassa di prova) si considera conclusa quando Ore stagionatura < Data e ora attuali – Data e ora inizio stagionatura
- Ore stagionatura per Cassa di prova
La stagionatura di una cassa di prova si considera conclusa quando Ore stagionatura per Cassa di prova < Data e ora attuali – Data e ora inizio stagionatura
- Soglia Allarme Termoregolazione: è il numero di unità di carico stoccate a magazzino termoregolazione raggiunto il quale, viene scritto un evento di allarme nel log nella GUI E80
- SKUGroup (GT, VW, VS, AU, PU): differenziazione della quarantena

# Stock unit Data
- Stato del ciclo di lavoro: è identificato dai primi caratteri del lotto (o dei lotti) che l’articolo contiene. Non è possibile che una Unità di Carico contenga lotti con stato del ciclo di lavoro diversi (ad esempio non è possibile che contenga alcuni “PG” e alcuni “PS”). Indica se i pezzi contenuti in una unità di carico sono:
	- Grezzi “PG”, da sabbiare
	- Sabbiati “PS”, da lavorare su CDL
	- Lavorati su CDL “PL”, da lavorare su Linea Finale
	- Vendibili “PV”, lavorati su Linea Finale e potenzialmente spedibili al cliente
- Data/ora primo ingresso in termoregolazione da sabbiatura
- Data/ora prelievo dalla Linea Finale (Audi o Punch)
- Quantità: numero di pezzi di una Unità di Carico
- Stato di Qualità: stato di Qualità dell’Unità di Carico
- ==Cassa di prova==
- PackageId: codice dell’etichetta stampata e applicata sull’unità di carico da spedire
- Stato di Qualità
- LottoPV: codice lotto dell’intera unità di carico di prodotto vendibile (Audi o Punch) da stampare sulle etichette
 - ==RFID: identificativo univoco della base pallet==: Controllo di coerenza con l’unità di carico attesa, e codice da trasmettere al gestionale cliente

==Subinventory pulita dopo 14 giorni==
Non gestire le stockunitpart su E80

Etichette stampabili in automatico e anche a  mano

# Storage Locations

|Descrizione|Nome|Tipo di prodotto||LGV|
|-|-|-|-|-|
|Magazzino Termoregolazione|MT\_xx\_yy\_ww_zz|Prodotti finiti (vari stati ciclo di lavorazione), non vuoti|xx Corridoio , yy Fila, ww Livello, zz profondità|==REACH?==
|Magazzino Spedizioni|MS_xx_yy_ww|Prodotto finito vendibile Audi e Punch|xx Corridoio , yy Fila, ww Livello|==REACH?==
|Uscita sabbiatura|SAB_OUT_xx_D||xx linea sabbiatura|CB|
|Uscita sabbiatura|SAB_OUT_xx_P||xx linea sabbiatura|CONV|
|Interscambio termoregolazione|TER_IN_xx_P||xx linea di termoregolazione
|Interscambio punch magazzino spedizione|MS\_SCAMBIO_
|Ingresso sabbiatura|SAB_IN_P_xx|
|Carwash reach||||REACH|
|Carwash Conveyor||||CONV|
|Reintroduzione da terra|TER_RIENTRO_x|
|Qualità|TER_QUALITA_x|
|CW spedizioni|MS_CW|
|Reintroduzione spedizioni|MS_RIENTRO_x|
|Carwash sabbiatura|SAB_CW|
|Staging lane|STAGx_yy_zz||3 staging lanes, due da 26 e una da 28 su due livelli
|Scambi linea di produzione|
|Ingresso Vuoti Audi e Punch |||Gravity|
|Uscita vuoti FOMA|||Conveyor|
|Bricchettatrici|||Riceve cassone vuoto e restituisce pieno|
|Scambio vuoti pieni cassoni sfridi|
|Etichettatura di spedizione|||Deposito da un lato, prelievo da entrambi i lati, serve x ruotare i pallet per la staging lane




