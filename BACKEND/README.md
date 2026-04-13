# Guida all'installazione e avvio - BACKEND ANH-RESTAURANT

Questo documento contiene tutte le istruzioni necessarie per configurare e avviare l'API Laravel su una nuova macchina per lo sviluppo locale.

## 1. Prerequisiti di Sistema (Dipendenze)
Assicurati che sul nuovo PC siano installati i seguenti strumenti:
* **PHP**: Versione **8.2** o superiore
* **Composer**: Gestore pacchetti per PHP
* **Node.js e npm**: Per gestire le dipendenze frontend e compilare con Vite
* **Database Server**: Un server MySQL o MariaDB locale in esecuzione

## 2. Preparazione del Database
Crea un database vuoto sul tuo server locale. Puoi usare il nome che preferisci (assicurati solo di aggiornare le credenziali nel file `.env` nei passaggi successivi).

---

## 3. Installazione e Configurazione

Se preferisci configurare il file `.env` con calma prima di lanciare le migrazioni del database, esegui questi comandi nell'ordine indicato:

1. Scarica le dipendenze PHP:
```bash
composer install
```

2. Crea il file delle variabili d'ambiente e genera la chiave crittografica:
```bash
cp .env.example .env
php artisan key:generate
```

3. Apri il file `.env` con un editor e aggiorna le credenziali del tuo database locale:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=prova_api_2
DB_USERNAME=root
DB_PASSWORD=latuapassword
```

4. Esegui le migrazioni per creare le tabelle nel database:
```bash
php artisan migrate
```

5. Installa i pacchetti Node.js e compila gli asset:
```bash
npm install
npm run build
```

---

## 4. Avvio dell'Applicazione

Il progetto include uno script ottimizzato per avviare contemporaneamente il server PHP, l'ascolto delle code e il server Vite. 

Per avviare il progetto esegui:
```bash
php artisan serve
```

L'API sarà ora accessibile all'indirizzo `http://localhost:8000`. Si può testarne il funzionamento usando anche Postman (i JSON da importare su Postman sono nella cartella `POSTMAN REQUESTS`).

## 5. Configurazione Frontend

Il frontend è composto da file HTML che interagiscono con l'API tramite JavaScript.

### Collegamento API
1. Apri i file `login.html` e `dashboard.html`.
2. Assicurati che la variabile `apiBaseUrl` (o l'endpoint nelle chiamate `fetch`) corrisponda all'indirizzo della tua API Laravel.

### Gestione Autenticazione
* Il sistema di login invia le credenziali (`email`, `password`) all'endpoint `/login`.
* Una volta autenticato, il browser memorizzerà i cookie di sessione o i token necessari per le chiamate successive nella `dashboard.html`.

### Avvio del Frontend
Non essendo integrato direttamente in Blade, il frontend va servito separatamente:
* **Opzione 1 (Consigliata):** Avvia un CMD nella cartella del frontend e digita `php -S localhost:8080`.
* **Opzione 2:** Usa l'estensione "Live Server" su VS Code aprendo la cartella dei file HTML.

---

## 6. Troubleshooting
* **CORS Error:** Se il frontend e l'API sono su domini/porte diverse, assicurati che il file `config/cors.php` dell'API permetta le richieste dall'origine del tuo frontend.
* **Database:** Verifica nel file `.env` che `DB_DATABASE` corrisponda al nome creato sul tuo server MySQL.
* **Tenant non trovato:** Se ricevi un errore 404 all'accesso, verifica che il tenant sia stato creato correttamente nel database centrale e che l'API sia raggiungibile.