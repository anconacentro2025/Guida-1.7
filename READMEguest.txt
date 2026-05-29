Guida Affittacamere Ancona Centro – Istruzioni  guida ver. 2.5

Questa web app è una guida interattiva per gli ospiti dell'affittacamere di Piazza Roma 3, Ancona.
È contenuta in un unico file HTML (index.html) e può essere aperta direttamente nel browser o caricata su un server web.

1. Utilizzo base

1. Apri index.html con un browser moderno (Chrome, Firefox, Safari, Edge).
2. Inserisci la password predefinita: anconacentro2025.
3. Puoi spuntare "Ricordami" per non reinserirla in futuro.
4. Una volta entrato, vedrai il banner e i pulsanti delle sezioni.
5. Tocca una sezione per visualizzare la mappa con i luoghi o le informazioni (Wi‑Fi, contatti, ecc.).
6. Clicca su un marker numerato o sul nome nella lista laterale per aprire il dettaglio del luogo.
7. Nel dettaglio puoi:
   · Leggere la descrizione e la distanza.
   · Aprire Google Maps.
   · Navigare tra i luoghi della stessa sezione con i pulsanti Prec./Succ.
8. Il cambio lingua (IT / EN / PL) si trova in alto a destra. Le descrizioni vengono tradotte automaticamente via internet (MyMemory), con cache locale.
9. La sezione Wi‑Fi mostra la password: toccala per copiarla negli appunti.

2. Modificare la password di accesso

1. Apri index.html con un editor di testo (es. Blocco note, VS Code).
2. Cerca la riga:
   const ACCESS_PASSWORD = "anconacentro2025";
3. Sostituisci "anconacentro2025" con la nuova password desiderata, mantenendo le virgolette.
4. Salva il file. La prossima volta che l'utente accede dovrà usare la nuova password.

Nota: La password è salvata in chiaro nel codice. Non utilizzare password sensibili se il file è accessibile pubblicamente su internet. Per una protezione più forte, si consiglia di spostare la verifica su un server.

3. Modificare le etichette delle sezioni e i pulsanti home

Le sezioni sono elencate nell'array sections (cerca const sections = [ nel codice). Ogni oggetto ha:

· id: identificativo interno (non toccare, serve al codice).
· icon: emoji visibile nel menu.
· label_it, label_en, label_pl: nome della sezione in italiano, inglese e polacco.

Esempio per cambiare il nome della sezione "Ristoranti" in italiano:

{ id:'restaurants', icon:'🍽️', label_it:'Ristoranti', label_en:'Restaurants', label_pl:'Restauracje' }

Modifica label_it a tuo piacimento.

I pulsanti della schermata iniziale e il pulsante "Home" usano questi stessi testi, quindi si aggiorneranno automaticamente.

4. Modificare didascalie e descrizioni dei luoghi

Tutti i dati dei luoghi (tour, ristoranti, bar, parcheggi, ecc.) sono nel grande oggetto appData. Cerca la sezione corrispondente (es. mustsee, restaurants, barpub, supermarkets, parking, around, marche).

Ogni luogo è un oggetto con questi campi principali:

· name: Nome del luogo (non viene tradotto, rimane uguale in tutte le lingue)
· emoji: Icona mostrata nella mappa e nel dettaglio
· photo: Nome del file immagine (es. 'rist-pozzo.jpg'). Lascia '' se non c'è foto.
· desc: Descrizione in italiano (verrà tradotta automaticamente per EN e PL)
· dist: Testo della distanza (es. '🚶 300 m – 4 min')
· mapQuery: Query per Google Maps (es. 'Il Pozzo Ancona')
· lat, lng: Coordinate geografiche per il marker sulla mappa
· order: (solo per mustsee) Ordine nel tour a piedi
· extraMap: (opzionale) Pulsante aggiuntivo per Google Maps, oggetto con label e query

Modifica di una descrizione esistente:
Esempio: vuoi cambiare la descrizione de "Il Pozzo" in italiano.
Cerca:

{ name:'Il Pozzo', emoji:'🍲', photo:'rist-pozzo.jpg', desc:'Trattoria tradizionale centro storico.', ... }

Modifica la stringa desc.

Aggiunta di un nuovo locale:
Aggiungi un nuovo oggetto nella lista restaurants, rispettando la sintassi. Assicurati di includere lat e lng per il marker (puoi ottenerle da Google Maps cliccando con il tasto destro su un punto e selezionando "Che cosa c'è qui?"). Il nuovo elemento apparirà automaticamente nella mappa e nella lista.

Modifica delle regole della casa:
Le regole sono in appData.rules, ogni regola ha icon, it, en, pl. Puoi cambiare i testi o aggiungerne di nuove.

5. Inserire o modificare foto

Le foto dei luoghi sono caricate da un repository GitHub:
https://raw.githubusercontent.com/anconacentro2025/Guida-1.7/main/img/

Il percorso base è definito nella variabile PHOTO_BASE.
Attualmente punta al ramo main della repo anconacentro2025/Guida-1.7.

Per cambiare il repository o la cartella delle immagini:

1. Carica le tue foto in una cartella pubblica (es. GitHub, Dropbox, tuo server).
2. Modifica PHOTO_BASE nel codice:
   const PHOTO_BASE = 'https://tuo-dominio.com/immagini/';
3. Per ogni luogo, imposta il campo photo con il nome esatto del file.

Attenzione:

· Le foto del dettaglio vengono caricate solo quando l'utente apre quel luogo.
· Se una foto non esiste, viene mostrata l'emoji del luogo come placeholder.
· Se il campo photo è vuoto (''), non viene mostrata nessuna immagine nel dettaglio (solo emoji).

6. Modificare i link dei trasporti

Nell'oggetto appData.transport trovi i testi per bus, treno, taxi, traghetti, aeroporto, ognuno nelle tre lingue.
Puoi cambiare gli URL dei collegamenti ipertestuali (es. href='https://www.conerobus.it/orari') con quelli che preferisci.
I testi sono scritti con tag HTML, quindi fai attenzione a mantenere gli apici corretti (usa ' all'interno di stringhe delimitate da " o viceversa).

7. Traduzioni automatiche e cache

· Le descrizioni (desc) e alcune etichette fisse vengono tradotte al volo richiamando l'API gratuita di MyMemory.
· La cache delle traduzioni viene salvata in localStorage: se un utente ha già visitato la guida in una lingua, le traduzioni verranno caricate dalla cache senza nuove chiamate di rete.
· Per svuotare la cache (ad esempio se modifichi una descrizione e vuoi forzare la ri-traduzione), l'utente può cancellare i dati del sito dal browser, oppure puoi cambiare la chiave di cache nel codice (cerca guideTranslations).

8. Mappe e Leaflet

La mappa utilizza la libreria Leaflet (caricata da CDN) e le tiles di OpenStreetMap.
Se modifichi le coordinate (lat, lng) di un luogo, il marker si sposterà automaticamente.
Per disabilitare la mappa (es. se non vuoi mostrarla per una sezione), modifica la funzione renderSectionWithMap o aggiungi condizioni.

9. Pubblicazione online

Per rendere la guida accessibile ai tuoi ospiti puoi:

· Caricare il file index.html su un hosting statico (GitHub Pages, Netlify, Vercel, Altervista, ecc.).
· Inviare il file via email o WhatsApp: l'ospite può salvarlo sul telefono e aprirlo con il browser (funziona offline, tranne le mappe e le traduzioni).

10. Contatti

Per assistenza o personalizzazioni:
📧 anconacentro@yahoo.com
📞 +39 335 675 0269

Ultimo aggiornamento: 29 maggio 2026 
