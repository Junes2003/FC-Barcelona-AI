# FC Barcelona RAG – AI-agent

## 1. Projektbeskrivelse

Dette projekt er et Retrieval-Augmented Generation-system (RAG) med fokus på FC Barcelona. Systemet kombinerer en vektordatabase og en grafdatabase ved hjælp af Neo4j samt en AI-agent bygget i n8n.

Formålet er at udvikle en AI-assistent, der kan besvare spørgsmål om FC Barcelona ved at hente relevant information fra både strukturerede data og tekstbaserede dokumenter.

Projektet er udarbejdet som en del af Two-Week RAG Project.

## 2. Teknologier

- **n8n:** Bruges til at bygge og forbinde workflows og AI-agenten.
- **Neo4j:** Bruges som både grafdatabase og vektordatabase.
- **Ollama:** Kører sprogmodellen og embedding-modellen lokalt.
- **Llama 3.2:** Bruges som chatmodel.
- **nomic-embed-text:** Bruges til at generere tekst-embeddings.
- **Docker:** Bruges til at køre de lokale tjenester.

## 3. Systemets opbygning

Systemet består af flere workflows, som arbejder sammen for at hente og behandle information om FC Barcelona.

### 3.1 Data Ingestion Workflow

Dette workflow indsamler og forbereder information om FC Barcelona.

Workflowet består af følgende trin:

1. Henter information om FC Barcelona.
2. Forbereder og organiserer teksten.
3. Behandler data ved hjælp af JavaScript.
4. Opretter dokumenter ud fra teksten.
5. Genererer embeddings med embedding-modellen.
6. Gemmer dokumenter og embeddings i Neo4j.
7. Opretter et vektorindeks til semantisk søgning.

Formålet er at gøre informationen tilgængelig for AI-agenten gennem vektorsøgning.

### 3.2 Vector Retrieval Tool

Vector Tool modtager et spørgsmål fra AI-agenten.

Først bruger værktøjet `nomic-embed-text:latest` til at omdanne spørgsmålet til en embedding. Derefter søger Neo4j efter dokumenter med lignende vektorer.

Værktøjet bruges, når agenten skal finde information ud fra betydningen og indholdet af tekster, eksempelvis spørgsmål om FC Barcelonas historie.

### 3.3 Graph Retrieval Tool

Graph Tool søger i Neo4j ved hjælp af Cypher-forespørgsler.

Det henter strukturerede oplysninger om FC Barcelonas spillere, herunder:

- Spillernes navne.
- Spillernes positioner.
- Spillernes trøjenumre.
- Hvilken klub spillerne tilhører.

Værktøjet bruges, når agenten skal finde konkrete fakta og relationer i grafdatabasen.

### 3.4 Graph RAG Workflow

Graph RAG-workflowet henter strukturerede oplysninger om FC Barcelonas trænere.

Det kan blandt andet hente:

- Trænernes navne.
- Deres roller.
- Deres nationaliteter.
- Deres profiler.
- Hvilken klub de er tilknyttet.

Det bruger en Cypher-forespørgsel til at hente oplysningerne fra Neo4j.

### 3.5 Main AI Agent Workflow

Hovedworkflowet modtager spørgsmål gennem n8n's Chat Trigger.

AI-agenten bruger Llama 3.2 som sprogmodel og har adgang til de forskellige retrieval-workflows som værktøjer.

Afhængigt af spørgsmålet kan agenten bruge:

- **Vector Tool:** Til at finde relevant information i tekstdokumenter.
- **Graph Tool:** Til at hente strukturerede oplysninger om spillere.
- **Graph RAG:** Til at hente strukturerede oplysninger om trænere.

Den indhentede information bruges derefter til at formulere det endelige svar.

## 4. Databasens opbygning

Projektet bruger Neo4j til både grafbaseret søgning og vektorbaseret søgning.

### 4.1 Grafdatabase

Grafdatabasen indeholder strukturerede oplysninger om FC Barcelona.

Projektet bruger følgende nodetyper:

- `Player`
- `Club`
- `Coach`

Relationerne omfatter:

- `PLAYS_FOR`: Forbinder en spiller med en klub.
- `COACHES`: Forbinder en træner med en klub.

Noderne indeholder egenskaber som navne, positioner, trøjenumre, roller, nationaliteter og profiler.

### 4.2 Vektordatabase

Vektordatabasen indeholder tekstdokumenter og deres embeddings.

Embedding-modellen er:

`nomic-embed-text:latest`

Neo4j-vektorindekset hedder:

`document_embedding`

Vektorindekset bruges til at finde dokumenter, der indholdsmæssigt minder om brugerens spørgsmål.

