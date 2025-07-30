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