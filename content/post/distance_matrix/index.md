---
authors:
- admin
categories:
- Tutorial
date: "2020-12-13T00:00:00Z"
draft: false
featured: false
lastmod: "2020-10-21T14:36:00Z"
projects: []
subtitle: "Tutorial su come creare una matrice delle distanze utilizzando gli shapefile Istat e definendo precisamente l'unità di misura ottenuta."
summary: "Tutorial su come creare una matrice delle distanze partendo dagli shapefile Istat e definendo precisamente l'unità di misura ottenuta."
tags:
- Qgis
- Tutorial
- Matrice delle distanze
title: Come creare una matrice delle distanze usando QGIS
reading_time: true 
share: false
image:
  caption: ""
  focal_point: ""
  preview_only: true
---

# Introduzione e obiettivi

L'utilizzo di dati geospaziali sta acquisendo un ruolo sempre più importante. [QGIS](https://www.qgis.org/en/site/) (Geographic Information System) è un software completamente Open Source disponibile su Windows, Linux e MacOs. Le caratteristiche che lo distinguono sono, appunto, la gratuità e la sua interfaccia userfrindly che permettono anche ad utenti principianti di svolgere operazioni che su altre piattaforme richiederebbero lo sviluppo di molte più competenze nell'utilizzo del software.

In questo breve tutorial il mio obiettivo sarà quello di illustrare il procedimento da seguire per costruire una matrice delle distanze partendo dagli shape file messi a disposizione dall'[Istat](https://www.istat.it/it/archivio/222527).

