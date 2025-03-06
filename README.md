Questo script Bash è progettato per automatizzare la modifica dei timestamp di file (data e ora di modifica) all'interno di una cartella, basandosi sulla data presente nel nome del file o, preferibilmente, nei metadati EXIF delle immagini (se disponibili).  È particolarmente utile per organizzare cronologicamente foto e video provenienti da diverse fonti (fotocamera, WhatsApp, screenshot, ecc.).

## Funzionalità Principali:

*   **Estrazione Data:**
    *   **Priorità EXIF:** Per file immagine (`IMG`, `VID`), lo script tenta prima di estrarre la data di creazione dai metadati EXIF utilizzando `exiftool`. Se la data EXIF è disponibile e valida, viene utilizzata.
    *   **Nome File come Fallback:** Se i metadati EXIF non sono disponibili o non contengono una data valida, lo script analizza il nome del file per estrarre la data secondo diversi pattern predefiniti (vedi sezione "Formati Nome File Supportati").
*   **Modifica Timestamp:** Utilizza il comando `touch -t` per impostare il timestamp dei file alla data estratta.
*   **Gestione Ricorsiva:**  Lo script può essere eseguito su una cartella specifica, elaborando anche tutte le sottocartelle in modo ricorsivo.
*   **Modalità Simulazione (Dry-run):**  L'opzione `-n` o `--dry-run` permette di eseguire lo script in modalità simulazione, visualizzando le modifiche che verrebbero apportate senza effettivamente modificare i timestamp.
*   **Supporto Formati File:**
    *   **Immagini e Video:** Prefissi `IMG`, `VID`, `PANO`.
    *   **Screenshot:** Prefisso `Screenshot_`.
    *   **WhatsApp Immagini:** Riconoscimento del pattern `IMG-YYYYMMDD-WA*` (l'ora viene impostata a 00:00 per questi file a causa della mancanza di informazione oraria nel nome file tipico di WhatsApp).

## Formati Nome File Supportati (per estrazione data dal nome):

*   `IMG-YYYYMMDD-HHMM*` (es: `IMG-20250306-1315.JPG` - Fotocamera, Telegram, Instagram)
*   `IMG-YYYYMMDDWA*` (es: `IMG-20250306WA001.JPG` - WhatsApp immagini, ora impostata a 00:00)
*   `VID-YYYYMMDD-HHMM*` (es: `VID-20250306-1315.MP4`)
*   `PANO-YYYYMMDD-HHMM*` (es: `PANO-20250306-1315.JPG`)
*   `Screenshot_YYYYMMDD_HHMMSS*` (es: `Screenshot_20250306_131530.png`)

## Prerequisiti:

*   **Bash:**  Lo script è scritto in Bash e richiede un ambiente Unix-like (Linux, macOS, WSL).
*   **`exiftool` (opzionale, ma raccomandato):** Per l'estrazione della data dai metadati EXIF. Installabile tramite gestore di pacchetti (es: `sudo apt install libimage-exiftool-perl` o `brew install exiftool`). Se non installato, lo script funzionerà estraendo la data solo dal nome file.
*   **`date`:** Comando `date` standard di sistema.

## Come Utilizzare:

1.  **Salva lo script:** Salva il codice in un file, ad esempio `cambia_timestamp.sh`.
2.  **Rendi eseguibile:**  `chmod +x cambia_timestamp.sh`
3.  **Esegui nella cartella desiderata:**

    *   **Cartella Corrente:** `./cambia_timestamp.sh` (rispondere `YES` alla conferma).
    *   **Cartella Specifica (e sottocartelle):** `./cambia_timestamp.sh percorso/della/cartella`
    *   **Modalità Simulazione (Dry-run) nella cartella corrente:** `./cambia_timestamp.sh -n`  oppure `./cambia_timestamp.sh --dry-run`
    *   **Modalità Simulazione (Dry-run) su cartella specifica:** `./cambia_timestamp.sh -n percorso/della/cartella`
    *   **Aiuto:** `./cambia_timestamp.sh -h` oppure `./cambia_timestamp.sh --help`

## Importante:

*   **Conferma iniziale:**  La prima volta che si esegue lo script senza argomenti nella cartella corrente, è richiesta una conferma digitando `YES` per procedere con la modifica dei timestamp.
*   **WhatsApp Immagini:** Per le immagini WhatsApp con pattern `IMG-DATA-WA*`, l'ora del timestamp verrà impostata a `00:00` a causa della mancanza di informazione oraria nel nome file tipico di WhatsApp.
*   **Backup:** Si raccomanda di effettuare un backup dei file importanti prima di eseguire lo script, specialmente la prima volta, per sicurezza.
