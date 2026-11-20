# Analisi-Geospaziale-e-Toponomastica-Enclavi-Gallo-Italiche-in-Sicilia-Tramite-OpenStreetMap-e-Python
Panoramica del Prodotto
Questo prodotto consiste in un Notebook di calcolo interattivo (sviluppato in ambiente Python tramite Google Colab) progettato per l'estrazione, l'analisi, la pulizia e la visualizzazione geospaziale di dati toponomastici. Il software è specificamente configurato per indagare il patrimonio linguistico e culturale delle enclavi gallo-italiche in Sicilia. Attraverso l'integrazione di API geografiche e librerie di data science, il notebook automatizza il recupero di informazioni da OpenStreetMap (OSM), trasformando dati grezzi in dataset strutturati e mappe interattive di immediata fruizione.

Contesto Scientifico e Metodologico
La toponomastica (lo studio dei nomi dei luoghi) rappresenta una preziosa testimonianza storica e linguistica. Le isole linguistiche gallo-italiche della Sicilia (come Novara di Sicilia, Nicosia, Piazza Armerina, Aidone, San Fratello, Fondachelli-Fantina e Sperlinga) conservano retaggi culturali unici. Questo notebook nasce per supportare la ricerca in ambito umanistico-digitale (Digital Humanities) e di Citizen Science, fornendo uno strumento riproducibile per censire, mappare e analizzare la presenza di toponimi locali, dialettali, alternativi o storici rispetto alla denominazione ufficiale italiana.

Funzionalità e Architettura del Codice
Il flusso di lavoro del software è strutturato in moduli sequenziali:

Estrazione Dati (Data Extraction): Tramite la libreria osmnx, il sistema interroga l'API di OpenStreetMap per estrarre elementi geografici (strade, montagne, punti di interesse, edifici) all'interno di un perimetro comunale predefinito. La query è ottimizzata per recuperare i tag relativi ai nomi ufficiali (name) e alle denominazioni locali/alternative (alt_name, loc_name, name:scn per il siciliano).

