[English](ADVANCED.md) | **Dansk** · [Tilbage til opgaven](README.da.md)

# Avanceret opgave: To containere på én VM

Deploy din Bun/React-webapp og dit .NET API som **to containere på samme Fly Machine** med Docker Compose. Brug mapperne `web` og `api` fra den første opgave.

Læs [Fly.io's guide til Compose og multi-container Machines](https://fly.io/docs/machines/guides-examples/multi-container-machines/#using-docker-compose). Det kræver `flyctl` version 0.3.152 eller nyere. Fly.io omsætter Compose-filen til containere på en Machine.

## 1. Klargør API-imaget

Lav en Dockerfile til API'et, og byg og push imaget til et containerregistry, som Fly.io kan hente fra. Brug et versionsnummer som tag, så du ved, hvilken version du deployer.

## 2. Lav en Compose-fil

Opret `compose.yaml` i projektets rod med to services: `web` og `api`. Brug `build: ./web` til webappen og `image:` med dit færdigbyggede API-image. Fly.io kræver præcis én service med `build`.

```text
 deployment-exercise/
 ├── compose.yaml
 ├── fly.toml
 ├── web/
 │   ├── Dockerfile
 │   └── ...
 └── api/
     ├── Dockerfile
     └── ...
```

## 3. Forbind web og API

Lad Bun-serveren videresende browserens `/api`-kald til .NET API'et. På Fly.io deler containerne netværk og kan kommunikere via `localhost` på forskellige porte, eksempelvis web på `3000` og API på `8080`. Browseren skal kalde webappens offentlige adresse.

## 4. Konfigurer Fly.io

Opret en separat Fly App til denne opgave med en `fly.toml` i projektets rod. Aktiver Compose med `[build.compose]`, og sæt `http_service.internal_port` til webserverens port. Kontroller, at begge servere bruger de valgte porte.

## 5. Deploy på én VM

Kør `fly deploy --ha=false` fra projektets rod. Kontroller med `fly machine list`, at appen har præcis én Machine, og se i dashboardets Machine-detaljer, at den indeholder både `web` og `api`.

## 6. Test den samlede app

Åbn den offentlige side, og kontroller, at den viser minions fra .NET API'et. Brug logs til at undersøge fejl. Start med 1 delt vCPU og 256 MB RAM, og undersøg hukommelsesforbruget, hvis de to tjenester ikke kan køre stabilt sammen.

## 7. Ryd op

Slet opgavens Fly App og tilknyttede ressourcer, når du er færdig.

## Automatisk build og deployment

Med Docker startet og Fly.io-login på plads kan du køre `bun run deploy.ts` fra projektets rod. Scriptet bygger API-imaget til `linux/amd64`, uploader det til Fly.io's registry og starter derefter deployment, hvor web-imaget bygges. Det bruger `api.image` fra `compose.yaml`, som skal pege på appen i `fly.toml`. Det samme image-tag opdateres ved hver kørsel. Scriptet stopper, hvis et trin fejler.
