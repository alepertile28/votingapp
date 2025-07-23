# votingapp
#  VotingApp com Kubernetes, Helm, ArgoCD e GitHub Actions

Este projeto é uma adaptação da aplicação open source **Voting App**, com o objetivo de aprofundar meus conhecimentos em **Kubernetes**, **CI/CD**, **Helm Charts** e **ArgoCD**, utilizando uma infraestrutura de cluster local e self-hosted na AWS e Oracle Linux.

---

##  Objetivo

O objetivo principal foi **construir um ambiente de entrega contínua (CI/CD)** com boas práticas, utilizando ferramentas amplamente adotadas em ambientes DevOps e Cloud Native:

-  Empacotar serviços com **Helm**
-  Automatizar deploy com **GitHub Actions** + runner self-hosted
-  Gerenciar estado de cluster com **ArgoCD**
-  Expor os serviços com **Ingress + Cert-Manager + Let's Encrypt**
-  Garantir confiabilidade com **readiness/liveness probes** e rollback automático

---

## Stack utilizada

| Camada           | Ferramentas                                  |
|------------------|----------------------------------------------|
| Orquestração     | Kubernetes (kubeadm + containerd)            |
| Empacotamento    | Helm                                          |
| Deploy contínuo  | GitHub Actions (self-hosted runner)          |
| GitOps           | ArgoCD                                       |
| Observabilidade  | (em andamento)                               |
| Ingress/Proxy    | NGINX Ingress Controller                     |
| SSL              | cert-manager + Let's Encrypt                 |
| Hosting          | Oracle Linux 8.5 (on-prem) e EC2 Free Tier   |

---

##  Estrutura do projeto

```text
votingapp/
├── .github/
│   └── workflows/
│       └── deploy.yaml         # CI/CD com GitHub Actions
├── helm/
│   └── votingapp/              # Helm chart do projeto
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
└── README.md

O chart Helm define todos os componentes do VotingApp:

vote (frontend de votação)

result (frontend de resultados)

worker (processa votos)

redis (fila)

postgres (banco)

---

##  CI/CD com GitHub Actions

Ao fazer `git push`, o workflow é disparado automaticamente.

O runner self-hosted no cluster executa:

1. `helm upgrade --install` do chart
2. Aguarda o deploy com `--wait`
3. (futuro) Rollback automático em caso de falha

---

##  Como rodar localmente

**Pré-requisitos:**

- Kubernetes local (`k3s`, `minikube`, `kind`, etc.) ou cluster real
- `kubectl`, `helm`, `nginx-ingress` e `cert-manager` instalados

### 1. Clone o repositório

```bash
git clone https://github.com/alepertile28/votingapp
cd votingapp

### 2. Confiugre o Helm
helm upgrade --install votingapp ./helm/votingapp \
  --namespace votingapp-lab \
  --create-namespace \
  --wait \
  --timeout 120s

### 3. acesse a aplicação via ingress
    http://votelab.seudominio.com
    http://resultlab.seudominio.cm
os dominios são configuraveis no values.yaml

## 🧪 Boas práticas aplicadas

- ✅ Deploys idempotentes com `helm upgrade --install`
- ✅ Probes (`readiness` e `liveness`) usando fallback `exec`
- ✅ Infraestrutura declarativa com Helm
- ✅ Pipeline CI/CD com GitHub Actions
- ✅ Runner self-hosted dentro do cluster Kubernetes
- ✅ Cluster provisionado com `kubeadm` + `containerd`
- ✅ TLS automático com `cert-manager` + Let's Encrypt

---

## 🧠 O que aprendi com o projeto

- Configuração avançada de Helm Charts reutilizáveis
- Implantação de cluster Kubernetes com `kubeadm` e `containerd`
- Criação de pipeline CI/CD real conectando GitHub → Runner → Cluster
- Aplicação de GitOps com ArgoCD e versionamento de infraestrutura
- Ajuste fino de probes (`readiness`/`liveness`) e rollback automático
- Como manter uma stack Kubernetes funcional dentro do **free tier da AWS**

---

## 📌 Próximos passos

## 👨‍💻 Autor

**Alexandre Augusto Pertile da Luz**  
🔗 [LinkedIn](https://www.linkedin.com/in/alexandre-pertile-36a350102/)  
🐙 [GitHub: alepertile28](https://github.com/alepertile28)