# Jaehyun Lee (이재현)

**Cloud & DevOps Engineer · 42 Gyeongsan**

> 안녕하세요, 42 경산에서 공부하며 멀티클라우드 인프라를 Terraform으로 설계·운영하는 이재현입니다.

## About

I'm a 42 Gyeongsan student (cohort 2) who builds and runs infrastructure across GCP and AWS with Terraform.
What I enjoy most is making operations **safe to get wrong**: CI pipelines with gated applies, keyless deploy identity, and rehearsed disaster recovery.
I split implementation work across AI coding agents, but I make the design calls and review and merge every change myself. Every live change goes through an explicit approval gate.

## Featured work

### lvz-matrix-infra: self-hosted Matrix homeserver and LLM agent on multi-cloud
*Private repository, demo on request · 2026.06 – present*

`Terraform` `GCP GKE Autopilot` `AWS ECS Fargate` `wasmCloud` `NATS` `Cloudflare Tunnel` `Buildkite` `Rust` `Bash`

- Migrating a Matrix homeserver from AWS ECS to wasmCloud (Rust → WASM components) on GKE Autopilot in a **zero-downtime dual-run**, with the existing service kept up throughout.
- **Disaster recovery:** a primary cluster in us-west1 plus a cold standby in us-central1 (0 replicas at rest), all in Terraform. The failover script has per-step RTO checkpoints and write-lease fencing. In a drill, against a 15-minute RTO target, it **failed over in about 3 min and failed back in about 2 min**.
- When drills failed (a DR API timeout, a missing cross-region lease promotion), I traced the causes and added read-only retries and an approved promotion step. Writes and scaling are never retried. The rerun passed.
- **CI/CD:** separate Buildkite pipelines for the PR check (`terraform fmt/validate/test`, `cargo test`), a main-only plan, an apply behind a manual block step, and WASM image publishing. About 40 Terraform test files plus shell-script tests catch regressions.
- **Security:** Workload Identity Federation lets CI reach GCP and AWS with no long-lived keys. Plan and apply roles are split, and apply stays disabled by default. Secrets live only in Secret Manager / Secrets Manager, never in git, logs or argv. Also: NATS account auth, digest-pinned images, and public exposure through Cloudflare Tunnel with no inbound ports.
- Runs an LLM coding agent ([Lavoisier](https://github.com/DavidLee18/lavoisier)) as an always-on container, with a model fallback chain and an EFS state volume that's backed up daily.
- Deployed an OAuth service for Element X (matrix-auth) as a WASM component. Tore down an unused GKE cluster on a plan to cut cost.
- **How it's run:** 680+ commits, about 190 merges. AI agents do the implementation; I own design, review, merge and the GO for each live step. Every live change follows plan review → approval → staged run → stop at the first failure.

### [Inception](https://github.com/DavidLee18/42_inception): Docker service infrastructure (42)
- Nine Alpine-based images built from scratch: nginx as the TLS 1.3-only entry point, WordPress, MariaDB, Redis, FTP, and Prometheus + Grafana monitoring. Orchestrated with Docker Compose.
- 13 credentials injected through Docker Secrets, never as environment variables. VM provisioning automated with Vagrant + Ansible.

### [webserv](https://github.com/DavidLee18/42_webserv): non-blocking HTTP/1.1 server in C++98 (42, team of 3)
- A single-threaded Linux `epoll` (edge-triggered) event loop, a config file format we designed ourselves, GET/POST/DELETE, and CGI/1.1 execution with timeout (504) handling.
- My part: the config grammar design and the CGI / WSGI implementation.

### Developer tooling for 42 students (Rust)
- [norminette_lsp](https://github.com/DavidLee18/norminette_lsp): a language server for the 42 Norminette style checker.
- [zed_norminette](https://github.com/DavidLee18/zed_norminette): a Zed extension for in-editor Norminette linting.
- [inject_42h](https://github.com/DavidLee18/inject_42h): a CLI that injects the 42 header into a file.

## Tech stack

- **IaC · CI/CD:** Terraform (S3 remote state + DynamoDB lock, `terraform test`), Buildkite, Git PR review flow
- **Cloud:** GCP (GKE Autopilot, Artifact Registry, Firestore, Secret Manager, Workload Identity Federation) · AWS (ECS Fargate, EFS, ALB, Route 53, IAM OIDC, Secrets Manager, AWS Backup) · Cloudflare Tunnel
- **Containers:** Kubernetes, Helm, wasmCloud / WASM components, NATS, Docker, Podman, Docker Compose, Vagrant, Ansible
- **Languages:** Rust, Bash, C, C++, Python, Haskell
- **Systems · networking:** Linux, HTTP/1.1, TLS, epoll event loops, processes and signals, Prometheus / Grafana

## Background

- **42 Gyeongsan**, core curriculum, cohort 2 (2024.10 – present): C (libft, pipex, push_swap, philosophers, minishell), C++ modules 00–09, webserv, Inception
- **Itean**, full-stack developer intern (2021.08 – 2021.11): the NaaSS web service (Vue front end + back end)
- **Gachon University**, Computer Engineering (left before graduating)

## Contact

- Email: [djwodus@gmail.com](mailto:djwodus@gmail.com)
- LinkedIn: [linkedin.com/in/jaehyun-lee-94b65333b](https://www.linkedin.com/in/jaehyun-lee-94b65333b)
- Blog: [davidlee18.github.io](https://davidlee18.github.io)
