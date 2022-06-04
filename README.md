# Evaluating DevContainer for Service & Product Engineering

Goal:

- Easy engineering onboarding with one command
- By using VS Code or Github CodeSpaces to open a mono repo in a container
- Than just run `tilt up`
- Have the whole system running with live updates on code changes

Problem:

- Remote containers have many bugs between architectures. I.e. things that run in codespace run or don't build on apple silicon etc. Even though the container is created by the vs code gui — no manual dockerfiles.
- Build not reliable. I.e. `postCreateCommand` of the `devcontainer.json` not executing reliably.
- Even when run, visual test runners like cypress require a gui shell to be interactive. Though there are a few approaches [enabling VNC browser](https://devopstar.com/2022/01/03/cypress-testing-in-devcontainers-and-github-codespaces) views.

---

<i>original readme below</i>

---

# Tilt Avatars - Getting Started Sample Project

[![Build Status](https://circleci.com/gh/tilt-dev/tilt-avatars/tree/main.svg?style=shield)](https://circleci.com/gh/tilt-dev/tilt-avatars)

Tilt Avatars is a small sample project used by the Tilt Getting Started guide.

It consists of a Python web API backend to generate avatars and a Javascript SPA (single page app) frontend.
If you are not a Python or Javascript guru, don't panic!
The focus of this project is on introducing the `Tiltfile` and other Tilt concepts: the services are demonstrative to support the guide, but you do not need to understand the code within them to be successful.

We also know that no two projects are alike!
This project uses `Dockerfile`s with Docker as the build engine and `kubectl` friendly YAML files.
These only cover a small subset of Tilt functionality but have been chosen to minimize dependencies.

Even if you're using other technologies (e.g. `podman` or `helm`), we recommend starting here to learn the Tilt fundamentals.
After you're comfortable with how Tilt works, we've got a more comprehensive guide on authoring your first `Tiltfile` from scratch that covers much more.

## Running

You'll need to first install Tilt and prerequisites (Docker + local Kubernetes cluster).

Once you've installed Tilt, clone this repo and launch Tilt:

```sh
git clone https://github.com/tilt-dev/tilt-avatars.git
cd tilt-avatars
tilt up
```

## Need Help?

Join us on the Kubernetes Slack in `#tilt`!
