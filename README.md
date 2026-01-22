# PR Preview Environments with GitHub Actions, Docker & Traefik

This repository demonstrates a **real-world CI/CD pattern** to automatically create **dynamic preview environments (review apps)** for every Pull Request.

Each Pull Request is deployed as an isolated environment with its **own public URL**, allowing teams to review changes before merging—similar to **Vercel**, **Netlify**, or **Heroku Review Apps**, but **self-hosted**.

---

## 🌍 What Problem Does This Solve?

In many teams, reviewing changes before merging is difficult because:

- Changes are only available locally  
- QA environments are shared and unstable  
- Reviewers can’t easily test a PR in a browser  

This setup solves that by automatically deploying **one environment per Pull Request**.

---

## ✨ What This CI/CD Pipeline Does

### ✅ On Pull Request Open / Update

1. GitHub Actions builds a Docker image for the PR  
2. The image is deployed to a VPS via SSH  
3. A container is started with **dynamic Traefik labels**  
4. Traefik exposes the app using a **unique subdomain**  

Example:

```bash
https://pr-42.example.dev  # Pull Request #42
https://pr-105.example.dev # Pull Request #105
```

### 🧹 On Pull Request Close (Automatic Cleanup)

When a Pull Request is closed or merged, a dedicated cleanup workflow is triggered to fully remove the preview environment.

The cleanup process:

- Stops the running container for the Pull Request  
- Removes the container from Docker  
- Deletes the Docker image associated with that PR  

This guarantees:

- No unused containers running on the server  
- No orphaned Docker images consuming disk space  
- A clean and predictable environment lifecycle  

Each preview environment exists only for the lifetime of the Pull Request.
