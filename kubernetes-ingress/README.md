# INGRESS CONTROLLER NGINX

Para o Ingress Controller NGINX, use a imagem nginx/nginx-ingress do DockerHub.
Clone o repositório do Ingress Controller e mude para a pasta de implantações:

```bash
$ git clone https://github.com/nginxinc/ingress-controller/
$ cd ingress-controller/kubernetes-ingress
```

## 1. Configure o RBAC
-----------------------

Crie um namespace e uma conta de serviço para o Ingress Controller:

```bash
kubectl apply -f 00-ns-and-sa.yaml
```
Crie uma rule do cluster e integre a função de cluster para a conta de serviço:

```bash
$ kubectl apply -f 01-rbac.yaml
```

> Nota: Para executar esta etapa, você deve ser um administrador de cluster. Siga a documentação da sua plataforma Kubernetes para configurar o acesso de administrador.

## 2. Crie recursos comuns
-----------------------

Nesta seção, criamos recursos comuns para a maioria das instalações do Ingress Controller:

Crie uma secret com um certificado TLS e uma chave para o servidor padrão no NGINX:

```bash
$ kubectl apply -f 02-default-server-secret.yaml
```

> Nota: O servidor padrão retorna a página Não encontrado com o código de status 404 para todos os pedidos de domínios para os quais não há regras do ingress definidas. Para fins de teste, incluímos um certificado autoassinado e uma chave que geramos. No entanto, recomendamos que você use seu próprio certificado e chave.

Crie um mapa de configuração para personalizar a configuração do NGINX:

```bash
$ kubectl apply -f 03-nginx-config.yaml
```

Se você deseja usar os recursos de balanceamento de carga TCP e UDP do Ingress Controller, crie os seguintes recursos adicionais:

Crie uma definição de recurso customizado para o recurso GlobalConfiguration:

```bash
$ kubectl apply -f 04-gc-definition.yaml
```

Crie um recurso GlobalConfiguration:

```
$ kubectl apply -f 05-global-configuration.yaml
```

> Nota: Certifique-se de referenciar este recurso no argumento da linha de comandos -global-configuration.

## 3. Implante o Ingress Controller
------------------------------------

Existe duas formas para implantar o Ingress Controller:

Deployment. Use um deployment se você planeja alterar dinamicamente o número de réplicas do Ingress Controller.
DaemonSet. Use um DaemonSet para implantar o controlador Ingress em cada nó ou subconjunto de nós.

Antes de criar um recurso Deployment ou Daemonset, atualize os argumentos da linha de comandos do contêiner do Ingress Controller no arquivo de manifesto correspondente, de acordo com seus requisitos.
Neste documento utilizo apenas o DaemonSet.

### 3.1 Execute o Ingress Controller
Para NGINX, execute:

```bash
$ kubectl apply -f 06-daemon-set-ingress.yaml
```

Use um DaemonSet: Quando você executa o Ingress Controller usando um DaemonSet, o Kubernetes cria um pod do controlador Ingress em todos os nós do cluster.

> Consulte também: Consulte os documentos do Kubernetes DaemonSet para saber como executar o controlador Ingress em um subconjunto de nós, em vez de em todos os nós do cluster.

### 3.2 Verifique se o Ingress Controller está em execução
Execute o seguinte comando para garantir que os pods do Ingress Controller estejam em execução:

```
$ kubectl get pods -n nginx-ingress
```

## 4. Criando aplicação de Exemplo

Para criar uma aplicação e validar as configurações siga os passos descrito [aqui](../example/README.md).