Elaborazione e Pulizia (Data Wrangling): Utilizzando pandas, i dati vettoriali grezzi (GeoDataFrames) vengono filtrati, ripuliti dalle geometrie non necessarie (per permettere l'esportazione in formati tabellari) e aggregati in un unico grande dataset. Viene gestita automaticamente l'eterogeneità dei campi e la presenza di valori mancanti (NaN).

Analisi Statistica: Il sistema calcola metriche descrittive, come il numero totale di elementi georeferenziati per ciascun comune e la percentuale di toponimi che presentano denominazioni locali o storiche.

Visualizzazione Geospaziale (Web Mapping): Tramite la libreria folium, vengono generate mappe interattive in formato HTML. Queste mappe utilizzano tile di base (come CartoDB o Esri) e posizionano marker informativi sui territori comunali, restituendo un impatto visivo immediato della densità dei dati estratti.

Visualizzazione Dati (Data Visualization): Utilizzando matplotlib e seaborn, il notebook genera infografiche e grafici a barre (es. la "Distribuzione della densità di record toponomastici per enclave") ad alta risoluzione (300 DPI), pronti per essere inclusi in pubblicazioni scientifiche o reportistica.

Output Generati
L'esecuzione completa del notebook produce i seguenti artefatti digitali:

Un dataset strutturato in formato .csv (toponomastica_gallo_italica_sicilia.csv), pronto per ulteriori analisi statistiche o linguistiche.

Mappe interattive in formato .html, esplorabili tramite qualsiasi browser web.

Grafici statistici in formato .png ad alta definizione.

Riproducibilità e Sviluppi Futuri
Il codice è progettato per essere altamente modulare: modificando semplicemente la lista dei comuni (variabile comuni_gallo_italici) o i parametri di ricerca, il notebook può essere adattato per analizzare qualsiasi altro contesto geografico o linguistico (es. minoranze arbëreshë, occitane, etc.) o integrato in progetti più ampi di mappatura partecipativa. Il prodotto è rilasciato in un'ottica di Open Science, incoraggiando il riutilizzo, la modifica e il miglioramento da parte della comunità scientifica e degli sviluppatori.


Descrizione Tecnica Aggiuntiva (Architettura, Pipeline e Specifiche di Implementazione)
Architettura del Software e Stack Tecnologico
Il presente artefatto computazionale è implementato come un Computational Notebook (formato .ipynb), ottimizzato per l'esecuzione su runtime cloud-based (Google Colab, Python 3). L'architettura si fonda su un'integrazione sinergica di librerie per l'analisi geospaziale e la data visualization. Nello specifico, il core engine si avvale di OSMnx (v2.1.1) per l'astrazione topologica e il parsing dei dati vettoriali, GeoPandas e Shapely per la gestione delle strutture dati geometriche (GeoDataFrame), Pandas per il data wrangling tabellare, e Folium (basato su Leaflet.js) per il rendering cartografico dinamico. L'output analitico statico è affidato ai backend di Matplotlib e Seaborn.

Pipeline di Acquisizione Dati (ETL) e Integrazione API
Il modulo di Data Extraction implementa un'interrogazione programmatica verso le API di OpenStreetMap (nello specifico, il framework Overpass). A seguito della deprecazione del metodo geometries_from_place, l'algoritmo è stato refattorizzato per utilizzare la funzione features_from_place, garantendo la retrocompatibilità e l'efficienza computazionale. Il sistema esegue query spaziali con filtri semantici sui tag OSM (name, alt_name, loc_name, name:scn), estraendo feature georeferenziate (puntuali, lineari e poligonali). La gestione delle eccezioni (mediante costrutti try-except) assicura la resilienza della pipeline contro timeout delle API o vuoti di risposta per specifiche bounding box comunali.

Data Wrangling e Gestione delle Strutture Dati Geospaziali
La fase di Data Transformation prevede un processo di normalizzazione e riduzione dimensionale dei GeoDataFrame. Viene eseguita un'operazione di feature selection per isolare i campi informativi di interesse linguistico, seguita dalla de-strutturazione della colonna geometrica (geometry). Quest'ultimo passaggio è cruciale per la serializzazione dei dati in formato flat-file (CSV), operando una conversione da tipi di dato spaziali complessi (WKT/WKB) a tipi primitivi, al fine di garantire l'integrità e la portabilità del dataset per analisi statistiche esterne. Il codice gestisce nativamente i valori nulli (NaN) e preserva l'encoding Unicode, fondamentale per la corretta rappresentazione dei foni dialettali gallo-italici (es. caratteri speciali come "ḍḍ" in "Casteḍḍu d'Orlannu").

Rendering Geospaziale e Visualizzazione Statistica
La componente di Data Visualization si articola su due livelli. A livello macro, il modulo folium genera artefatti HTML5 interattivi, sfruttando tile layer vettoriali (es. CartoDB Positron, Esri WorldTopoMap) per la rappresentazione del contesto orografico e infrastrutturale. La georeferenziazione dei marker (con popup informativi generati dinamicamente) avviene tramite l'iniezione di coordinate WGS84 (EPSG:4326). A livello micro, l'analisi esplorativa dei dati (EDA) viene restituita tramite grafici a barre orizzontali generati con seaborn, caratterizzati da una palette sequenziale (Blues_r) e da annotazioni parametriche (bounding box dei patch). Il rendering finale è esportato in formato raster ad alta densità (DPI 300) per garantirne l'idoneità alla stampa accademica.

Robustezza, Riproducibilità e Scalabilità
Il software è ingegnerizzato per garantire l'assoluta riproducibilità del flusso di lavoro. La parametrizzazione della lista dei comuni target (variabile iterabile comuni_gallo_italici) consente l'estensione modulare dell'analisi a ulteriori enclave linguistiche (es. comunità arbëreshë o occitane) senza alterazioni del codice sorgente. La correzione di errori sintattici e l'adozione di pratiche di clean code (es. corretta denominazione delle variabili, come df_pulito) assicurano la stabilità dell'esecuzione. Il prodotto è inoltre provvisto di meccanismi di salvataggio incrementale (checkpointing) dei dataset intermedi (.csv) e dei file cartografici (.html), facilitando l'interscambio dei dati all'interno di ecosistemi di ricerca Open Science e Citizen Science.
