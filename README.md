# Flask Helm Chart

Este repositório contém um Helm Chart para fazer o deploy da aplicação Flask contida no repositório [`flask-poc`](https://github.com/mateuzor/flask-poc).

---

## 🏗️ Estrutura

O Helm Chart está localizado dentro da pasta `flask-chart/`, criada com o comando `helm create flask-chart`.

---

## ⚙️ Requisitos

- Docker instalado
- [Minikube](https://minikube.sigs.k8s.io/) instalado e funcionando
- [Helm](https://helm.sh/) instalado
- [Kubectl](https://kubernetes.io/docs/tasks/tools/) configurado para o Minikube

---

## 🚀 Como rodar localmente com Minikube

### 1. Clonar o repositório (caso ainda não tenha)

```bash
cd ~/Projects
git clone https://github.com/mateuzor/flask-helm-chart.git
cd flask-helm-chart
```

### 2. Iniciar o Minikube

```bash
minikube start --driver=docker
```

### 3. Configurar o values.yaml

Abra `flask-chart/values.yaml` e certifique-se de que está assim:

- image.repository: flask-app
- image.tag: latest
- service.type: NodePort
- service.port: 5000
- autoscaling.enabled: false
- ingress.enabled: false
- serviceAccount.create: false

### 4. Build da imagem local

Rode no terminal **dentro do projeto `flask-poc`**:

```bash
cd ~/Projects/flask-poc
docker build -t flask-app:latest .
```

### 5. Instalar o Chart no cluster

Rode no terminal **dentro do projeto `flask-helm-chart`**:

```bash
cd ~/Projects/flask-helm-chart
helm install flask-app ./flask-chart
```

### 6. Verificar se foi criado corretamente

```bash
kubectl get all
```

### 7. Acessar a aplicação Flask

```bash
minikube service flask-app
```
<img width="835" height="346" alt="Screenshot 2025-07-30 at 9 22 10 AM" src="https://github.com/user-attachments/assets/fffabb93-2ace-40d7-b81c-51abfd66a997" />

Isso abrirá a URL local da aplicação Flask no navegador (via NodePort).

---

## 🔄 Para resetar

```bash
helm uninstall flask-app
```

---

## 🔗 Repositórios relacionados

- [flask-poc](https://github.com/mateuzor/flask-poc): aplicação Flask com pipeline Jenkins e testes automatizados.

---


## 📈 Deploy de Prometheus e Grafana no Minikube com Helm

Este guia documenta o processo para instalar **Prometheus** e **Grafana** em um cluster local via **Minikube**, usando o Helm.

---

## ✅ Pré-requisitos

Certifique-se de que você já possui:

- [Minikube](https://minikube.sigs.k8s.io/docs/) instalado e rodando
- [Helm](https://helm.sh/) instalado
- [kubectl](https://kubernetes.io/) configurado com o contexto do Minikube
- Internet ativa para baixar os charts

---

## 🚀 Passo a passo

### 1. Iniciar o Minikube (caso não esteja rodando)

```bash
minikube start --driver=docker
```

---

### 2. Criar o namespace `infra`

```bash
kubectl create namespace infra
```

---

### 3. Adicionar repositórios do Helm

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

---

### 4. Instalar Prometheus no namespace `infra`

```bash
helm install prometheus prometheus-community/prometheus \
  --namespace infra
```

---

### 5. Instalar Grafana no namespace `infra`

```bash
helm install grafana grafana/grafana \
  --namespace infra \
  --set adminPassword=admin \
  --set service.type=NodePort
```

> 📌 A senha do Grafana foi configurada como `admin` para facilitar o acesso local.

---

## 🔍 Verificar os recursos

### Ver pods e serviços

```bash
kubectl get all -n infra
```

---

## 🌐 Acessar o Grafana

### Obter a URL do serviço

```bash
minikube service grafana --namespace infra
```

Isso abrirá o Grafana no navegador. Use:

- **Usuário:** admin  
- **Senha:** admin (ou a que você definiu)

---

<img width="1464" height="1056" alt="Screenshot 2025-07-30 at 9 39 43 AM" src="https://github.com/user-attachments/assets/9b6e8f29-5be2-471f-bf70-f4166e2ba679" />


Pegar senha admin

```bash
kubectl get secret --namespace infra grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```


## 🧹 Remover tudo (opcional)

```bash
helm uninstall prometheus -n infra
helm uninstall grafana -n infra
kubectl delete namespace infra
```

---

## 📁 Estrutura usada

Nenhum repositório customizado foi necessário para Prometheus ou Grafana — os charts usados foram os oficiais da comunidade Helm.

---
