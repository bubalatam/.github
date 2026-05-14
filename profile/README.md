<h1 align="center">Bubalatam</h1>

<p align="center">
  <strong>Software para la industria de la construcción en México 🇲🇽</strong>
</p>

<p align="center">
  Plataforma B2B que digitaliza la operación de constructoras: presupuestos,
  obra, compras, contabilidad y reporting — todo en un solo lugar.
</p>

---

### Stack

<p>
  <img src="https://img.shields.io/badge/.NET_10-512BD4?style=flat&logo=dotnet&logoColor=white" alt=".NET 10" />
  <img src="https://img.shields.io/badge/C%23-239120?style=flat&logo=csharp&logoColor=white" alt="C#" />
  <img src="https://img.shields.io/badge/EF_Core-512BD4?style=flat&logo=dotnet&logoColor=white" alt="EF Core" />
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white" alt="PostgreSQL" />
  <img src="https://img.shields.io/badge/Angular_21-DD0031?style=flat&logo=angular&logoColor=white" alt="Angular 21" />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/Tailwind-06B6D4?style=flat&logo=tailwindcss&logoColor=white" alt="Tailwind" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white" alt="Docker" />
  <img src="https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazonaws&logoColor=white" alt="AWS" />
  <img src="https://img.shields.io/badge/Terraform-7B42BC?style=flat&logo=terraform&logoColor=white" alt="Terraform" />
  <img src="https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat&logo=githubactions&logoColor=white" alt="GitHub Actions" />
</p>

### Repos

| Repo | Stack | Descripción |
|---|---|---|
| [`api`](https://github.com/bubalatam/api) | .NET 10 · EF Core · Carter | Backend principal — APIs REST del CMS |
| [`web`](https://github.com/bubalatam/web) | Angular 21 · Tailwind · Fuse | SPA admin para constructoras |
| [`mobile`](https://github.com/bubalatam/mobile) | — | App móvil para residentes y supervisores en obra |
| [`infra`](https://github.com/bubalatam/infra) | Terraform · Docker Compose · AWS | IaC, IAM policies, stack de staging |
| [`.github`](https://github.com/bubalatam/.github) | GitHub Actions | Reusable workflows y defaults org-wide |
| [`docs`](https://github.com/bubalatam/docs) | Obsidian | Knowledge base interno (brand, dominio, runbooks) |

### Infraestructura

Despliegues automatizados vía [GitHub Actions reusable workflows](https://github.com/bubalatam/.github/blob/main/.github/workflows/deploy-service-staging.yml):
build → ECR → SSM → EC2 → health check. Secrets de runtime en AWS Secrets Manager.

---

<p align="center">
  <sub>Hecho con ❤️ desde México</sub>
</p>
