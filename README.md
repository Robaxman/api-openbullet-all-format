# api-openbullet-all-format
Api OpenBullet Mod
Informazioni su questa guida
Questa guida spiega come ospitarel'APIOpenBullet e come collegarla al clientOpenBullet.

Introduttiva
Per prima cosa, devi scaricare l'API daqui. Quindi assicurati di aver installatodotnet core sdk 2.2. L'sdk dotnet core è disponibile per Windows, MacOS o Linux, quindi puoi ospitare questa API web su qualsiasi VPS o qualsiasi host che ti dia accesso root.

Configurazione
Se apri il file appsettings.json vedrai all'inizio 3 campi modificabili:

db- il file LiteDB in cui verranno archiviati gli utenti (se il file non esiste verrà creato automaticamente).
configFolder- la cartella sul filesystem in cui sono memorizzate le configurazioni da servire.
secretKey: la chiave segreta utilizzata per accedere all'endpoint /api/users e gestire gli utenti.
Ora crea una cartella Configs (è già inclusa nella versione precompilata) e al suo interno, per il bene di questo tutorial, crea una cartella chiamata e aggiungi qualsiasi OpenBulletconfig al suo interno.test.loli

Corsa
Aprire un terminale, accedere alla cartella in cui si trova OpenBulletAPI.dll e digitare. Questo eseguirà l'applicazione che di default sarà accessibile da remoto sulla porta HTTP 5000 (o sulla porta HTTPS 5001 se configurata).dotnet ./OpenBulletAPI.dll

Nota: Se per qualsiasi motivo non funziona durante la digitazione, ti consiglio di utilizzare questo comando per avviare il server web.http://localhost:5000dotnet ./OpenBulletAPI.dll --urls "http://*:5000"

Amministrazione
Scarica il pannello di amministrazione di OpenBullet daqui. Dopo averlo aperto, imposta l'URL dell'API (ad esempio) e il tuo tasto segreto, quindi premi aggiorna. Se non viene visualizzato alcun errore, è possibile procedere con l'aggiunta di un utente. Per questa esercitazione, aggiungere un utente generando una chiave api e scrivendo all'interno della casella di testo.http://localhost:5000testgroups

Configurazione delle fonti
Ora aggiorna OpenBullet all'ultima versione e vai alla scheda Impostazioni >OB > Sources. Aggiungere un'origine e digitare il percorso in cui si trovano le configurazioni estratto da (per impostazione predefinita è API / configs ad esempio). Successivamente, incolla la chiave API del tuo utente e salva le impostazioni.http://localhost:5000/api/configs

Infine, vai alla paginaGestione configurazionie fai clic suRipeti scansione. Se tutto è andato bene, qualsiasi configurazione sul tuo remotefolder verrà tirata giù al client. Queste configurazioni risiedono esclusivamente in memoria e la modifica/salvataggio è disabilitata.test

Autenticazione UserPass
Se lo desideri, puoi anche utilizzare Username e Password per autenticarti nell'API. Dovrai implementare una nuova chiamata a un'altra API esterna (ad esempio un sito Web, un forum) dove i gruppi vengono restituiti all'API OpenBullet che fornirà le configurazioni corrette all'utente.

API propria
Se desideri implementare la tua API scritta in un altro linguaggio (nodejs, python, go, php...) dovrai seguire alcune specifiche per farlo funzionare con il client OpenBullet originale.

La richiesta inviata da OpenBullet è una semplicerichiestaGET con l'intestazione impostata. Questo può essere:Authorization

Authorization: API_KEYse l'utente esegue l'autenticazione tramite chiave API
Authorization: Basic BASE_64dove BASE_64 codificato come base64USER:PASS
Larispostadell'API deve essere un fileZipcontenente nella sua radice tutte le configurazioni a cui si desidera che l'utente acceda..loli

Nelle ultime versioni di OpenBullet, se si inseriscono cartelle nel file Zip che si invia al client, verranno interpretate come nomi di categoria e visualizzate di conseguenza in Gestione configurazione.

La risposta può anche avere unheader. Nel caso in cui il valore dell'intestazione sia, OB richiederà all'utente il contenuto del corpo della risposta (codificato ASCII).ResultError
