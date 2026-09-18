# Come contribuire al Ready To Deploy

Il **Ready To Deploy** è un corso di Python per principianti, da zero al primo deploy. Ogni
modulo del corso nasce come **notebook Jupyter** (`.ipynb`) e solo dopo viene pubblicato come
pagine del sito.

Puoi contribuire con **contenuti**: un modulo nuovo, una riscrittura di un modulo esistente o
un progetto pratico. Non serve saper toccare il sito — il sito è la fase finale, ed è curata
dall'autore del corso dopo l'approvazione.

Il percorso è sempre lo stesso:

```
clonare il repository
      ↓
inserire il tuo materiale "grezzo" in  contributing/raw_content/
      ↓
chiedere a Claude di generare      →  contributing/generated_ipynb/modules/module_NN.ipynb
(con le skill di questo repo)      →  contributing/generated_ipynb/projects/project_NN.ipynb
      ↓
rivedere il notebook tu stesso (eseguirlo dall'inizio alla fine)
      ↓
commit + pull request  →  l'autore rivede, approva e pubblica sul sito
```

---

## 1. Cosa ti serve

| Requisito | A cosa serve |
| --- | --- |
| Git e un account su GitHub | clonare il repository e aprire la *pull request* |
| [Claude Code](https://claude.com/claude-code) | eseguire le *skill* di generazione già presenti nel repository (`.claude/skills/`) |
| Python 3.10 o più recente | i validatori eseguono tutte le celle del notebook |
| Jupyter, VS Code o Google Colab | aprire e rivedere il notebook generato |

Non serve installare le dipendenze del sito (`node_modules/`) per contribuire con contenuti.

## 2. Clona e crea un branch

Se non hai i permessi di scrittura sul repository, fai prima un *fork* e clona il tuo *fork*:

```bash
git clone https://github.com/<tuo-utente>/ready-to-deploy.git
cd ready-to-deploy
git checkout -b contenuto/modulo-10-test
```

Usa un nome di branch che descriva cosa stai portando: `contenuto/modulo-10-test`,
`contenuto/progetto-modulo-04`, `contenuto/riscrive-modulo-02`.

## 3. Inserisci il tuo materiale grezzo in `contributing/raw_content/`

Il **materiale grezzo** è tutto ciò che già hai sull'argomento, senza alcuna formattazione
particolare: appunti di lezione, un vecchio notebook, un `.md`, un `.txt`, esempi in `.py`, un
PDF, un elenco di argomenti, un CSV di esempio. Non deve essere ordinato né elegante — è
proprio questo materiale che la *skill* riscriverà secondo lo standard del corso.

Due modi di organizzarlo, entrambi validi:

```
contributing/raw_content/
├── module_10.ipynb                  ← un solo file, già con il numero del modulo
└── test-automatizzati/              ← oppure una sottocartella con più file dello stesso argomento
    ├── appunti.md
    ├── esempi.py
    └── scaletta-lezione.txt
```

Se il materiale è composto da più file, **preferisci la sottocartella**: è più chiaro cosa fa
parte dello stesso contributo.

**Non inserire qui:** materiale protetto da diritti d'autore di un'altra persona o scuola
(slide, capitoli di libri, dispense di corsi a pagamento), dati personali, credenziali, né file
binari di grandi dimensioni (video, immagini di centinaia di MB). Devi avere il diritto di
contribuire con ciò che carichi — il contributo entra nel repository sotto la licenza del
progetto (vedi [LICENSE](../LICENSE)).

## 4. Genera il modulo con Claude

Apri Claude Code nella root del repository e chiedi, in italiano, qualcosa come:

> Genera il modulo 10 a partire dai file in `contributing/raw_content/test-automatizzati/`.

Claude userà la *skill* **`gerar-modulo-ipynb`**, già presente nel repository. Lei:

1. legge **tutti** i file grezzi che hai indicato;
2. riscrive il contenuto secondo lo standard ufficiale del corso (intestazione, tabella degli
   Argomenti, sezioni numerate, consigli, avvisi, riepilogo finale), correggendo gli errori ed
   espandendo ciò che è superficiale;
3. salva in **`contributing/generated_ipynb/modules/module_NN.ipynb`**;
4. esegue il validatore, che fa girare **tutte** le celle e verifica la struttura, finché il
   risultato non è `APROVADO`.

Se il numero del modulo non è chiaro, Claude chiederà in quale bootcamp e in quale posizione del
corso il modulo va inserito. Se non lo sai, dillo: l'autore può rinumerare in fase di revisione.

Il contenuto è scritto in **portoghese brasiliano**: è la lingua del corso, quindi anche i
moduli nati da materiale in italiano vengono riscritti in portoghese da Claude. La versione
italiana del corso viene poi curata dall'autore — tu non devi tradurre nulla.

Se vuoi eseguire il validatore per conto tuo dopo aver modificato il notebook a mano:

```bash
python .claude/skills/gerar-modulo-ipynb/scripts/validar_notebook.py \
  contributing/generated_ipynb/modules/module_10.ipynb
```

## 5. Genera il progetto pratico (opzionale, ma molto benvenuto)

Ogni modulo ha un **progetto pratico**: un notebook in cui lo studente mette in pratica i
concetti dentro una storia — un'azienda immaginaria, colleghi che chiedono cose in chat,
missioni con risultato atteso, consigli e sfide extra.

Chiedi a Claude:

> Genera il progetto pratico del modulo 10.

Userà la *skill* **`gerar-projeto-pratico-ipynb`**, che accetta come fonte:

- un modulo che hai appena generato, in `contributing/generated_ipynb/modules/`;
- un modulo **già pubblicato**, in `bootcamps/<lang>/<bootcamp>/Módulos/module_NN.ipynb` (usalo
  per creare il progetto di un modulo già online);
- **materiale grezzo** da `contributing/raw_content/`, quando il modulo non esiste ancora;
- **entrambi insieme** — per esempio il modulo più un file grezzo con i dati o lo scenario che
  vuoi usare.

Il risultato va in **`contributing/generated_ipynb/projects/project_NN.ipynb`** (`NN` è il
numero del modulo corrispondente).

Questo progetto ha una garanzia integrata: ogni "Risultato atteso" della consegna è verificato
contro una soluzione che viene eseguita davvero. Se un numero non torna, il notebook **non**
viene salvato — quindi nessun valore della consegna è indovinato a occhio.

```bash
python .claude/skills/gerar-projeto-pratico-ipynb/scripts/validar_projeto.py \
  contributing/generated_ipynb/projects/project_10.ipynb
```

## 6. Rivedi tu stesso prima di aprire la PR

Il validatore garantisce che il notebook funzioni e abbia la struttura corretta. Non garantisce
che il contenuto sia buono — quello è compito tuo. Prima di aprire la *pull request*, controlla:

- [ ] Ho aperto il notebook in Jupyter/Colab e ho eseguito **"Esegui tutto"** senza alcun errore.
- [ ] La tabella degli **Argomenti** corrisponde alle sezioni del notebook, nello stesso ordine.
- [ ] Le spiegazioni dicono **perché** qualcosa funziona, non solo come — un principiante
      capirebbe.
- [ ] I nomi delle variabili sono in **portoghese**, in `snake_case`, descrittivi
      (`quantidade_vendas`, non `qtd`).
- [ ] Non è rimasto nulla del materiale originale che non sia tuo: loghi, nomi di altre scuole,
      "Caderno de Aula", vecchi link personali.
- [ ] Niente fuori ambito: il modulo non usa concetti che il corso insegna solo più avanti.
- [ ] Nel progetto pratico: l'ho eseguito come farebbe uno studente (celle consegnate, prima di
      scrivere qualsiasi cosa) e tutte le consegne hanno senso.

