## Test af AI-agenten

AI-agenten er blevet testet med forskellige spørgsmål for at undersøge, hvordan den bruger databaserne, og hvordan den håndterer spørgsmål uden for projektets emne.

Følgende screenshots dokumenterer testene:

- `FC Barcelonas historie.png` – Test af vektorsøgning med et spørgsmål om FC Barcelonas historie.
- `Korret svar fra databasen.png` – Eksempel på et korrekt svar baseret på information fra databasen.
- `Svarer ikke.png` – Test med et spørgsmål, som systemet ikke kunne besvare ud fra den tilgængelige information.

Testene viser både, hvordan systemet finder relevant information, og en begrænsning ved dets vidensgrundlag.

## Screenshots af workflows

Følgende screenshots viser de forskellige workflows:

- `Data import.png` – Data ingestion-workflowet, der indsamler og gemmer information i databasen.
- `FC Barcelona – Graph RAG.png` – Graph RAG-workflowet, der henter information om trænere.
- `FC Barcelona – Graph Tool.png` – Graph Tool, der henter strukturerede oplysninger om spillere.
- `FC Barcelona – Vector Tool.png` – Vector Tool, der søger efter semantisk relevant tekst.

## Mappestruktur

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
