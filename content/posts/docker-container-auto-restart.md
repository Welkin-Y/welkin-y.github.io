---
title: "Docker Container Auto Restart"
date: 2025-03-27T17:33:53+09:00
draft: false
---

> [!NOTE]
>
> Archived answers from chatGPT
>
To make a Docker container start automatically after a system reboot,
you need to set its restart policy when running or updating the container. Here's how you can do it:

1. Use --restart flag when running a container
Run your container with one of the --restart policies:

```bash
docker run -d --restart unless-stopped my-container-image
```

Restart Policies:

- no (default) → Container does not restart.

- always → Container always restarts after a reboot or crash.

- unless-stopped → Container restarts unless explicitly stopped.

- on-failure → Restarts only if the container exits with an error.

👉 Recommended: unless-stopped ensures the container starts again
after a reboot but doesn’t restart if you manually stopped it.

1. Update an existing container’s restart policy
If the container is already running, update its restart policy with:

```bash
docker update --restart unless-stopped my-container-name
```

1. Enable Docker service at boot (if needed)
Ensure the Docker service itself starts on boot:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

To check its status:

```bash
sudo systemctl status docker
```

1. Verify Containers Restart After Reboot
Restart your system:

```bash
sudo reboot
```

After rebooting, check running containers:

```bash
docker ps -a
```

Your container should be running if its restart policy is set correctly.