## 7. Commit e pull request

Carica **solo** ciò che fa parte del tuo contributo: i file grezzi e i notebook generati.

```bash
git add contributing/raw_content/test-automatizzati \
        contributing/generated_ipynb/modules/module_10.ipynb \
        contributing/generated_ipynb/projects/project_10.ipynb
git commit -m "contenuto: modulo 10 - test automatizzati"
git push origin contenuto/modulo-10-test
```

Poi apri la *pull request* verso il branch `main`, descrivendo:

1. **Cos'è** il contributo (modulo nuovo, riscrittura, progetto pratico) e a quale bootcamp e
   posizione del corso è destinato.
2. **Da dove viene il materiale grezzo** — è tuo, è originale, hai il diritto di contribuire con
   esso.
3. **Cosa ha detto il validatore** (`APROVADO`) e che hai eseguito il notebook dall'inizio alla
   fine.
4. **Dubbi e decisioni** che l'autore deve rivedere: numerazione, qualcosa che è rimasto fuori,
   un argomento di cui non eri sicuro se ci stesse.

Se è stato Claude a generare il notebook, il resoconto finale che ti ha dato in conversazione è
un'ottima base per questa descrizione.

## 8. Cosa succede dopo

1. L'autore del corso rivede il contenuto nella *pull request*.
2. Se serve, commenta e tu aggiusti sullo stesso branch (bastano nuovi commit).
3. Approvato e fatto il merge, il notebook esce da `contributing/` e va in
   `bootcamps/<lang>/<bootcamp>/Módulos|Projetos/`, che è la cartella del contenuto ufficiale.
