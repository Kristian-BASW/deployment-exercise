**English** | [Dansk](README.da.md)

# Deployment assignment

Deploy the project's React frontend and Bun server to **Fly.io** so the app is accessible via HTTPS. Follow the tasks to take your local code to an app running online.

## 1. Install the CLI

Install the Fly.io CLI for your system: [macOS](https://fly.io/docs/flyctl/install/#macos), [Windows](https://fly.io/docs/flyctl/install/#windows), or [Ubuntu](https://fly.io/docs/flyctl/install/#linux). Open a new terminal and verify the installation with `fly version`.

## 2. Log in

Run `fly auth login` and log in to your Fly.io account in the browser. Return to the terminal once you have logged in.

## 3. Create the app

Run `fly launch --no-deploy` in the project directory and follow the prompts. Choose an app name and a region close to you. Let Fly.io generate the necessary deployment files.

## 4. Choose a small machine

Make sure the app's machine uses **1 shared vCPU and 256 MB RAM**. Update the `[[vm]]` section in `fly.toml` to include the following values, and remove any conflicting size settings:

```toml
[[vm]]
  cpu_kind = "shared"
  cpus = 1
  memory = "256mb"
```

See the [CPU and RAM guide](https://fly.io/docs/launch/scale-machine/) for help. The settings in this file also apply to future deployments.

## 5. Deploy and check resources

Run `fly deploy`. Then use `fly scale show` to check that the deployed machines show `shared`, `1` CPU, and `256 MB` RAM. If the values do not match, update `fly.toml` and deploy again.

## 6. Test the app

Open the app with `fly apps open` and try the API tester. Check that both the page and the API work with the selected machine size. Use `fly logs` to investigate errors if the app does not work.

## 7. Make a change

Change the heading in `src/App.tsx` and run `fly deploy` again. Reload the public page and check that your change is visible.

## 8. Use your .NET API

Take your .NET API from the previous lesson and put it in the `api` directory. Move the current Bun/React project and its deployment files into the `web` directory so the structure looks like this (the API filenames are examples):

```text
deployment-exercise/
├── README.md
├── web/
│   ├── ...
└── api/
    └── ...
```

Next, make the API deployable on Fly.io.

## 9. Connect the web app to the API

Try connecting the web app to your API. Display the minions you receive from the API.

## 10. Continue to the advanced assignment or skip it

Deploy the web app and API as two containers on one Fly Machine in the [advanced assignment](ADVANCED.md).

## 11. Clean up

Delete the assignment's app and associated resources on Fly.io when you are finished. Check the dashboard to make sure no resources remain that you no longer use.
