**OpenShift Reference Architecture — Enterprise IaC & CI/CD**

A concise, production-ready reference architecture and Infrastructure-as-Code (IaC) to provision, configure, and operate a Red Hat OpenShift cluster from base OS to CI/CD and observability.

**Executive Summary**
- **Problem:** Many enterprise deployments lack a repeatable, documented OpenShift architecture and automation.
- **Goal:** Provide a tested reference architecture, Ansible playbooks, pipeline templates, and observability patterns to enable consistent, auditable deployments.

**Table of contents**
- **Overview**
- **Technology stack**
- **Quick start**
- **Repository layout**
- **How to run**
- **Status**
- **Contributing & support**

**Technology stack (high level)**
- Container orchestration: Red Hat OpenShift / Kubernetes
- IaC & automation: Ansible playbooks
- Base OS: RHEL / Rocky Linux 9 / Fedora
- Container runtime & registry: Podman, Red Hat Quay
- CI/CD: Tekton (OpenShift Pipelines)
- Observability: Prometheus, Loki, Grafana

**Quick start (developer / operator)**
1. Install prerequisites: Git, Ansible, Python 3, SSH access to target hosts.
2. Clone the repo and inspect inventory and playbooks.

```bash
git clone <repo-url>
cd openshift-reference-architecture
ls -la
```

3. Edit inventory in [inventory/](inventory/) to match your environment.
4. Run the base OS playbook (example):

```bash
ansible-playbook -i inventory/hosts.ini playbooks/cluster-setup/01-base-os.yaml -u <ssh-user> --become
```

Notes:
- Use `--check` to dry-run where supported.
- Add `-e @secrets.yml` or vault-encrypted vars for credentials.

**Repository layout**
- [docs/](docs/) — architecture diagrams and design notes (Mermaid diagrams render on GitHub).
- [playbooks/](playbooks/) — Ansible playbooks and roles.
	- [playbooks/cluster-setup/](playbooks/cluster-setup/) — base OS and cluster bootstrapping.
	- [playbooks/cicd/](playbooks/cicd/) — Tekton pipeline templates and automation.
	- [playbooks/monitoring/](playbooks/monitoring/) — Prometheus, Loki, Grafana configs.
- [inventory/](inventory/) — example host inventories and group variables.

**How to run (recommendations)**
- Validate inventory and connectivity: `ansible -i inventory/hosts.ini all -m ping`
- Start with node bootstrap tasks in `playbooks/cluster-setup/`.
- Use a dedicated control machine with Ansible and required credentials.

Example commands

```bash
# Validate hosts
ansible -i inventory/hosts.ini all -m ping

# Dry-run a playbook
ansible-playbook -i inventory/hosts.ini playbooks/cluster-setup/01-base-os.yaml --check

# Apply changes
ansible-playbook -i inventory/hosts.ini playbooks/cluster-setup/01-base-os.yaml -u core --become
```

**Status & roadmap**
- Phase 1 — Architecture & design: ✅ diagrams and design docs in [docs/](docs/)
- Phase 2 — IaC: ⏳ core playbooks under [playbooks/cluster-setup/](playbooks/cluster-setup/) (documentation & tests in progress)
- Phase 3 — CI/CD & registry: planned — Tekton templates and Quay modules
- Phase 4 — Day-2 ops: planned — monitoring, alerting, backup/DR runbooks

**Contributing**
- Edit or add playbooks under `playbooks/` and update `inventory/` samples.
- Open issues for bugs, feature requests, or runbook gaps.
- Create PRs with clear descriptions and test steps.

**Support & contact**
- For questions, open an issue or contact the maintainers in the repository.

**License**
- See the repository `LICENSE` file if present.

---
If you'd like, I can also:
- add a minimal `CONTRIBUTING.md` and `ISSUE_TEMPLATE.md`;
- add example `secrets.yml` + Ansible Vault instructions;
- or run a quick lint of the playbooks.
