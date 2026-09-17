**English** | [Dansk](ADVANCED.da.md) · [Back to the assignment](README.md)

# Advanced assignment: Two containers on one VM

Deploy your Bun/React web app and .NET API as **two containers on the same Fly Machine** using Docker Compose. Use the `web` and `api` directories from the first assignment.

Read [Fly.io's guide to Compose and multi-container Machines](https://fly.io/docs/machines/guides-examples/multi-container-machines/#using-docker-compose). This requires `flyctl` version 0.3.152 or newer. Fly.io translates the Compose file into containers on a Machine.

## 1. Prepare the API image

Create a Dockerfile for the API, then build and push the image to a container registry that Fly.io can pull from. Use a version number as the tag so you know which version you are deploying.

## 2. Create a Compose file

Create `compose.yaml` in the project root with two services: `web` and `api`. Use `build: ./web` for the web app and `image:` with your prebuilt API image. Fly.io requires exactly one service with `build`.

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

## 3. Connect the web app and API

Make the Bun server forward the browser's `/api` requests to the .NET API. On Fly.io, the containers share a network and can communicate through `localhost` on different ports, for example web on `3000` and API on `8080`. The browser should call the web app's public address.

## 4. Configure Fly.io

Create a separate Fly App for this assignment with a `fly.toml` in the project root. Enable Compose with `[build.compose]` and set `http_service.internal_port` to the web server's port. Check that both servers use the chosen ports.

## 5. Deploy on one VM

Run `fly deploy --ha=false` from the project root. Use `fly machine list` to check that the app has exactly one Machine, then inspect its details in the dashboard to confirm that it contains both `web` and `api`.

## 6. Test the complete app

Open the public page and check that it displays minions from the .NET API. Use logs to investigate errors. Start with 1 shared vCPU and 256 MB RAM, and investigate memory usage if the two services cannot run reliably together.

## 7. Clean up

Delete the assignment's Fly App and associated resources when you are finished.

## Automatic build and deployment

With Docker running and Fly.io authentication configured, run `bun run deploy.ts` from the project root. The script builds the API image for `linux/amd64`, pushes it to Fly.io's registry, then starts deployment, which builds the web image. It uses `api.image` from `compose.yaml`, which must refer to the app in `fly.toml`. The same image tag is updated on each run. The script stops if any step fails.