A tal proposito, oltre al procedimento legato alla creazione della matrice, cercherò di discutere anche i diversi [sistemi spaziali di riferimento (SRS)](https://en.wikipedia.org/wiki/Spatial_reference_system) utilizzati per convertire i risultati ottenuti da gradi (ovvero l'unità di misura delle coordinate) a metri (unità più comoda quando si parla di matrice delle distanze).

Il livello di dettaglio utilizzato sarà quello delle province italiane aggiornato al 1 gennaio 2022.

# Download shapefile e caricamento su QGIS

Per poter ottenere la nostra matrice il primo passo da compiere sarebbe quello di caricare su QGIS un csv contenente latitudine e longitudine di ciascuna provincia. Tuttavia questo procedimento non è semplice come sembra per almeno due ragioni:
- Spesso i file online non sono aggiornati e la suddivisione delle provincie potrebbe differire da quella attuale;
- Alcuni siti mettono a disposizione liste parziali o non del tutto gratuite;

Per questo motivo quello che andremo a fare sarà importare su QGIS gli shapefile messi a disposizione da Istat per poi andare a calcolare su quest'ultimi i [centroidi](https://www.gissiamo.it/wp/2019/12/18/cosa-sono-i-centroidi/) di ogni poligono rappresentante la singola provincia e poterne estrapolare le coordinate.

Per fare questo basterà aprire [questo link](https://www.istat.it/it/archivio/222527) e scaricare uno dei due file zip messi a disposizione dall'Istat, per i nostri scopi la versione generalizzata (meno dettagliata) andrà più che bene.


   ![Alt text here](foto1.png "Dawnload shapefile da sito Istat")
 
 A questo punto sarà possibile importarli su QGIS avviando un nuovo progetto e seguendo il processo "Layer>Aggiungi Layer>Aggiungi vettore Layer".
 
 Come riportato nella seconda figura, a questo punto dovrete prima selezionare il file zip comprendente tutti gli shapefile scaricati e, nella finestra che si aprirà, selezionare solo quello relativo alle province e premere "ok". Il procedimento sarà analogo nel caso il vostro livello di dettaglio sia diverso, tenente in considerazione che, nel caso dei comuni, il dispendio in termini di capacità di calcolo potrebbe essere maggiore a causa dell'elevata numerosità.
 
  ![Alt text here](foto2.png "Importazione shapefile province su QGIS")
  
  A questo punto dovreste avere una situazione simile a quella presente nell'immagine numero 3:
  
  ![Alt text here](foto3.png "EPSG:32632")
  
  Come potrete notare, l'immagine evidenzia una sigla di estrema importanza, che andrebbe considerata ogni volta in cui un nuovo layer viene importato su QGIS. Mi riferisco al c.d. [Sistema di Riferimento delle Coordinate Predefinito](https://docs.qgis.org/3.22/it/docs/gentle_gis_introduction/coordinate_reference_systems.html). Per avere maggiori informazioni sul sistema utilizzato vi basterà schiacciare sulla sigla e sarà QGIS ad illustravi le sue caratteristiche.
Per i nostri scopi sarà fondamentale sapere sempre in quale sistema stiamo lavorando perchè questo ci indicherà anche l'unità di misura delle coordinate sulla nostra mappa (metri o gradi). In particolare, quando andremo a calcolarci le coordinate sarà importante ragionare in gradi e poi, quando passeremo al calcolo della matrice sarà altrettanto importante convertire tutto in metri.
  
Per capire l'unità di misura utilizzata dall'istat basterà scrivere nel sito [epsg.io](https://epsg.io/) il sistema su cui desiderate avere maggiori informazioni. Facendolo potrete constatare che quello attualmente usato nel nostro progetto esprime le misure in metri. Per il momento sarà possibile procedere al calcolo dei centroidi senza conversioni. Tuttavia per gli step futuri tenere a mente questa informazione risulterà cruciale per evitare errori grossolani nell'interpretazione dei risultati.

# Calcolo dei centroidi 

Per calcolare il centroide di ogni provincia (poligono) basterà selezionare il tool **Centoridi** dalla sezione di strumenti di Processing **Geometria vettore**. Lasciate le opzioni di default e premete esegui.
Noterete subito che QGIS restituisce un errore,  riportato anche nella figura successiva.

  ![Intersezioni tra poligoni](foto4.png "Intersezioni tra poligoni")

Tale errore è dovuto al fatto che alcuni poligoni hanno una geometria non corretta, per poter procedere sarà necessario correggerli.

Per prima cosa questi errori andranno individuati nella mappa, per farlo basterà utilizzare lo strumento **Controllo validità**, eseguitelo sul layer importato in precedenza senza modificare le impostazioni di default e noterete che la mappa assumerà due colori differenti in cui appariranno anche dei piccoli cerchi indicanti i punti che hanno portato all'errore in questione.

  ![Identificare i poligoni](foto5.png "Identificare i poligoni")

Zoomando su uno dei punti potrete facilmente identificare e comprendere l'origine del problema.

  ![Identificare i poligoni](foto7.png "Identificare i poligoni")

A questo punto, selezionando il layer originario "ProvCM01012022_g_WGS84" selezionate lo strumento **Ripara geometrie** e applicatelo al layer di partenza come mostrato in figura:

![Ripara geometrie](foto6.png "Ripara geometrie")

Da questo momento in avanti il vostro layer di riferimento sarà quello dal nome "Geometrie riparate", togliete la spunta dai rimanenti e lavorate solo su questo. Attenzione però, come evidenzia la figura precedente, il layer è solo temporaneo, alla chiusura di QGIS questo sparirà e non verrà salvato in nessuna cartella.

A questo punto non vi resta che ripetere il procedimento relativo al calcolo dei centroidi sul livello riparato, la procedura non dovrebbe riportare errori e dovreste ottenere un centroide per ogni provincia.

# Calcolo delle coordinate associate ad ogni centroide

Gli shapefile messi a disposizione dall'Istat sono ricchi di informazioni ma, sfortunatamente, non possiedono indicazioni relative alle coordinate. Quello che faremo a questo punto sarà convertire il nostro layer in un EPSG in gradi per poi aggiungere al nostro file latitudine e longitudine, fondamentali per ottenere la matrice desiderata.

Partendo dal sito [epsg.io](https://epsg.io/) sarà necessario convertire il layer contenente i centoridi in EPSG:4326 che,sulla base della documentazione presente nel sito in questione, risulta essere un sistema cartografico espresso in gradi, quindi perfetto per il nostro scopo.

![EPSG:4326](foto8.png "EPSG:4326")

Per convertire il Layer sarà necessario selezionarlo ed esportarlo nel sistema scelto premendo tasto destro sul layer>esporta. Selezionate l'estensione GeoPackage e il sistema EPSG:4326 dalla sezione "SR", a questo punto selezionando un percorso di vostro gradimento dovrebbe essere possibile confermare l'operazione e completare l'esportazione.

Per poter aggiungere le coordinate al layer in questione basterà selezionarlo e andare su Vettore>Strumenti di Geometria e selezionare **Aggiungi attributi della Geometria**.
Per verificare se il processo è andato a buon fine potete aprire, con il tasto destro sul layer, la **Tabella Attributi del Layer**. Quello che dovreste vedere nelle ultime due colonne appare nell'immagine seguente:

![Aggiunta coordinate](foto9.png "Aggiunta coordinate")

Come potete notare QGIS ha aggiunto due colonne contenenti la latitudine e la longitudine associate ad ogni centroide pur mantenendo anche tutte le altre informazioni presenti nel file originario come codice province, denominazione completa etc...

Selezionando il layer appena creato esportatelo nel vostro pc in formato CSV, seguendo il processo svolto in precedenza. Questo file (in gradi) rappresenterà la base di partenza per la creazione della matrice delle distanze.

# Creazione della matrice

Per completare questo punto potreste anche decidere di avviare un nuovo progetto e considerare questa sezione come il punto di partenza nel caso in cui disponiate già di un file contenente le coordinate per ciascuna provincia.

Selezionando "Layer>Aggiungi Layer>Aggiungi Layer testo Delimitato" potrete procedere ad importare il file creato in precedenza. Ispezionando quest'ultimo dovreste rendervi conto che nella colonna "DEN_PROV" le provincie che corrispondono anche al capoluogo di regione rappresentano un missing (-) nella colonna relativa alla denominazione. Per rimediare a questo problema potete attivare le modifiche premendo tasto dentro sul layer>Attiva modifiche.
Ispezionando il Layer a questo punto vi sarà possibile sostituire i 21 missing copiando e incollando il nome della provincia presente nella colonna dei capoluoghi di regione.


![Sostituzione missing values](foto10.png "Sostituzione missing values")

I nostri dati sono completi, possiamo procedere alla creazione della matrice!

Per farlo vi basterà selezionare il layer importato in precedenza (l'unico se avete avviato un nuovo progetto) e selezionare lo strumento **Matrice delle distanze** spuntando le impostazioni riportate nell'immagine seguente.

![Matrice delle distanze](foto11.png "Matrice delle distanze")

L'output dovrebbe restituirvi un Layer contenente una matrice NxN.

A questo punto il processo potrebbe essere terminato....se non fosse che le nostre distanze sono in gradi! per poter interpretare tutto senza problemi sarà prima necessario passare da un sistema di misurazione in gradi a uno in metri.

Seguendo il processo di esportazione illustrato in precedenza vi basterà selezionare il sistema [EPSG:6875](https://epsg.io/6875) e salvare il tutto in formato CSV nel vostro pc per utilizzare la matrice in progetti futuri.

Attenzione: QGIS, di default, salverà tutto utilizzando il sistema decimale in cui il "." rappresenta il separatore dei decimali, sarà necessario tenerne conto nel caso in cui decideste di importare tutto su Excel. In alternativa potreste anche decidere di sostituite ogni "." con una "," aprendo il file csv nel Blocco Note e premendo CTRL+H per avviare una ricerca con sostitutzione.

Ricordate che le distanze trovare sono espresse in termini di distanza aerea, per cui dati due punti potrete anche verificarne la correttezza misurando i due punti selezionati (che corrisponderanno a due centroidi) con lo strumento **Misura Linea** in questo modo potrete avere la certezza di aver svolto tutto nel modo corretto. 

