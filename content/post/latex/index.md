---
authors:
- admin
categories:
- Tutorial
date: "2022-10-23T15:21:54+05:30"
draft: false
featured: false
lastmod: "2022-10-28T15:21:54+05:30"
projects: []
subtitle: "Primi passi e template funzionanti"
summary: "Quali sono le ragioni che rendono LaTex il linguaggio tipografico più utilizzato in ambito accademico? Una breve guida sui vantaggi che potrai sperimentare durante il suo utilizzo, i primi passi da compiere e alcuni template da riutilizzare nei tuoi progetti."
tags:
- LaTex
- Tutorial
- Template LaTex
- Impaginazione documento
- Tipografia professionale
title: Come iniziare a usare LaTex
reading_time: true 
share: false
image:
  caption: ""
  focal_point: ""
  preview_only: true
---


{{< toc >}}

## Un pò di storia

LaTex è un programma di composizione tipografica open source che utilizza il "motore" di tipocomposizione Tex sviluppato da [Dondald Ervin Knuth](https://en.wikipedia.org/wiki/Donald_Knuth) nel 1982. L'idea del suo *creatore* era quella di  porre rimedio alle crescenti difficoltà che, al tempo, sempre più studiosi riscontravano nello scrivere espressioni matematiche utilizzando esclusivamente la macchina da scrivere.

Di conseguenza LaTex **non** è Tex. Per riprendere le parole della [guida italiana più famosa sull'argomento](http://www.lorenzopantieri.net/LaTeX_files/ArteLaTeX.pdf): "*si potrebbe paragonare Tex a un corpo, e LaTex al più popolare degli “abiti” (fatto, però, di istruzioni in linguaggio Tex) che nel corso degli anni gli sono stati confezionati addosso per avvicinarlo al pubblico con sembianze amichevoli*".

## Composizione Asincrona e filosofia

La differenza principale che distingue LaTex da altri strumenti di composizione tipografica più famosi è che l'introduzione del testo e la sua composizione grafica avvengono in momenti differenti.
Si consideri, ad esempio, la stesura di un documento per mezzo del software Microsoft Word, in questo caso l'utente potrà agilmente apporre modifiche a quanto scritto, visualizzandole immediatamente sul proprio schermo (pensiamo ad esempio ad un cambio di font, l'aggiunta di un titolo o una modifica del colore).
Ecco, questo su LaTex *non* avviene. Infatti si distingue per essere un ambiente di *composizione assincrona*. Questa permette all'utente di introdurre il testo che desidera concentrandosi esclusivamente sul suo **contenuto**, senza ricadere in distrazioni. Successivamente questo potrà procedere a dare "in pasto" quanto scritto al compilatore dedicato, nel nostro caso LaTex.

L'idea di fondo è che, poichè la visualizzazione del testo passerà in secondo piano, l'utente potrà concentrarsi esclusivamente sul contenuto per poi visualizzare sempre il testo nel suo complesso in un secondo momento.

> LaTex **pretende** dall’utente considerazioni sul cosa: «il mio documento sa-
rà composto da un certo numero di capitoli, ciascuno diviso in paragrafi
numerati, avrà indice generale e analitico, delle figure e qualche tabella».

> Al **come** pensa LaTex, e lo fa molto bene. Per esempio, uno stesso file sorgente
può generare in teoria documenti radicalmente diversi soltanto cambiando-
ne la classe o caricando un pacchetto che agisce in modo globale su di esso.

## Installazione LaTex

Per iniziare a scrivere il primo documento di testo in LaTex il primo passo è **evitare** di confondere il motore (LaTex) con un editor che invece ci mostra semplicemente il codice a schermo.

{{% callout warning %}}
**Ricorda**: LaTex è il "linguaggio" che si differenzia da un semplice editor, il quale non sarebbe in grado, da solo, di riprodurre il risultato desiderato. Il suo scopo sarà solo quello di fornirci un interfaccia con cui scrivere (e compilare) agilmente il nostro codice.
{{% /callout %}}

Per questo il primo passo dovrebbe essere quello di procedere all'installazione della distribuzione più recente di LaTex partendo da [questo](https://www.tug.org/texlive/) link.

{{< figure src="textlive.png" caption="Distribuzioni Text Live per sistema operativo" numbered="true">}}


Come mostra la figura 1 la distribuzione può essere installata facilmente in qualsiasi sistema.

Va detto che, sebbene la distribuzione completa rimanga comunque l'opzione migliore, esiste la possibilità di iniziare ad utilizzare LaTex anche *completamente* online grazie al sito [https://www.overleaf.com/](Overlaf). Il fatto che non sia necessaria nessuna installazione potrebbe ulteriormente ridurre le resistenze anche per i più scettici.
In ogni caso il sito offre comunque un ottimi template che possono essere scaricati e riutilizzati offline anche da utilizzatori "tradizionali". Infine risulta, per certi versi, migliore quando il documento fa parte di un progetto in cui lavorano contemporaneamente diverse persone.
Nonostante esistano diversi Editor LaTex i due più utilizzati (offline) rimangono:

  1. [Text Studio](http://www.texstudio.org/) (Multi piattaforma)
  2. [Tex Shop](http://www.uoregon.edu/~koch/texshop/) (Mac)
  
## Basi e scrittura del primo documento

Dal momento in cui la compilazione del documento produrrà diverse tipologie di file è *buona norma* creare una cartella in cui si andrà a salvare il corpo del documento Tex e tutti gli elementi esterni che si potrebbe decidere di utilizzare (immagini, pdf, tabelle...).

```{=latex}
% Questo è un commento

\documentclass[a4paper]{article}
\usepackage[T1]{fontenc}
\usepackage[utf8]{inputenc}
\usepackage[italian]{babel}

\begin{document}
Ecco il mio primo documento con \LaTeX.
\end{document}

```
![Alt text here](imm.jpg "Sto ancora lavorando a questo articolo, ripassa tra qualche giorno per leggere la sua versione definitiva")
