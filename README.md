# FC Barcelona RAG – AI-agent

## Projektbeskrivelse

Dette projekt er et Retrieval-Augmented Generation-system (RAG) med fokus på FC Barcelona. Systemet kombinerer en vektordatabase og en grafdatabase ved hjælp af Neo4j samt en AI-agent bygget i n8n.

Formålet er at udvikle en AI-assistent, der kan besvare spørgsmål om FC Barcelona ved at hente relevant information fra både strukturerede data og tekstbaserede dokumenter.

Projektet er udarbejdet som en del af Two-Week RAG Project.

## Teknologier

- **n8n:** Bruges til at bygge og forbinde workflows og AI-agenten.
- **Neo4j:** Bruges som både grafdatabase og vektordatabase.
- **Ollama:** Kører sprogmodellen og embedding-modellen lokalt.
- **Llama 3.2:** Bruges som chatmodel.
- **nomic-embed-text:** Bruges til at generere tekst-embeddings.

## Systemets opbygning

Systemet består af flere workflows.

### 1. Data Ingestion Workflow

Dette workflow indsamler og forbereder information om FC Barcelona.

Teksten behandles og omdannes til dokumenter. Derefter bruger en embedding-model teksten til at generere numeriske vektorer, som gemmes i Neo4j.

Workflowet opretter også det vektorindeks, der bruges til semantisk søgning.

### 2. Vector Retrieval Tool

Vector Tool modtager et spørgsmål fra AI-agenten.

Først bruger det `nomic-embed-text` til at omdanne spørgsmålet til en embedding. Derefter søger Neo4j efter dokumenter med lignende vektorer.

Værktøjet bruges, når agenten skal finde information ud fra betydningen og indholdet af tekster, eksempelvis spørgsmål om FC Barcelonas historie.

### 3. Graph Retrieval Tool

Graph Tool søger i Neo4j ved hjælp af Cypher-forespørgsler.

Det henter strukturerede oplysninger om FC Barcelonas spillere, herunder navne, positioner og trøjenumre.

Værktøjet bruges, når agenten skal finde konkrete fakta og relationer i grafdatabasen.

### 4. Graph RAG Workflow

Graph RAG-workflowet henter strukturerede oplysninger om FC Barcelonas trænere.

Det kan blandt andet hente trænernes navne, roller, nationaliteter og profiler.

Det bruger en Cypher-forespørgsel til at hente oplysningerne fra Neo4j.

### 5. Main AI Agent Workflow

Hovedworkflowet modtager spørgsmål gennem n8n's Chat Trigger.

AI-agenten bruger Llama 3.2 som sprogmodel og har adgang til de forskellige retrieval-workflows som værktøjer.

Afhængigt af spørgsmålet kan agenten bruge Vector Tool, Graph Tool eller Graph RAG til at hente information.

Den indhentede information bruges derefter til at formulere det endelige svar.

## Databasens opbygning

Projektet bruger Neo4j til både grafbaseret søgning og vektorbaseret søgning.

### Grafdatabase

Grafdatabasen indeholder strukturerede oplysninger om FC Barcelona.

Projektet bruger følgende nodetyper:

- `Player`
- `Club`
- `Coach`

Relationerne omfatter:

- `PLAYS_FOR`: Forbinder en spiller med en klub.
- `COACHES`: Forbinder en træner med en klub.

Noderne indeholder egenskaber som navne, positioner, trøjenumre, roller, nationaliteter og profiler.

### Vektordatabase

Vektordatabasen indeholder tekstdokumenter og deres embeddings.

Embedding-modellen er `nomic-embed-text:latest`.

Neo4j-vektorindekset hedder:

`document_embedding`

Vektorindekset bruges til at finde dokumenter, der indholdsmæssigt minder om brugerens spørgsmål.

## AI-agentens rolle

AI-agenten fungerer som en informationsassistent med fokus på FC Barcelona.

Dens opgave er at besvare spørgsmål ved hjælp af information, der hentes fra Neo4j's grafdatabase og vektordatabase.

Agenten skal vælge det relevante værktøj ud fra brugerens spørgsmål og bruge de fundne oplysninger til at formulere et forståeligt svar.

Systemet er primært afgrænset til FC Barcelona og den information, der findes i databaserne.

## Test af AI-agenten

AI-agenten er blevet testet med forskellige spørgsmål.

Eksempler på tests:

1. **Hvem er Lamine Yamal?**  
   Denne test undersøger, om agenten kan besvare spørgsmål om en FC Barcelona-spiller.

2. **Fortæl om FC Barcelonas historie.**  
   Denne test undersøger den semantiske søgning gennem vektordatabasen.

3. **Kender du julemanden?**  
   Denne test undersøger, hvordan systemet håndterer spørgsmål, der ikke handler om FC Barcelona.

Screenshots af spørgsmålene, værktøjskaldene og agentens faktiske svar findes i mappen `screenshots`.

## Mappestruktur

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
│   ├── data-ingestion.png
│   ├── main-agent.png
│   ├── vector-tool.png
│   ├── graph-tool.png
│   ├── graph-rag.png
│   └── agent-tests/
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

## Installation og genskabelse af projektet

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

Workflow-eksporter, eksempeldata og instruktioner til opsætning af databasen findes i projektets mapper.

## Konklusion

Projektet viser, hvordan vektorsøgning og grafbaseret søgning kan kombineres i ét RAG-system.

Vektorsøgning bruges til at finde relevant information ud fra semantisk lighed, mens grafdatabasen bruges til at hente konkrete fakta og relationer.

Ved at forbinde begge søgemetoder til en AI-agent i n8n kan systemet anvende forskellige typer information til at besvare spørgsmål om FC Barcelona.
