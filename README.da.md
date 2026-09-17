[English](README.md) | **Dansk**

# Deployment exercise

Deploy projektets React-frontend og Bun-server til **Fly.io**, så appen kan tilgås via HTTPS. Brug opgaverne til at komme fra lokal kode til en app, der kører online.

## 1. Installer CLI

Installer Fly.io CLI til dit system: [macOS](https://fly.io/docs/flyctl/install/#macos), [Windows](https://fly.io/docs/flyctl/install/#windows) eller [Ubuntu](https://fly.io/docs/flyctl/install/#linux). Åbn en ny terminal, og kontroller installationen med `fly version`.

## 2. Log ind

Kør `fly auth login`, og log ind på din Fly.io-konto i browseren. Vend tilbage til terminalen, når login er gennemført.

## 3. Opret appen

Kør `fly launch --no-deploy` i projektmappen, og følg guiden. Vælg et appnavn og en region tæt på dig. Lad Fly.io generere de nødvendige deployment-filer.

## 4. Vælg en lille maskine

Sørg for, at appens maskine bruger **1 delt vCPU og 256 MB RAM**. Tilpas `[[vm]]`-afsnittet i `fly.toml`, så det indeholder følgende værdier, og fjern eventuelle modstridende størrelsesindstillinger:

```toml
[[vm]]
  cpu_kind = "shared"
  cpus = 1
  memory = "256mb"
```

Find hjælp i [guiden til CPU og RAM](https://fly.io/docs/launch/scale-machine/). Indstillingerne i filen bruges også ved senere deployments.

## 5. Deploy og kontroller ressourcer

Kør `fly deploy`. Kontroller derefter med `fly scale show`, at de deployede maskiner viser `shared`, `1` CPU og `256 MB` RAM. Hvis værdierne ikke passer, skal du rette `fly.toml` og deploye igen.

## 6. Test appen

Åbn appen med `fly apps open`, og prøv API-testeren. Kontroller, at både siden og API'et virker med den valgte maskinstørrelse. Brug `fly logs` til at undersøge fejl, hvis appen ikke virker.

## 7. Lav en ændring

Skift overskriften i `src/App.tsx`, og kør `fly deploy` igen. Genindlæs den offentlige side, og kontroller, at din ændring er kommet med.

## 8. Brug dit .NET API

Tag dit .NET API fra sidste lektion, og læg det i mappen `api`. Flyt det nuværende Bun/React-projekt og dets deployment-filer til mappen `web`, så strukturen ser sådan ud (API-filnavnene er eksempler):

```text
deployment-exercise/
├── README.md
├── web/
│   ├── src/
│   │   ├── App.tsx
│   │   ├── APITester.tsx
│   │   └── ...
│   ├── package.json
│   ├── bun.lock
│   ├── bunfig.toml
│   ├── bun-env.d.ts
│   ├── tsconfig.json
│   ├── Dockerfile
│   ├── .dockerignore
│   └── fly.toml
└── api/
    ├── Program.cs
    ├── MitApi.csproj
    ├── appsettings.json
    └── ...
```

Herefter skal du lave api'et deployable med fly.io

## 9. Connecte webapp til API'et

Prøv at se om du kan connecte web-appen til dit API. Her skal du vise de minions du får fra API'et.


## 10. Gå til den avancerede opgave eller spring over

Deploy webappen og API'et som to containere på én Fly Machine i [den avancerede opgave](ADVANCED.da.md).

## 11. Ryd op

Slet opgavens app og tilknyttede ressourcer på Fly.io, når du er færdig. Kontroller i dashboardet, at der ikke står ressourcer tilbage, som du ikke længere bruger.
