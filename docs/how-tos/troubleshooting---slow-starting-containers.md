# Troubleshooting - Slow starting containers using 100% CPU

Some docker images used in Semantic Works stacks, notably those based on sbcl (common lisp) and elixir images, are very slow and CPU intensive to start if the limits of open file descriptors are very high for the container. This leads to a process using 100% of a CPU for some time before that container becomes usable. This can be worked around by setting the defaults for new containers in the docker daemon config (/etc/docker/daemon.json (create it if it doesn't exist)):

```json
{
  "default-ulimits": {
    "nofile": {
      "Hard": 104583,
      "Name": "nofile",
      "Soft": 104583
    }
  }
}
```

Or, if you want these high defaults for some reason, you can set per-container limits in a docker-compose file for each of the mu-project services:

```yml
    ulimits:
      nofile:
        soft: 104583
        hard: 104583
```

