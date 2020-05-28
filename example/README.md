# EXEMPLO
<p>Neste exemplo, implantamos o controlador NGINX ou NGINX Plus Ingress, um aplicativo Web simples e, em 
seguida, configuramos o balanceamento de carga para esse aplicativo usando o recurso Ingress.</p>

## Executando o exemplo

## 1. Deployments

### 1.1. Deploy da aplicação e serviço Coffee
Nesta execução será criado o deployment do nginx e um serviço que respoderá na port 80 tcp.

```
kubectl apply -f deploy-coffee.yaml
```

Aguarde todos os pods estarem em execução. Você pode acompanhar com o comando:

```
kubectl get pods -w
```
ou
```
watch -n 1 'kubectl get pods'
```

### 1.2. Deploy da aplicação e serviço Tea

```
kubectl apply -f deploy-tea.yaml
```

Aguarde todos os pods estarem em execução. Você pode acompanhar com o comando:

```
kubectl get pods -w
```
ou
```
watch -n 1 'kubectl get pods'
```

## 2. Configuração do Load Balancer

Criando a secret para o Certificado SSL e a key:

```
kubectl apply -f cafe-secret.yaml
```

Criando o Ingress reource:
```
kubectl apply -f cafe-ingress.yaml
```

## 3. Fazendo os teste de acesso

Vou configurar o meu /etc/hosts para resolver o dns nginx.lab.local para o endereço IP do um dos NODEs Worker.
Ex:
```
[...]
192.168.1.21    nginx.lab.local
[...]
```
No navegador basta acessar e passar o path, caso você acesse o dominio padrão o erro 404 aparecerá.

### Acesso a aplicação coffee
```
curl -k https://nginx.lab.local/coffee
```
ou 
```
firefox https://nginx.lab.local/coffee
```

### Acesso a aplicação tea
```
curl -k https://nginx.lab.local/tea
```
ou
```
firefox https://nginx.lab.local/tea
```