Vektorsøgning er især nyttig, når brugeren stiller spørgsmål med forskellige formuleringer, hvor betydningen er vigtigere end de præcise søgeord.

## 5. AI-agentens rolle

AI-agenten fungerer som en informationsassistent med fokus på FC Barcelona.

Dens opgave er at besvare spørgsmål ved hjælp af information, der hentes fra Neo4j's grafdatabase og vektordatabase.

Agenten skal vælge det relevante værktøj ud fra brugerens spørgsmål og bruge de fundne oplysninger til at formulere et forståeligt svar.

Systemet er primært afgrænset til FC Barcelona og den information, der findes i databaserne.

### 5.1 Model og embedding

- **Chatmodel:** `llama3.2:latest`
- **Embedding-model:** `nomic-embed-text:latest`

Chatmodellen bruges til at forstå brugerens spørgsmål og formulere svar. Embedding-modellen bruges til at omdanne tekst og spørgsmål til vektorer, så Neo4j kan udføre semantisk søgning.

## 6. Test af AI-agenten

AI-agenten er blevet testet med forskellige spørgsmål for at undersøge, hvordan den bruger databaserne, og hvordan den håndterer spørgsmål uden for projektets emne.

Følgende screenshots dokumenterer testene:

- `FC Barcelonas historie.png` – Test af vektorsøgning med et spørgsmål om FC Barcelonas historie.
- `Korret svar fra databasen.png` – Eksempel på et korrekt svar baseret på information fra databasen.
- `Svarer ikke.png` – Test med et spørgsmål, som systemet ikke kunne besvare ud fra den tilgængelige information.

Testene viser både, hvordan systemet finder relevant information, og en begrænsning ved dets vidensgrundlag.

## 7. Screenshots af workflows

Følgende screenshots viser de forskellige workflows:

- `Data import.png` – Data ingestion-workflowet, der indsamler og gemmer information i databasen.
- `FC Barcelona – Graph RAG.png` – Graph RAG-workflowet, der henter information om trænere.
- `FC Barcelona – Graph Tool.png` – Graph Tool, der henter strukturerede oplysninger om spillere.
- `FC Barcelona – Vector Tool.png` – Vector Tool, der søger efter semantisk relevant tekst.

## 8. Mappestruktur

Projektets filer er organiseret således:

```text
FC-Barcelona-RAG/
│
├── README.md
│
├── workflows/
│   ├── data-ingestion.json
│   ├── main-agent.json
│   ├── vector-tool.json
│   ├── graph-tool.json
│   └── graph-rag.json
│
├── screenshots/
│   ├── Data import.png
│   ├── FC Barcelona – Graph RAG.png
│   ├── FC Barcelona – Graph Tool.png
│   ├── FC Barcelona – Vector Tool.png
│   ├── FC Barcelonas historie.png
│   ├── Korret svar fra databasen.png
│   └── Svarer ikke.png
│
├── agent/
│   ├── instructions.txt
│   ├── model-and-tools.txt
│   └── prompts.txt
│
└── data/
    ├── sample-data.json
    ├── neo4j-structure.txt
    └── setup-instructions.txt
```

## 9. Installation og genskabelse af projektet

Følgende trin beskriver, hvordan projektet kan sættes op igen:

1. Installer og start Docker.
2. Opsæt en selvhostet n8n-instans.
3. Installer og start Ollama.
4. Hent de nødvendige modeller:
   - `llama3.2:latest`
   - `nomic-embed-text:latest`
5. Opret en Neo4j-database med navnet `fc-barcelona-rag`.
6. Konfigurer forbindelserne til Neo4j og Ollama i n8n.
7. Importer workflow-JSON-filerne fra mappen `workflows`.
8. Kør Data Ingestion-workflowet for at indsætte data i Neo4j.
9. Opret eller kontrollér vektorindekset `document_embedding`.
10. Aktivér retrieval-workflows og hovedworkflowet med AI-agenten.
11. Åbn n8n's chatinterface, og test agenten.

Workflow-eksporter, eksempeldata og instruktioner til opsætning af databasen skal findes i projektets mapper.

## 10. Konklusion

Projektet viser, hvordan vektorsøgning og grafbaseret søgning kan kombineres i ét RAG-system.

Vektorsøgning bruges til at finde relevant information ud fra semantisk lighed, mens grafdatabasen bruges til at hente konkrete fakta og relationer.

Ved at forbinde begge søgemetoder til en AI-agent i n8n kan systemet anvende forskellige typer information til at besvare spørgsmål om FC Barcelona.

Projektet har givet praktisk erfaring med opsætning af Neo4j, databehandling i n8n, embeddings med Ollama og udvikling af en AI-agent med flere retrieval-værktøjer.
