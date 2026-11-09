# gitlab-runner

The GitLab Runner on the VPS, using the Docker executor.

> A published snapshot of a repository that runs on my self-hosted GitLab.
> More about the lab: [homelab](https://github.com/MatejK97/homelab)

## Design

- **Jobs run unprivileged.** The runner container uses the host's Docker
  socket to start job containers, but the jobs themselves get no socket and no
  privileged mode, so a pipeline can't take over the server.
- **Deploys are pinned here.** This runner is tagged `vps`, and deploy jobs
  require that tag; a second runner on the home VM only picks up validation
  and scan jobs.
- **`config.toml` is not in Git:** it holds the runner's authentication token.
  It's mounted read-only.
- **Validate only.** CI checks the compose file; the runner isn't redeployed
  by a pipeline that's running on it.
