# Week 8 Comparison: On-Premise Docker vs. Cloud Run

| Dimension | On-Premise Docker (Weeks 3–5) | Cloud Run (Week 8) |
|---|---|---|
| Infrastructure setup | 3 VMs created, Docker installed on each | No VM management. Cloud Run runs the container serverlessly. |
| Deployment command | SSH to VM, docker build, docker run | GitHub Actions builds, pushes, and deploys with gcloud run deploy. |
| TLS / HTTPS | Not configured manually | HTTPS URL is created automatically by Cloud Run. |
| Scaling approach | Manual scaling by adding or changing VMs | Automatic scaling, including scale to zero. |
| Port management | Ports 5000/5001/5002 per environment | Cloud Run handles routing through HTTPS. |
| Cost when idle | VM runs 24/7 even with no traffic | Cloud Run can scale to zero when idle. |
| Rollback | Re-deploy previous image manually | Revisions and commit SHA image tags make rollback easier. |
| Secrets management | GitHub Secrets and SSH keys used in workflows | OIDC replaces long-lived SSH key secrets. |

## Reflection Questions

### Q1: Which approach required more manual steps from push to live URL?

The on-premise Docker approach required more manual steps. I had to SSH into machines, build Docker images, run containers, manage ports, and check each VM. Cloud Run removed most of those steps because the workflow builds, pushes, and deploys the container automatically.

### Q2: How do you know which version of code is running in production?

With on-premise Docker, it is harder to confirm unless I carefully track the image or deployment manually. With Cloud Run, the image is tagged with the commit SHA, such as `393a25c`, so I can connect the running service back to the exact GitHub commit.

### Q3: What is the security advantage of scale-to-zero beyond cost savings?

Scale-to-zero reduces the time the application is actively running when nobody is using it. This lowers exposure because there are fewer active instances available for attackers to target. It also reduces unnecessary resource use and limits the running attack surface.

### Q4: What attack surface was eliminated by OIDC replacing SSH keys?

OIDC removed the need to store long-lived SSH private keys in GitHub Secrets. This reduces the risk of stolen keys being reused by an attacker. The workflow now uses short-lived identity-based authentication instead of permanent credentials.