4. L'autore pubblica il modulo sul sito (pagine HTML + registrazione nei JSON di
   `course_content/`, con la *skill* `gerar-modulo-html`) e cura la versione italiana.

In altre parole: la tua parte finisce con la *pull request*. Non devi generare HTML, toccare
JSON né tradurre nulla.

---

## Mappa del repository

| Cartella | Cos'è | La tocchi? |
| --- | --- | --- |
| `contributing/raw_content/` | materiale grezzo, l'ingresso delle *skill* | **Sì** — è dove metti i tuoi file |
| `contributing/generated_ipynb/modules/` | moduli `.ipynb` generati | **Sì** — output della *skill* dei moduli |
| `contributing/generated_ipynb/projects/` | progetti pratici `.ipynb` generati | **Sì** — output della *skill* dei progetti |
| `.claude/skills/` | le *skill* di generazione (istruzioni, costruttori e validatori) | Solo se vuoi migliorare il processo stesso |
| `bootcamps/<lang>/<bootcamp>/` | notebook **ufficiali**, già approvati (`Módulos/`, `Projetos/`) | **No** — sola lettura; l'autore sposta qui il contenuto |
| `course_content/<lang>/` | il corso pubblicato: pagine HTML e i JSON (`bootcamps.json`, `modules.json`, `lessons.json`) | **No** — generato dall'autore in fase di pubblicazione |
| `application_content/`, `pages/`, `assets/`, `index.html` | il sito vero e proprio | **No** per i contributi di contenuto |

## Convenzioni rapide

- **Nomi dei file:** `module_NN.ipynb` per i moduli, `project_NN.ipynb` per i progetti, sempre
  con due cifre (`module_07`, non `module_7`).
- **Un modulo per pull request.** Modulo + il suo progetto pratico nella stessa PR va benissimo;
  due moduli diversi, meglio separarli.
- **Lingua del contenuto:** portoghese brasiliano (vedi il punto 4).
- **Pubblico:** principiante assoluto. Termini in inglese in *corsivo*, codice tra `backtick`.
- **Non sovrascrivere mai** un notebook di `bootcamps/` né il materiale grezzo di qualcun altro.

## Migliorare le skill

Le *skill* in `.claude/skills/` fanno parte del progetto e accettano contributi anche loro: una
regola mancante, un validatore più rigoroso, un'istruzione ambigua. Se modifichi una *skill*,
spiega nella *pull request* quale problema reale di contenuto risolve — e, possibilmente, genera
un modulo con la nuova versione per mostrare la differenza.

## Domande

Apri una *issue* descrivendo cosa vuoi contribuire prima di investire troppo tempo, soprattutto
se si tratta di un modulo nuovo: così si può allineare prima tema, numerazione e ambito.

Grazie per il tuo contributo. 🚀
