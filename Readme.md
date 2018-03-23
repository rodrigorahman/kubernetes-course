# Curso Kubernets

##Exemplo de criação de um POD dentro do Kubernets:

##Para Trabalhar com o kubernets local você não precisa instalar o kubernets completo basta apenas baixar o projeto minikube 

>https://kubernetes.io/docs/tasks/tools/install-minikube/


##Instalando Minikube

```
#Instalar virtualbox

curl -Lo minikube https://github.com/kubernetes/minikube/releases/download/v0.25.0/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

### Menor Objeto dentro do kubernets é um POD

##Adicionando o minikube no cluster

```
kubectl config use-context minikube
```

**Primeiro crie um arquivo yaml da sua aplicação ex:**

```YAML
# aplicacao.yaml
apiVersion: v1
kind: Pod
metadata:
  name: aplicacao
spec:
  containers: 
    - name: container-aplicacao-loja
      image: Caminho da imagen dentro do docker hub
      ports: 
        - containerPort: 80
```

##Agora para adicionar esse pod no Kubernets instale o kubctl: 

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/darwin/amd64/kubectl

chmod +x ./kubectl

sudo mv ./kubectl /usr/local/bin/kubectl
```


##Adicionando um POD

```
kubectl create -f aplicacao.yaml
```

##Recuperando os PODs

```
kubectl get pods
```

##Recuperando os PODs

```
kubectl get pods
```

##Deletando um POD

```
kubectl delete pods aplicacao
```

#Gerenciando pods

Para gerenciar os pods precisamos abstrair o POD e criar um objeto deployment

Ex:

```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aplicacao-deployment
spec:
  selector:
    matchLabels:
      app: aplicacao
  replicas: 1
  template:
    metadata:
      labels:
        app: aplicacao
    spec:
      containers: 
      - name: container-aplicacao-loja
        image: rodrigorahman/php-server-projeto
        ports: 
          - containerPort: 80
```


**Adicionando objeto deployment no kubernets**

```
kubectl create -f <nome_do_arquivo>.yaml
```

## Pedindo para o cluster o ip do pod

```
kubectl describe pods | grep IP
```

## Abrindo o painel administrativo

```
minikube dashboard
```

## Services (Abstraindo acesso aos pods) (Load Balance)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aplicacao-service
spec:
  type: LoadBalancer
  ports:
    - port: 80
  selector:
    name: aplicacao-deployment
```

**Para adicionar no Kube rodar o comando**

```
kubectl create -f aplicacao-service.yaml
```

**Para recuperar a URL de acesso ao service (Load Balance)**
```
minikube service aplicacao-service --url
```

## Objetos Statefullset

Utilize quando queira que o algo seja persistido dentro do kubernet por ex: Bancos de dados;
Não podemos perder o banco de dados porque o pod caiu ou subiu um novo pod

