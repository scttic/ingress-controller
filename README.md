# Kubernetes Ingress

<p>Siga os passos aqui descrito para a implementação do Ingress Controller no cluster kubernetes</p>
<p>Neste exemplo vou fazer a instalação do Ingress controller em um cluster kubernetes com um Node Master e dois Nodes Worker</p>
<p>Como teste vou fazer implementar uma aplicação que utilizará um load balancer para os nodes worker e as replicas dos PODS</p>

## 1. Kubernetes Ingress Nginx

Siga os passos deste documento para implementar o Ingress Controller. [Clique aqui](kubernetes-ingress/README.md).

## 2. Aplicação de exemplo

Para fazer os teste na instalação do Ingress Controller implemente uma aplicação com Ingress Resource, siga estes passoa para implementar uma aplicação de teste. [Clique aqui](example/README.md).